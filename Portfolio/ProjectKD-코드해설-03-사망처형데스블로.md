---
title: ProjectKD 코드 해설 03 — 사망 / 처형 / 데스블로 사이클
tags:
  - ProjectKD
  - portfolio
  - code-study
  - gas
  - execution
  - component
created: 2026-06-15
status: 학습용 v1
related:
  - "[[ProjectKD-코드해설-01-WeaponTrace패링]]"
  - "[[ProjectKD-코드해설-02-적AI직교모델]]"
  - "[[ragdoll-death-montage-decision]]"
---

# 코드 해설 03 — 사망 / 처형 / 데스블로 사이클

> **목적**: 처형 메커닉(스텔라 블레이드식)을 코드로 설명. 컴포넌트 분리 + GAS 동기 사망 + 데이터드리븐 분기.
> 줄 번호는 실제 파일 기준(2026-06-15).

**관련 파일**
- `Source/Project_KD/Enemy/KDEnemyBaseCharacter.cpp` — 사망/랙돌/오케스트레이션
- `Source/Project_KD/Combat/StaggerComponent.cpp` — 경직 상태머신
- `Source/Project_KD/Combat/ExecutionComponent.cpp` — 처형 사이클
- `Source/Project_KD/Combat/ExecutionProfile.h` — 처형 정의(DataAsset)
- `AS_Combat.cpp` — Health 차감(해설 01에서 본 곳)

---

## 1. 큰 그림 — Poise가 깨지면 처형까지 가는 사슬

```
[플레이어가 적을 때림 — 해설 01의 OnWeaponHit → Event.Combat.Hit]
   │
   ▼  KDEnemyBaseCharacter::OnHitReceived  (poise 차감)
Poise -= PoiseDamage  →  0 도달
   │
   ▼  StaggerComponent::OnPoiseChanged → BeginStagger
경직! State.Combat.Staggered GE 부여 + 자동복귀 타이머(StaggerDuration)
   - 적 공격 GA들이 ActivationBlockedTags로 자기차단 → 경직 중 공격 못 함
   - Pawn은 brain/이동 정지 (OnStaggerBegin 델리게이트)
   │
   ├─[타임아웃] ─────────────→ RecoverFromStagger → 기상 + Poise 풀리셋
   │
   └─[경직 중 강공 피격] ─────→ ExecutionComponent::HandleExecution  ★처형★
          │
          ▼  데스블로 판정 (시작 시점)
       bSurvivable=false(잡몹) → 항상 데스블로(즉사)
       bSurvivable=true(엘리트) → Health ≤ Threshold면 데스블로, 아니면 생존
          │
          ▼  OnExecutionBegin → Pawn이 처형 몽타주 재생 → 몽타주 끝
       FinishExecution
          ├─[데스블로] SetNumericAttributeBase(Health, 0) ──동기──→ HandleDeath → 랙돌
          └─[생존]     칩 데미지 → StaggerComponent 회복 + Poise 리셋 → 기상
```

**핵심 설계**: 처형은 **적-반응형**. 플레이어는 "경직 중 강공"을 트리거만 하고(0 터치), 판정·데미지·연출은 전부 적 측 컴포넌트가 소유.

---

## 2. 컴포넌트 분리 — God-class 방지 (CLAUDE.md §1)

`KDEnemyBaseCharacter` 생성자 (cpp:41~42):
```cpp
StaggerComp = CreateDefaultSubobject<UStaggerComponent>(TEXT("StaggerComp"));
ExecutionComp = CreateDefaultSubobject<UExecutionComponent>(TEXT("ExecutionComp"));
```
경직·처형을 Pawn에서 컴포넌트로 빼냄(417→~290줄). **통신은 델리게이트로만, 직접 포인터 금지**(§1-3).

