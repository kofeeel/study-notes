---
title: ProjectKD 코드 해설 01 — WeaponTrace + 패링 게이팅
tags:
  - ProjectKD
  - portfolio
  - code-study
  - gas
  - weapon-trace
created: 2026-06-15
status: 학습용 v1
related:
  - "[[ProjectKD-담당작업-시스템정리]]"
  - "[[ProjectKD-문제해결-사례집]]"
---

# 코드 해설 01 — WeaponTrace + 패링 게이팅

> **목적**: 면접에서 "이 코드 직접 설명해보세요"에 답할 수 있게, 실제 소스를 줄 단위로 이해.
> 모든 줄 번호는 실제 파일 기준(2026-06-15). 옆에 파일 열어두고 대조하며 읽을 것.
> **읽는 법**: ① 데이터 흐름 한 바퀴 → ② 줄별 해설 → ③ GAS 개념 → ④ 예상 Q&A. 마지막에 직접 설명 연습.

**관련 파일 4개 (이것만 보면 됨)**
- `Source/Project_KD/AbilitySystem/Abilities/GA_WeaponTraceBase.{h,cpp}` — 공격 능력 본체
- `Source/Project_KD/AbilitySystem/Abilities/Enemy/GA_EnemyWeaponTraceBase.cpp` — 적 전용 디폴트
- `Source/Project_KD/AbilitySystem/Attributes/AS_Combat.cpp` — **패링 게이트**(피격 처리)
- `Source/Project_KD/AbilitySystem/AnimNotifies/ANS_WeaponTrace.h` — 윈도우별 오버라이드

---

## 1. 큰 그림 — 한 번의 공격이 도는 순서

```
[몽타주 재생]
   │  몽타주 타임라인에 ANS_WeaponTrace(위험 윈도우)가 깔려 있음
   ▼
ANS_WeaponTrace.NotifyBegin ──Event.Montage.TraceBegin──┐
                                                        ▼
GA_WeaponTraceBase::OnTraceBeginEvent
   - 무기 메시(태그 "Weapon") 찾기
   - 윈도우 오버라이드 읽기 (Payload.OptionalObject = 그 노티 인스턴스)
   - AT_WeaponTrace 시작 → OnHit 바인딩
   ▼ (매 틱 캡슐 sweep, 맞으면)
GA_WeaponTraceBase::OnWeaponHit(Hit)
   - bOncePerActor 중복 필터
   - 진영 게이트 (적↔적 차단)
   - 데미지 GE 스펙 만들기 + SetByCaller(AttackPower)
   - ApplyGameplayEffectSpecToTarget ──────────┐  (피해자 ASC의 IncomingDamage에 값 씀)
   - Event.Combat.Hit 도 따로 송신(연출용)     │
                                               ▼
AS_Combat::PostGameplayEffectExecute  ← ★패링/방어/사망이 전부 여기서 갈림★
   - IncomingDamage 게이트웨이 비우기
   - 전방 판정(±90°)
   - 언블록? → 패링 스킵
   - 퍼펙트 패링? → 데미지 0 + 이벤트
   - 적 방어 패링? → 데미지 0 + 클래시 이벤트
   - 일반 가드? → 50% 감소
   - Defense 경감 → 치명 선판정 → HitReact 이벤트 → Health 차감
```

**핵심 한 문장**: 공격 GA는 *데미지를 피해자 ASC에 쏘기만* 하고, **막혔는지/죽었는지 판단은 전부 피해자의 AttributeSet(`AS_Combat`)이 한다.** 공격자와 피해자의 책임이 분리돼 있다.

---

## 2. GA_WeaponTraceBase — 공격 본체

### 2-1. ActivateAbility (cpp:22~77) — 공격 시작

```cpp
if (!CommitAbility(...)) { EndAbility(...); return; }   // 28: 코스트/쿨다운 커밋, 실패면 종료
if (!IsValid(AttackMontage)) { EndAbility(...); return; } // 34: 몽타주 없으면 종료
AlreadyHitActors.Reset();                                // 40: 이번 발동의 중복-히트 기록 초기화
```

