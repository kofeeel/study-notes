---
title: ProjectKD 코드 해설 02 — 적 AI 직교 모델 (토큰/EQS/Crowd + SelectAttack)
tags:
  - ProjectKD
  - portfolio
  - code-study
  - enemy-ai
  - behavior-tree
created: 2026-06-15
status: 학습용 v1
related:
  - "[[ProjectKD-코드해설-01-WeaponTrace패링]]"
  - "[[ProjectKD-담당작업-시스템정리]]"
  - "[[ai-eqs-crowd-eqs]]"
---

# 코드 해설 02 — 적 AI 직교 모델

> **목적**: "AI를 어떻게 설계했냐"에 코드로 답하기. 시프트업 디자이너 자격요건 "AI 제작"에 직결.
> 줄 번호는 실제 파일 기준(2026-06-15). 옆에 파일 열고 대조.

**관련 파일**
- `Source/Project_KD/Enemy/AI/EncounterSubsystem.cpp` — 토큰("누가 칠지")
- `Source/Project_KD/Enemy/AI/Service/BTService_RequestAttackToken.cpp` — 토큰 요청 + 원거리 면제
- `Source/Project_KD/Enemy/AI/Task/BTTask_SelectAttack.cpp` — 데이터드리븐 공격 선택
- `Source/Project_KD/Enemy/AI/Service/BTService_FindPlayer.cpp` — 타겟 미러 + facing
- `Source/Project_KD/Enemy/KDEnemyAIController.cpp` — 인지 + facing + Crowd 회피

---

## 1. 큰 그림 — 직교 3축 (이게 설계의 핵심)

세 시스템이 **서로 안 겹친다.** 각자 다른 질문에 답하고, 한 축을 바꿔도 나머지에 영향 없음.

```
토큰 (EncounterSubsystem)   = "누가 칠지"        동시 공격자 ≤2, 2.5s 로테이션
EQS                         = "어디 설지/접근할지"  Donut 제너레이터 + 빌트인 테스트
MoveTo + Detour Crowd       = "그 목적지까지 경로 + 국소 회피"
```

**왜 직교가 중요한가** — 적 AI가 god-tree(거대한 분기 트리)가 되는 걸 막는다. "근접이면서 토큰 있고 사거리 안이고 LOS 있고…" 같은 중첩 분기 대신, 각 축이 독립적으로 BB(Blackboard) 키 하나만 책임진다. 새 포지셔닝 로직을 추가할 때 "이게 3축 중 어디 소관인지"부터 분류.

**그리고 공유 BT 1개 + 데이터드리븐.** 적별로 BT를 따로 안 만든다. BT 골격은 무분기, 다양성은 **적 정의 DataAsset의 공격 엔트리**에서 나온다(아래 §3).

---

## 2. 토큰 — "누가 칠지" (EncounterSubsystem)

> 동시에 너무 많은 적이 달려들면 불공평하고 정신없다 → 동시 공격자 수를 제한. 스텔라 블레이드/귀무자식 패턴.

`EncounterSubsystem`은 **WorldSubsystem** — 월드에 1개 존재하는 서비스. 싱글톤 금지 규칙(CLAUDE.md §1)의 예외가 Subsystem이고, 이게 그 정당한 용례야.

### CVar로 튜닝 노브 노출 (cpp:9~17)
```cpp
static int32 GKDMaxAttackers = 2;
static FAutoConsoleVariableRef CVarKDMaxAttackers(TEXT("kd.Encounter.MaxAttackers"), ...); // 동시 공격 상한
static float GKDTokenHoldTime = 2.5f;  // 토큰 강제 회수 시각
```
→ 콘솔에서 `kd.Encounter.MaxAttackers 3` 치면 런타임에 바뀜. 밸런싱 값을 코드 재컴파일 없이 만진다.

### RequestToken (cpp:19~59) — 토큰 발급 로직
```cpp
TokenHolders.RemoveAll([](const FTokenSlot& Slot){ return !Slot.Holder.IsValid(); }); // 24: 죽은 holder 청소
for (...) if (Slot.Holder.Get() == Enemy) return true;          // 27: 이미 보유면 유지
if (TokenHolders.Num() < GKDMaxAttackers) { ...Add...; return true; } // 35: 빈 슬롯 → 즉시 발급
// 41~56: 꽉 참 → hold-time(2.5s) 초과 보유자 중 가장 오래된 1명 회수 후 발급 (로테이션)
return false;  // 58: 슬롯 꽉 차고 아무도 hold-time 미초과 → 대기
```
- **`Holder`는 weak 포인터** (`Slot.Holder.IsValid()`) — 적이 죽으면 자동으로 null이 돼서, 죽은 적이 슬롯을 영원히 잡는 걸 막음. (강한 포인터면 죽은 적을 붙들어 GC도 안 되고 슬롯도 안 풀림)
- **로테이션** — 2.5초 넘게 토큰 쥔 놈을 뺏어 다른 적에게. 그래서 "항상 같은 2명만 친다"가 아니라 돌아가며 침.