`PossessedBy`의 배선 (cpp:118~132):
```cpp
StaggerComp->OnStaggerBegin.AddDynamic(this, &AKDEnemyBaseCharacter::OnStaggerBegin);
StaggerComp->OnStaggerRecovered.AddDynamic(this, &AKDEnemyBaseCharacter::OnStaggerRecovered);
ExecutionComp->OnExecutionBegin.AddDynamic(this, &AKDEnemyBaseCharacter::OnExecutionBegin);
// 컴포넌트 ↔ 컴포넌트도 직접 X — Pawn이 중개
ExecutionComp->OnExecutionResolved.AddDynamic(StaggerComp, &UStaggerComponent::HandleExecutionResolved);
```
- **컴포넌트→ASC 접근도 캐스팅 금지** — `UAbilitySystemBlueprintLibrary::GetAbilitySystemComponent(GetOwner())`로 인터페이스 경유(StaggerComponent.cpp:25, ExecutionComponent.cpp:27). concrete Pawn을 모르니 어느 캐릭터에든 붙일 수 있음.
- **AttributeSet은 순수 데이터** — 컴포넌트가 어트리뷰트 변화 델리게이트를 구독하고 *판단*은 자기가 함. (StaggerComp가 Poise 구독→경직 결정, cpp:30)

> 면접 포인트: "왜 컴포넌트로 뺐냐" = (1)Pawn 500줄 규칙 (2)단위 테스트/재사용 가능성 (3)관심사 분리. 직접 포인터 0개로 의존성을 단방향(Pawn→컴포넌트)으로 강제.

---

## 3. 경직 — StaggerComponent

### Poise 0 → 경직 (cpp:39~78)
```cpp
void OnPoiseChanged(...) {
    if (!IsStaggered() && Data.NewValue <= 0.0f) BeginStagger();   // 43
}
void BeginStagger() {
    OwnerASC->CancelAllAbilities();                    // 54: 진행 중 공격 몽타주 중단
    StaggerEffectHandle = ...ApplyGameplayEffectSpecToSelf(Stagger GE);  // 65: State.Combat.Staggered 부여
    GetWorld()->...SetTimer(StaggerTimeoutTimer, this, &OnStaggerTimeout, StaggerDuration); // 73: 자동복귀
    OnStaggerBegin.Broadcast();                        // 77: Pawn이 brain/이동 정지
}
```
- **`State.Combat.Staggered` GE** — 이 태그가 붙으면 적 공격 GA가 `ActivationBlockedTags`로 *자기* 발동을 막음. "경직 중 공격 불가"를 if 분기가 아니라 GAS 태그로.
- **IsStaggered()는 태그로 판정** (cpp:34~37) — 상태를 bool 멤버가 아니라 ASC 태그로 들고 있음. GE가 곧 진실의 원천.

### 타임아웃 vs 처형 — 누가 창을 소비하나 (cpp:80~86)
```cpp
void OnStaggerTimeout() {
    // 처형이 먼저 시작했으면(Invulnerable 태그) un-stagger 금지 — 처형이 복귀를 소유
    if (ASC->HasMatchingGameplayTag(State_Combat_Invulnerable)) { return; }
    RecoverFromStagger();
}
```
→ 타임아웃을 `RecoverFromStagger`에 직결 안 하고 **가드 함수로 우회.** 타임아웃이 처형 시네마틱 도중에 터져도 몸을 일으켜버리지 않게. 처형 중엔 `Invulnerable` 태그가 켜져 있으니 그걸로 구분.

### 회복 = Poise 풀리셋 (cpp:88~110)
```cpp
void RecoverFromStagger() {
    OwnerASC->RemoveActiveGameplayEffect(StaggerEffectHandle);  // 경직 해제
    OwnerASC->SetNumericAttributeBase(Poise, MaxPoise);         // 106: Poise를 Max로 리셋
    OnStaggerRecovered.Broadcast();
}
```
- **Model B: 시간 회복 없음.** Poise는 맞으면 깎이고 유지되다가, 경직 사이클이 끝날 때만 풀로 리셋. "0 → 처형 → 리셋"이 유일한 회복 경로(스텔라 블레이드식). 시간 regen은 경직 윈도우와 싸워서 아예 제거.

---

## 4. 처형 — ExecutionComponent

### 트리거 = 경직 중 강공 (cpp:36~50)
```cpp
void OnHitReceived(Payload) {
    if (bIsBeingExecuted || ...State_Dead) return;
    if (ASC->HasMatchingGameplayTag(State_Combat_Staggered)       // 경직 중 +
        && Payload->InstigatorTags.HasAny(ExecutionTriggerTags)) { // 강공 태그(Ability.Mugong.Execution)
        ExecutionInstigator = ...Payload->Instigator;
        HandleExecution();
    }
}
```
- 같은 `Event.Combat.Hit`를 Pawn(poise/넉백)과 ExecutionComp(처형)가 **각자 구독**. 경직 중이면 Pawn쪽은 early-return(중복 방지), 처형 트리거면 ExecutionComp가 발동.