- **`CommitAbility`** = GAS에서 "이 능력을 실제로 발동한다"고 확정하는 단계. 쿨다운/코스트를 여기서 소모. 실패하면 발동 취소.
- **`ensureMsgf(!GetAssetTags().IsEmpty())` (46)** — Asset Tag가 비면 적 포이즈/처형이 조용히 안 도니까, 셋업 빠진 걸 **개발 중 1회 경고**로 잡는 안전망. (`ensure`는 shipping에서 크래시 안 냄, 개발 중에만 걸림)

그 다음 **태스크 3개**를 등록:
```cpp
MontageTask = ...PlayMontageAndWait(...)   // 50: 몽타주 재생 + 끝/중단 콜백
TraceBeginTask = ...WaitGameplayEvent(Event.Montage.TraceBegin)  // 62: 트레이스 시작 신호 대기
TraceEndTask   = ...WaitGameplayEvent(Event.Montage.TraceEnd)    // 67: 트레이스 끝 신호 대기
StartSafetyTimer(...)                      // 72: 몽타주가 어떤 이유로 콜백 안 와도 능력 강제종료
```

> **왜 AbilityTask?** GAS에서 "시간이 걸리는 비동기 작업"(몽타주 재생, 이벤트 대기)은 AbilityTask로 만든다. 능력이 끝나면 태스크도 자동 정리됨 → 메모리/델리게이트 누수 방지. 직접 타이머 돌리는 것보다 GAS 수명주기에 안전하게 묶임.

### 2-2. OnTraceBeginEvent (cpp:79~135) — 트레이스 시작

```cpp
if (!IsActive()) return;   // 82: 레이스 가드 — 취소 체인이 먼저 EndAbility 했으면 1프레임 유령 트레이스 방지
```
이 한 줄이 **방어적 프로그래밍**의 예. 콤보 취소/히트리액트로 능력이 막 끝난 직후에 이 노티가 도착할 수 있어서, 활성 상태가 아니면 무시.

무기 메시 찾기 (87~104): 액터의 SkeletalMesh 컴포넌트 중 **`"Weapon"` 태그가 붙은 것**을 찾음. (캐릭터 본체 메시 말고 손에 든 무기)

**윈도우별 오버라이드 (113~125)** — 이게 설계의 핵심:
```cpp
FName EffStartSocket = StartSocket;   // 기본값 = GA 디폴트
...
if (const UANS_WeaponTrace* Window = Cast<UANS_WeaponTrace>(Payload.OptionalObject))
{
    if (Window->StartSocketOverride != NAME_None) EffStartSocket = Window->StartSocketOverride;
    ... // 비어있지(None/0/uncheck) 않은 칸만 덮어씀
}
```
- `Payload.OptionalObject`에 **그 노티 인스턴스 자신**이 실려 옴(`ANS_WeaponTrace`가 `this`를 넣어 보냄, 헤더 주석 참고).
- 노티에 값이 있으면 그 윈도우만 다른 소켓/반지름/모드로 트레이스 → **한 몽타주가 윈도우마다 다른 모양**.
- 비어 있으면 GA 디폴트 상속. → "평소엔 다 비워두는 게 정상, 같은 몽타주 안에서 윈도우마다 달라야 할 때만 채움."
- **동기 디스패치**라 노티 인스턴스 수명 안전(저장 안 함).

### 2-3. OnWeaponHit (cpp:146~200) — 맞았을 때

```cpp
if (bOncePerActor) {                          // 151: 한 액터 한 번만?
    if (AlreadyHitActors.Contains(HitActor)) return;
    AlreadyHitActors.Add(HitActor);
}
```
→ **이 필터의 스코프가 "패링 간헐적" 버그의 진범.** `AlreadyHitActors`는 `ActivateAbility`에서만 리셋(40) → **발동 1회(=몽타주 전체) 스코프**. 다단 공격이면 1타가 추가 → 2·3타가 여기서 `return`. (해결은 §4)

```cpp
// 진영 게이트 (162): 적 무기는 다른 적 안 때림
if (AttackerASC->HasMatchingGameplayTag(Team_Enemy)
    && TargetASC->HasMatchingGameplayTag(Team_Enemy)) return;
```