> **토큰 반납은 어디서?** → 해설 01에서 본 `GA_EnemyWeaponTraceBase::OnCleanup`(cpp:57~63)이 공격 끝나면 `ReturnToken`. 이게 안 되면 "공격 후에도 슬롯 점유 → 다음 적이 영원히 대기"하는 공격 지연 버그가 났었음.

---

## 3. SelectAttack — 데이터드리븐 공격 선택 (BTTask_SelectAttack)

> "어떤 공격을 쓸지"를 BT 분기가 아니라 **데이터(거리밴드 + 가중치)** 로 푼다. 공유 BT 무분기의 핵심.

`ExecuteTask` (cpp:35~108):

### 1단계 — 거리 계산 (cpp:49)
```cpp
const float Dist = FVector::Dist2D(SelfPawn->GetActorLocation(), Target->GetActorLocation());
```
`Dist2D` = 높이 무시 수평 거리. 공격 사거리는 평면 기준이라.

### 2단계 — 후보 수집 (cpp:52~78)
```cpp
const TArray<FEnemyAttackEntry>& Entries = SelfPawn->GetAttackEntries(); // DataAsset의 공격 목록
for (const FEnemyAttackEntry& Entry : Entries)
{
    if (Dist < Entry.MinRange || Dist > Entry.MaxRange) continue;  // 58: 거리밴드 밖이면 탈락
    // 60~73: 이 어빌리티가 지금 발동 가능한가? (쿨다운/코스트/차단태그)
    ASC->GetActivatableGameplayAbilitySpecsByAllMatchingTags(Query, Specs);
    if (Spec->Ability->CanActivateAbility(...)) bCanActivate = true;
    if (!bCanActivate) continue;
    Candidates.Add(&Entry);
    TotalWeight += FMath::Max(Entry.Weight, 0.f);
}
```
- **각 공격 엔트리** = `{AbilityTag, MinRange, MaxRange, Weight}`. 적별 DataAsset에 정의.
- 거리밴드 안 + 쿨다운 OK인 것만 후보. **쿨다운 판정을 직접 안 하고 `CanActivateAbility`에 위임** — GAS가 GE로 쿨다운을 관리하니 그걸 그대로 신뢰.
- 예: 검사 = {약공 0~150, 강공 100~200, 돌진 300~600}. 거리에 따라 자동으로 맞는 공격만 후보가 됨.

### 3단계 — 가중 랜덤 선택 (cpp:86~104)
```cpp
float Pick = FMath::FRandRange(0.f, TotalWeight);
for (const FEnemyAttackEntry* Cand : Candidates) {
    Pick -= FMath::Max(Cand->Weight, 0.f);
    if (Pick <= 0.f) { Chosen = Cand; break; }
}
```
→ **가중 랜덤의 정석 알고리즘.** 가중치 합 범위에서 난수 하나 뽑고, 누적하며 빼다가 0 이하 되는 지점이 당첨. 가중치 클수록 구간이 넓어 더 자주 뽑힘. (Weight 합이 0이면 균등 랜덤 폴백, cpp:101)

### 4단계 — 결과를 BB에 기록 (cpp:107)
```cpp
BB->SetValueAsName(SelectedAbilityTagNameKey.SelectedKeyName, Chosen->AbilityTag.GetTagName());
return EBTNodeResult::Succeeded;
```
→ 고른 어빌리티 태그를 Blackboard에 씀. 그 다음 BT 노드(`BTTask_ActivateAbilityByTag`)가 읽어서 발동. **후보 0이면 `Failed`(cpp:81~84) → BT가 reposition(포지셔닝) 분기로 빠짐.**

> **다양성을 코드가 아니라 데이터로** — 새 적을 만들 때 코드 0줄. DataAsset에 공격 엔트리 세트만 다르게 채우면 "다른 성격의 적"이 된다. 이게 "공유 BT 골격 + 데이터드리븐"의 실체.

---

## 4. 원거리 면제 — 토큰은 근접 전용 (BTService_RequestAttackToken)

> 직교 모델에서 토큰 축은 **사실상 근접 전용**. 이걸 원거리에 적용하면 버그가 난다.

`TickNode` (cpp:35~85)의 핵심 분기:

```cpp
// 51~56: 원거리 kiter(StandoffRange>0)는 토큰 면제
if (Enemy->GetStandoffRange() > 0.f) {
    BB->SetValueAsBool(HasTokenKey, true);          // 항상 공격 허용
    BB->SetValueAsBool(ShouldRepositionKey, false);
    return;
}
```
**왜?** 토큰은 "동시 근접 공격자 ≤2" 자원인데, 활은 다수가 동시에 쏴도 됨. 토큰을 적용하면 토큰 못 받은 활이 reposition/chase로 빠져 플레이어한테 비빔(근접처럼 굴게 됨). 그래서 원거리는 토큰 강제 부여 + reposition 차단 → "사거리 안이면 멈춰 사격". 거리 관리는 kiting 노드가 따로 함.

```cpp
// 65~79: 근접 — 교전거리(EngagementRange) 기준으로 토큰 경쟁
const float EngagementRange = Enemy->GetEngagementRange();
if (DistToPlayer > EngagementRange) {   // 교전거리 밖 → 토큰 반납, 추격만
    Encounter->ReturnToken(Enemy);
    BB->SetValueAsBool(HasTokenKey, false);
    return;
}
// 82~84: 교전거리 안 → 토큰 요청. 보유=공격 진입 / 미보유=포위(reposition)
const bool bHasToken = Encounter->RequestToken(Enemy);
BB->SetValueAsBool(HasTokenKey, bHasToken);
BB->SetValueAsBool(ShouldRepositionKey, !bHasToken);
```
- **교전거리(EngagementRange) vs 공격거리(AttackRange)** — 토큰 경쟁을 *넓은* 교전거리부터 시작. 이전엔 좁은 공격거리 안에서만 토큰을 켜서 "플레이어가 적 코앞에 가야 포위 시작"하는 버그가 있었음. 이제 멀리서부터 부채꼴로 흩어져 둘러싸고 일부만 치고 들어옴.

---

## 5. 타겟 미러 + facing — 뒷걸음 버그 (BTService_FindPlayer + UpdateCombatFacing)

> 해설 01이 "전투 판정"이면 이건 "전투 자세". facing 버그는 사례집 #5.

### BTService_FindPlayer (cpp:31~81) — 인지 결과를 BB에 미러
```cpp
AActor* Target = AICon->GetCurrentTarget();   // 44: 인지 판정은 controller가 함, 여기선 미러만
...
BB->SetValueAsObject(TargetActorKey, Target);
BB->SetValueAsBool(CanAttackKey, Dist <= AttackRange && bHasLOS);  // 77: 사거리 안 + 시야 확보
AICon->UpdateCombatFacing(Target, Dist);       // 80: facing 갱신
```
- **`bCanAttack` = 사거리 안 + LOS(벽 안 막힘).** 벽 너머 타격 방지(cpp:62~74). 이 bool이 BT chase/attack 분기를 가른다.
- 인지(시야 콘/피격/기억)는 **controller가 판정**하고, 이 서비스는 BB로 **미러만** 한다 — 관심사 분리.

### UpdateCombatFacing (cpp:173~191) — facing의 핵심
```cpp
const float EngageDist = FMath::Max(Enemy->GetAttackRange(), Enemy->GetStandoffRange()) * 1.3f;
if (Target && DistToTarget <= EngageDist) {
    SetFocus(Target);          // 교전거리 안 → 플레이어 바라봄(공격/사격 자세)
    SetCombatFacing(true);
} else {
    ClearFocus(EAIFocusPriority::Gameplay);  // 밖 → 풀어서 가는 방향을 봄
    SetCombatFacing(false);
}
```
- **버그였던 것**: `FindPlayer`가 인지 중이면 거리 무관 **매 틱 `SetFocus`** → 멀리서 home으로 복귀할 때도 플레이어를 계속 바라봄 → 이동방향(home)과 facing이 반대 = **뒷걸음**.
- **해결**: facing에 교전거리 게이트(`×1.3`). 안이면 focus, 밖이면 ClearFocus. 튜닝 노브 = `EngageDist ×1.3`.

### SetCombatFacing (cpp:152~171) — 회전 모드 스왑
```cpp
if (Enemy->IsDead() || Enemy->IsStaggered()) return;  // 163: 경직/사망 중 facing 동결
Move->bUseControllerDesiredRotation = bCombat;   // 전투 = 컨트롤러 desired로 부드럽게 facing
Move->bOrientRotationToMovement = !bCombat;      // 비전투 = 이동방향
```
- 전투/비전투에서 **CMC 회전 모드를 토글**. 전투 땐 타겟을 부드럽게 바라보고(desired rotation), 비전투 땐 가는 방향을 봄(orient to movement).
- **경직/사망 early-return** (163) — 경직 중인데 perception 자극이 `SetCombatFacing(true)`로 yaw를 되살리면 경직된 몸이 플레이어 따라 도는 버그(사례집/메모리). 그래서 동결.