### 데스블로 판정 = 시작 시점 (cpp:65~76)
```cpp
void HandleExecution() {
    bIsBeingExecuted = true;
    bDeathblow = true;
    if (ExecutionProfile->bSurvivable) {   // 엘리트만
        const float Health = ASC->GetNumericAttribute(GetHealthAttribute());
        bDeathblow = Health <= ExecutionProfile->DeathblowHealthThreshold;
    }
    OwnerASC->AddLooseGameplayTag(State_Combat_Invulnerable);  // 83: 시네마틱 보호막
    OnExecutionBegin.Broadcast();                              // 102: Pawn이 몽타주 재생
    ...SetTimer(ExecutionTimer, &FinishExecution, SafetyTime); // 113: 안전망
}
```
- **왜 시작 시점에 판정?** 데미지는 몽타주 *끝*에 적용되지만, 모션(다운→사망 vs 다운→기상)은 *시작*에 골라야 하니까. `bDeathblow`를 먼저 정해 `GetExecutionMontage()`가 맞는 몽타주를 반환.
- **`Invulnerable` 보호막** — 처형 시네마틱 도중 다른 데미지가 들어오는 걸 막음(GE_Damage_Physical이 Invulnerable 거부). loose tag라 ref-count.

### 결판 (cpp:117~166)
```cpp
void FinishExecution() {
    if (!bIsBeingExecuted) return;        // 119: 이중발동 가드 (몽타주끝 + 안전타이머)
    bIsBeingExecuted = false;
    OwnerASC->RemoveLooseGameplayTag(Invulnerable);   // 보호막 해제

    if (bDeathblow) {
        bResolvingExecution = true;       // 140: 래치 — 곧 동기 HandleDeath가 소비
        OwnerASC->SetNumericAttributeBase(Health, 0.f);  // 141: ★즉사 확정★
    } else if (ExecutionProfile->SurviveDamageEffectClass) {
        bResolvingExecution = true;
        OwnerASC->ApplyGameplayEffectSpecToSelf(칩 데미지);  // 생존 처형 = 칩만
    }

    const float Health = OwnerASC->GetNumericAttribute(GetHealthAttribute());
    const bool bSurvived = Health > 0.f;
    if (bSurvived) bResolvingExecution = false;
    OnExecutionResolved.Broadcast(bSurvived);   // 생존이면 StaggerComp가 기상
}
```
- **데스블로 = 데미지 GE가 아니라 코드로 Health=0 확정** (cpp:134~141). 왜? 데미지 GE 숫자 튜닝이 모자라면 "다운→사망 모션인데 살아서 AI 복귀"하는 깨진 상태가 남음. 즉사를 코드로 보장.

---

## 5. 동기 사망 — GAS의 진짜 핵심 ★

여기가 해설 01의 "Health 차감이 동기 발화"와 이어지는 지점.

```
ExecutionComponent::FinishExecution (cpp:141)
   SetNumericAttributeBase(Health, 0)
        │ ← 이 한 줄이 Health 변화 델리게이트를 "동기" 발화
        ▼ (같은 콜스택 안에서 바로)
KDEnemyBaseCharacter::OnHealthChanged (cpp:194)
        │
        ▼
HandleDeath()   ← FinishExecution이 아직 안 끝났는데 이게 먼저 다 돈다
```

`HandleDeath` 첫 줄 (cpp:202~208):
```cpp
void HandleDeath() {
    bIsDead = true;
    // ★ AbortForDeath가 상태를 지우기 전에 "처형 사망인지" 캡처
    const bool bExecutionDeath = ExecutionComp && ExecutionComp->IsExecutionDeath();
    ...
}
```
- **`bResolvingExecution` 래치** (ExecutionComponent.cpp:140) — `FinishExecution`이 `SetHealth(0)` *전에* 이 플래그를 켜둠. 그래야 동기로 끌려온 `HandleDeath`가 `IsExecutionDeath()=true`를 읽음. (그 후 `AbortForDeath`가 소비 후 해제, cpp:179)
- **왜 래치가 필요?** 사망이 동기라도, "이 사망이 처형 때문인가"를 HandleDeath가 알아야 죽음 몽타주를 스킵. 처형 모션이 곧 죽음 연출이니까.