**데미지 적용 (174~186)** — GAS의 정석:
```cpp
const float AttackPower = AttackerASC->GetNumericAttribute(GetAttackPowerAttribute()); // 공격력 읽기
FGameplayEffectContextHandle Context = AttackerASC->MakeEffectContext();
Context.AddSourceObject(GetAvatarActorFromActorInfo());
Context.AddHitResult(Hit);                     // 맞은 위치/법선 → 나중에 방향 넉백/연출에 씀
FGameplayEffectSpecHandle SpecHandle = AttackerASC->MakeOutgoingSpec(DamageEffectClass, 1.f, Context);
AssignTagSetByCallerMagnitude(SpecHandle, SetByCaller_AttackPower, AttackPower); // 런타임 값 주입
AttackerASC->ApplyGameplayEffectSpecToTarget(*SpecHandle.Data, TargetASC);        // 피해자에게 적용
```
> **SetByCaller** = GE에 "런타임에 정해지는 숫자"를 꽂는 방법. 데미지 값을 GE 에셋에 하드코딩 안 하고 코드에서 공격력을 계산해 넘김. CLAUDE.md의 "단순 케이스는 SetByCaller 1회성" 규칙 그대로.

**히트 이벤트 (190~196)** — 데미지와 별개로 `Event.Combat.Hit`를 피해자에게 송신. "피해자가 자기 반응(히트스탑/넉백)을 알아서 결정"하는 디커플링. 데미지(숫자)와 연출(이벤트)을 분리.

---

## 3. AS_Combat::PostGameplayEffectExecute — ★패링 게이트★

> 가장 중요한 함수. `AS_Combat.cpp:21~112`. **"막혔나/죽었나"가 전부 여기.**

```cpp
if (Data.EvaluatedData.Attribute != GetIncomingDamageAttribute()) { return; } // 25: IncomingDamage 변화일 때만
const float LocalDamage = GetIncomingDamage();
SetIncomingDamage(0.0f);              // 28~29: 게이트웨이 즉시 비움
if (LocalDamage <= 0.0f) { return; }
```

> **메타 어트리뷰트(IncomingDamage)** = "1회용 받는 통로". Health처럼 영속되는 값이 아니라, 데미지가 들어올 때마다 채웠다 즉시 0으로 비움. `OnWeaponHit`이 `ApplyGameplayEffectSpecToTarget`으로 여기에 값을 쏘면 → 이 함수가 발화. **이게 "패링은 트레이스 히트에 100% 게이팅"의 코드적 실체**: 트레이스가 안 맞으면 IncomingDamage에 값이 안 들어와서 이 함수 자체가 안 돈다.

**전방 판정 (38~49)**:
```cpp
const FVector ToAttacker = (Attacker->GetActorLocation() - Defender->GetActorLocation()).GetSafeNormal2D();
const FVector Forward = Defender->GetActorForwardVector().GetSafeNormal2D();
bFrontalAttack = FVector::DotProduct(Forward, ToAttacker) > 0.f;   // 내적 > 0 = 정면 반구(±90°)
HitAngle = FMath::FindDeltaAngleDegrees(...);                       // 좌우 부호각 (방향별 피격에 씀)
```
- **`GetEffectCauser()`를 쓰는 이유 (37 주석)**: `GetInstigator()`는 플레이어가 PlayerState(월드 원점)라 각도가 깨짐. EffectCauser = 실제 때린 아바타라 위치가 정확.
- 내적(dot)으로 정면 여부 판정 = 벡터 기본기. 외워둘 것.

**패링 분기 (순서가 중요)**:
```cpp
// ① 언블록 (54): Ability.Combat.Unblockable 태그 = 노랑 공격 → 아래 패링 전부 스킵(회피만)
// ② 퍼펙트 패링 (57): 데미지 0 + Event.Combat.PerfectParryTriggered → return
// ③ 적 방어 패링 (69): 적이 가드 중 정면 피격 → 데미지 0 + ParrySuccess 클래시 이벤트 → return
// ④ 일반 가드 (84): 50% 감소 (return 안 함, 계속 진행)
```
- ①~③은 `return`으로 끝(데미지 0). ④는 절반만 깎고 계속.
- **비대칭 주석 (68)**: 적 방어 패링은 슬로모 경로를 안 타서 플레이어는 무경직. 스텔라 블레이드식 비대칭이 코드에 박혀 있음.