---

## 6. EQS / Crowd (직교의 나머지 두 축, 개념)

- **Crowd 회피** — `KDEnemyAIController` 생성자(cpp:31)에서 `PathFollowingComponent`를 `UCrowdFollowingComponent`로 교체. MoveTo 실행 중 적끼리 겹치지 않게 국소 회피(RVO). NavMesh 필요.
- **EQS** — "어디 설지"를 고르는 쿼리. Donut 제너레이터(중심=플레이어) + 빌트인 테스트(Distance→Trace→Pathfinding). melee 포위는 360° 도넛 + **Score Only** 트레이스(보이는 자리 *선호*지 금지 아님). 커스텀 C++는 `EnvQueryContext_Target`(BB 타겟 읽기) 정도. → 상세 [[ai-eqs-crowd-eqs]]

---

## 7. 예상 면접 Q&A

**Q1. 적 AI를 어떻게 구조화했나요?**
> 세 가지 직교한 축으로 분리했습니다. 토큰(누가 칠지), EQS(어디 설지), MoveTo+Crowd(어떻게 갈지). 각 축이 Blackboard 키 하나만 책임지고 서로 안 겹쳐서, 거대한 분기 트리 없이 BT가 단순하게 유지됩니다. 새 포지셔닝 로직은 "이게 어느 축 소관인지"부터 분류하고요.

**Q2. 적마다 BT를 따로 만들었나요?**
> 아니요, 공유 BT 한 개에 적별 DataAsset만 다르게 붙입니다. 공격 선택을 BT 분기가 아니라 데이터로 풀었거든요. SelectAttack 노드가 거리밴드 안에서 발동 가능한 공격을 모아 가중 랜덤으로 고릅니다. 새 적은 코드 0줄, 공격 엔트리 세트만 다르게 채우면 다른 성격이 됩니다.

**Q3. SelectAttack의 후보 선정 기준은?**
> 두 조건입니다. 거리밴드(MinRange~MaxRange) 안에 있고, 지금 발동 가능한가(쿨다운/코스트). 쿨다운은 직접 판정 안 하고 GAS의 CanActivateAbility에 위임합니다. 통과한 후보를 가중치 기반 랜덤으로 고르고, 후보가 0이면 Failed를 반환해 BT가 reposition 분기로 빠집니다.

**Q4. 토큰을 왜 원거리엔 안 쓰나요?**
> 토큰은 "동시 근접 공격자 수 제한" 자원입니다. 활은 다수가 동시에 쏴도 되니까 토큰을 적용하면 토큰 못 받은 활이 chase로 빠져 근접처럼 비비게 됩니다. 그래서 StandoffRange가 있는 원거리는 토큰을 강제 부여하고 reposition을 막아, 사거리 안이면 멈춰 쏘게 했습니다. 거리 관리는 kiting 노드가 따로 하고요.

**Q5. 적이 뒷걸음치던 버그는?**
> FindPlayer 서비스가 인지 중이면 거리에 상관없이 매 틱 SetFocus를 호출해서, 멀리 home으로 복귀할 때도 플레이어를 계속 바라봤습니다. 이동 방향과 정반대를 보니 뒷걸음이 된 거죠. UpdateCombatFacing에 교전거리 게이트를 넣어서, 그 안에서만 focus하고 밖이면 ClearFocus해서 가는 방향을 보게 고쳤습니다.

**Q6. WorldSubsystem을 쓴 이유? 싱글톤 아닌가요?**
> EncounterSubsystem은 월드당 하나 존재하는 토큰 중재자입니다. 프로젝트 규칙이 싱글톤을 금지하지만 UE Subsystem은 예외고, 이게 정당한 용례입니다. 엔진이 수명주기를 관리하고, 전역 가변 상태를 자기 멤버로 캡슐화하니까요. holder는 weak 포인터라 적이 죽으면 슬롯이 자동으로 풀립니다.

---

## 8. 직접 설명 연습 (점검)

보지 말고 소리내어:

1. 직교 3축이 각각 무슨 질문에 답하는지 + 왜 직교가 중요한지
2. SelectAttack이 공격을 고르는 4단계 (거리 → 후보 → 가중랜덤 → BB기록)
3. "공유 BT + 데이터드리븐"으로 새 적을 코드 0줄로 만든다는 게 무슨 뜻인지
4. 원거리가 토큰을 면제받는 이유
5. 뒷걸음 버그의 원인(매 틱 SetFocus)과 해결(교전거리 게이트)

> 5개 막힘없으면 이 시스템도 "네 것". 막히면 해당 섹션 다시.

---

## 다음 해설 예정
- `03` — 사망/처형/데스블로 사이클 (AS_Combat 사망 동기 + 컴포넌트 분리)