> 면접 포인트: "SetNumericAttributeBase가 델리게이트를 동기 발화한다"를 알고 코드를 짰다는 게 GAS 깊이의 증거. 한때 "지연 사망" 가설을 세웠다가 로그(`bExecutionDeath=1`)로 동기임을 증명하고 폐기한 이터레이션.

---

## 6. 사망 연출 = 랙돌 하이브리드 (cpp:260~297)

```cpp
UAnimMontage* DeathMontage = EnemyDefinition ? EnemyDefinition->DeathMontage : nullptr;
if (!bExecutionDeath && DeathMontage) {           // 처형사망은 죽음몽타주 스킵
    if (Anim->Montage_Play(DeathMontage) > 0.f) {
        // 블렌드아웃 "시작"에 랙돌 인계
        Anim->Montage_SetBlendingOutDelegate(... OnDeathMontageEnded ...);  // 279
        // 백스톱 타이머 — 델리게이트가 영영 안 오면 동결 시체 방지
        ...SetTimer(BackstopTimer, &EnterRagdoll, length+0.5f);            // 284
    }
}
if (!bPlayedDeathMontage) EnterRagdoll();   // 미지정/실패 → 즉시 랙돌
```

`EnterRagdoll` (cpp:565~575):
```cpp
void EnterRagdoll() {
    if (!MeshComp || MeshComp->IsSimulatingPhysics()) return;   // 569: 멱등
    MeshComp->SetCollisionProfileName(TEXT("Ragdoll"));
    MeshComp->SetSimulatePhysics(true);
}
```

**세 가지 함정과 해결 (모두 코드 주석에 박제)**:
1. **블렌드아웃 시작 vs End** (cpp:275~279) — End(블렌드아웃 *완료* 후)면 그 사이 ABP가 idle로 되돌리는 블렌드가 끼어 시체가 반쯤 일어섰다 무너짐(포즈 팝). → 블렌드아웃 *시작*에 인계.
2. **멱등 EnterRagdoll** (cpp:567~569) — 블렌드아웃 델리게이트/백스톱 타이머/처형 경로가 중복 호출해도 `IsSimulatingPhysics()` 체크로 1회만.
3. **처형 사망 스킵** (cpp:264) — `bExecutionDeath`면 죽음 몽타주 안 틈(처형 피니셔가 곧 죽음 연출).

**그리고 코드 무죄** (cpp:571~572 주석): 랙돌이 몸을 꺾던 진범은 **PhysicsAsset**. 코드는 표준 `SetSimulatePhysics`만 함. → "또 꺾이면 PhysicsAsset/콜리전을 보지 코드 보지 말 것."

---

## 7. 데이터드리븐 — ExecutionProfile (OCP)

`ExecutionProfile.h` — 적 한 종류의 처형 정의(DataAsset):
```cpp
TObjectPtr<UAnimMontage> Montage;            // 생존 처형(다운→기상)
TObjectPtr<UAnimMontage> DeathblowMontage;   // 처치(다운→사망)
TSubclassOf<UGameplayEffect> SurviveDamageEffectClass;  // 생존 칩 데미지
bool bSurvivable = false;                     // false=잡몹 즉사 / true=엘리트 생존
float DeathblowHealthThreshold = 0.f;         // bSurvivable에서만, 이 HP 이하면 치명
```
- **잡몹 vs 엘리트를 코드 분기가 아니라 데이터로** — 잡몹 프로필은 `bSurvivable=false`(항상 즉사), 엘리트는 `true`+Threshold. 새 적의 처형 성격은 DataAsset만 채우면 됨(OCP, 개방-폐쇄).

> ⚠️ DA 튜닝 함정(메모리): `DeathblowHealthThreshold`는 양수여야(0이면 데스블로 영영 발동 불가) + 생존 칩 데미지보다 커야(안 그러면 생존 분기가 적을 죽여 "기상 끝에 풀썩").

---

## 8. 예상 면접 Q&A

**Q1. 처형 메커닉을 설명해보세요.**
> 경직 중인 적에게 강공이 들어가면 처형이 발동합니다. 플레이어는 트리거만 하고, 판정·데미지·연출은 전부 적의 ExecutionComponent가 소유합니다. 시작 시점에 데스블로(치명)인지 생존인지 판정해서 맞는 몽타주를 고르고, 몽타주가 끝나면 FinishExecution이 결판을 냅니다. 잡몹은 항상 즉사, 엘리트는 Health가 임계 이하일 때만 즉사하고 아니면 다운됐다 일어납니다.