**치명 선판정 + 히트리액트 (87~104)**:
```cpp
const float Mitigated = FMath::Max(FinalDamage - GetDefense(), 0.0f);
const float NewHealth = ... - Mitigated;
const bool bLethal = Mitigated > 0.0f && NewHealth <= 0.0f;   // 죽는 타격인가?

if (!bLethal) {   // 치명타면 움찔 이벤트 스킵 (사망 연출과 같은 프레임 충돌 방지)
    ... Event.Combat.HitReact, EventMagnitude = bBlockedHit ? 1.0f : 0.0f
}
```
→ **죽는 타격에 HitReact를 안 보내는 것**이 "몽타주 순서 꼬임" 버그의 해결(사례집 #2). Health 깎기 *전에* 죽을지 미리 계산하는 게 포인트.

**Health 차감 (111)**:
```cpp
ASC->SetNumericAttributeBase(GetHealthAttribute(), FMath::Max(NewHealth, 0.0f));
```
- 주석 108~110 필독: **이 Set이 Health 델리게이트를 동기 발화** → 0 도달 시 `HandleDeath`(랙돌)가 *이 호출 안에서* 다 끝나고 리턴. 그래서 "이 줄 아래에 死후 가정 로직 추가 금지".
- `SetNumericAttributeBase`는 `PreAttributeChange` 클램프를 건너뛰므로 여기서 직접 `Max(_, 0)` 하한.

---

## 4. 진영별 비대칭 = 분기가 아니라 데이터 (CDO)

`GA_EnemyWeaponTraceBase.cpp:11~16`:
```cpp
UGA_EnemyWeaponTraceBase::UGA_EnemyWeaponTraceBase()
{
    bOncePerActor = false;   // 적은 스윙(윈도우)당 1히트
}
```
- 플레이어 base는 `bOncePerActor = true`(헤더 55) → "1스윙 1히트".
- 적은 생성자에서 `false`로 → 다단 몽타주에서 스윙마다 패링 게이트 도달.
- **`if (적)` 분기가 아니라 클래스 디폴트(CDO)로 표현.** 생성자 = "이 클래스의 정적 디자인 디폴트"를 두는 자리.

> **CDO (Class Default Object)** = 모든 UClass가 갖는 "기본값 원본 인스턴스". 생성자에서 설정한 값이 CDO에 박히고, 에디터 디폴트/직렬화의 기준이 됨. GA는 Actor가 아니라 `BeginPlay` 없음 → 정적 디폴트는 생성자에 둔다.

추가로 적 base가 오버라이드하는 것:
- `OnActivated` (18): 공격 시작 시 `StopMovement` → 추격 잔여 속도로 미끄러지는 것 방지.
- `GetEffectiveMontagePlayRate` (38): `AttackSpeedMultiplier`(DataAsset 밸런싱 노브) 곱함.
- `OnCleanup` (46): 전조 큐 제거 + **토큰 반납**(EncounterSubsystem). `Super::OnCleanup` 호출로 base 트레이스 정리 체인.

---

## 5. 예상 면접 Q&A

**Q1. 패링이 트레이스에 게이팅된다는 게 무슨 뜻이죠?**
> 패링 판정은 `AS_Combat::PostGameplayEffectExecute`에서 IncomingDamage가 들어올 때만 발화합니다. 그 데미지는 `OnWeaponHit`이 트레이스가 플레이어를 맞췄을 때만 적용하죠. 그래서 트레이스가 안 맞으면 패링 함수 자체가 안 돕니다. "패링이 가끔 안 된다"를 디버깅할 때 패링 로직이 아니라 상류 트레이스부터 본 이유입니다.

**Q2. 다단 공격 1타만 패링되던 버그, 원인과 해결은?**
> once-per-actor 필터가 GA 레벨에서 발동 전체(몽타주 전체) 스코프였습니다. 다단 몽타주는 1발동에 여러 스윙인데, 1타가 플레이어를 AlreadyHitActors에 추가하면 2·3타가 Contains에서 return돼 데미지가 안 나가고 패링 게이트에 도달 못 했죠. 해결은 적 base 생성자에서 `bOncePerActor=false`. 스윙 내 중복은 Task 레벨 필터가 따로 막아서 "스윙당 1히트"가 됩니다.

**Q3. 왜 if 분기 대신 CDO로 비대칭을 표현했나요?**
> 플레이어=1스윙1히트, 적=스윙당히트는 진영의 *정적 성질*이지 런타임 조건이 아닙니다. 매 히트마다 `if(적)`을 타는 것보다 클래스 디폴트로 박는 게 의미에 맞고, 에디터에서 BP child가 상속/오버라이드하기도 자연스럽습니다. GA는 Actor가 아니라 생성자가 그 자리고요.

**Q4. 윈도우별 트레이스 오버라이드는 왜 필요했죠?**
> 세로 찍기가 패링이 안 됐는데, 캡슐이 자루 전체를 따라 길쭉해서 날 끝이 머리 높이에 닿는 순간(도끼 아직 위) 이미 히트가 박혔습니다. 시각보다 판정이 일러서 타이밍이 안 맞았죠. 반지름(폭)으론 타이밍을 못 고칩니다. 그래서 AnimNotifyState에 소켓/반지름/모드 오버라이드를 얹어, 찍기 윈도우만 도끼 머리 소켓으로 트레이스하게 했습니다. 히트 시점이 시각과 일치해서 패링이 됩니다. 전달은 게임플레이 이벤트 페이로드로 동기, 비우면 GA 디폴트로 폴백이라 하위호환됩니다.

**Q5. 히트 정확도와 패링 난이도를 왜 분리했나요?**
> 적 무기 캡슐 크기 하나가 두 가지를 동시에 조종했습니다 — 히트가 언제 박히나(정확도)와 패링이 얼마나 관대한가(오버랩 길수록 아무 때나 잡힘). 캡슐을 키우면 패링은 쉬워지지만 도끼가 안 닿았는데 맞는 싸구려 히트가 됩니다. 그래서 적 히트는 정확하게(머리 소켓+타이트 윈도우) 두고, 패링 관대함은 플레이어 퍼펙트 윈도우 길이로 옮겼습니다. 세키로/스텔라 블레이드 모델이죠.

**Q6. SetByCaller가 뭐고 왜 썼나요?**
> GE 에셋에 데미지를 하드코딩하지 않고, 런타임에 계산한 공격력을 GE 스펙에 주입하는 방법입니다. 공격력은 AttributeSet에서 읽어 매번 달라지니까요. 복잡한 데미지 계산은 ExecCalc로 빼지만, 단순 케이스는 SetByCaller 한 번이 적절하다는 게 프로젝트 규칙입니다.

**Q7. 왜 메타 어트리뷰트(IncomingDamage)를 거치나요? 그냥 Health를 깎으면?**
> 데미지가 들어오는 단일 지점을 만들기 위해서입니다. IncomingDamage라는 1회용 통로를 거치면 PostGameplayEffectExecute 한 곳에서 패링/방어/경감/치명판정/사망을 순서대로 처리할 수 있죠. Health를 직접 깎으면 그 모든 로직이 호출자마다 흩어집니다. GAS의 표준 데미지 파이프라인 패턴입니다.

---

## 6. 직접 설명 연습 (점검)

면접관이 됐다 치고, **보지 않고** 다음을 소리내어 설명해봐:

1. 공격 한 번이 도는 순서를 5단계로 (몽타주 → 노티 → 트레이스 → 히트 → AttributeSet)
2. "패링이 트레이스에 게이팅된다"를 IncomingDamage 메타 어트리뷰트로 설명
3. 다단 1타 버그의 원인(스코프)과 해결(CDO)
4. 세로 찍기 패링 실패가 왜 "폭"이 아니라 "타이밍" 문제였는지
5. 히트 정확도 ⊥ 패링 관대함 분리

> 막히는 항목이 있으면 그 섹션(§2~§4)을 다시. 5개 다 막힘없이 나오면 이 시스템은 "네 것".

---

## 다음 해설 예정
- `02` — 적 AI 직교 모델 (토큰/EQS/Crowd + SelectAttack 거리밴드)
- `03` — 사망/처형/데스블로 사이클
- (이후 히트피드백, 컴포넌트 분리 …)