**Q2. 경직·처형을 왜 컴포넌트로 분리했나요?**
> Pawn이 417줄로 커져서 프로젝트의 500줄 규칙에 걸렸고, 경직 상태머신과 처형 사이클은 독립적인 관심사였습니다. StaggerComponent, ExecutionComponent, ExecutionProfile로 빼고 통신은 델리게이트로만 했습니다. 직접 포인터가 0개라 의존성이 Pawn→컴포넌트 단방향이고, 컴포넌트는 ASC를 인터페이스로 접근해서 어느 캐릭터에든 붙일 수 있습니다.

**Q3. "동기 사망"이 무슨 뜻이고 왜 중요한가요?**
> SetNumericAttributeBase로 Health를 0으로 만들면 Health 변화 델리게이트가 그 자리에서 동기 발화합니다. 그래서 FinishExecution이 Health를 0으로 만드는 순간, OnHealthChanged→HandleDeath가 그 콜스택 안에서 먼저 다 돕니다. 이걸 모르면 "Health 깎고 나서 뭔가 하겠지" 하고 死후 로직을 잘못 넣게 됩니다. 한때 지연 사망인 줄 알았다가 로그로 동기임을 증명하고 정리했습니다.

**Q4. 처형 사망일 때 죽음 몽타주를 왜 스킵하나요? 어떻게 알고요?**
> 처형 피니셔 자체가 죽음 연출이라, 죽음 몽타주를 겹쳐 틀면 모션이 꼬입니다. HandleDeath가 ExecutionComponent::IsExecutionDeath()로 판정하는데, FinishExecution이 Health를 0으로 만들기 전에 bResolvingExecution 래치를 켜둬서, 동기로 끌려온 HandleDeath가 그 플래그를 읽습니다. 래치는 AbortForDeath가 소비하고요.

**Q5. 랙돌이 몸을 꺾던 문제, 코드로 어떻게 고쳤나요?**
> 안 고쳤습니다 — 코드는 표준 SetSimulatePhysics만 하니까요. 진범은 PhysicsAsset이었고 그걸 교체해서 해결했습니다. 그 전에 비물리 사망(프레임 동결)을 시도했다가 처형 후 공중에서 멈추는 버그를 겪고 전부 되돌렸습니다. 교훈은 "랙돌이 꺾이면 코드가 아니라 PhysicsAsset을 봐라"고, 주석에 박아뒀습니다.

**Q6. 타임아웃과 처형이 동시에 일어나면요?**
> 경직은 StaggerDuration 후 자동 복귀 타이머가 도는데, 처형이 먼저 시작하면 Invulnerable 태그가 켜집니다. OnStaggerTimeout이 RecoverFromStagger를 직접 부르지 않고 가드를 거쳐서, Invulnerable이면 return합니다. 처형 시네마틱 도중에 몸이 일어서버리는 걸 막고, 복귀는 처형의 OnExecutionResolved가 소유합니다.

---

## 9. 직접 설명 연습 (점검)

보지 말고 소리내어:

1. Poise 0부터 처형까지 전체 사슬 (poise→stagger→경직중강공→처형→데스블로/생존)
2. "동기 사망" — SetHealth(0)이 같은 콜스택에서 HandleDeath를 부른다는 것
3. 처형 사망 시 죽음 몽타주 스킵 + 그걸 아는 방법(IsExecutionDeath 래치)
4. 컴포넌트 분리 — 왜, 통신은 어떻게(델리게이트, 직접포인터 0)
5. 잡몹/엘리트를 코드 분기 없이 데이터(ExecutionProfile)로 가르는 법
6. 랙돌 꺾임의 진범(PhysicsAsset, 코드 무죄)

> 6개 막힘없으면 이 시스템도 "네 것". 이게 제일 복잡하니 2~3번 돌려도 정상.

---

## 다음 해설 예정
- `04` — 히트 피드백 + 비대칭 경직 (HitFeedbackComponent, GE 디커플링)
- `05` — (선택) 컴포넌트 간 의존성 다이어그램 종합 정리
