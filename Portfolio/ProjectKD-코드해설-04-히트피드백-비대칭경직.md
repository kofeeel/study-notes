---
title: ProjectKD 코드 해설 04 — 히트 피드백 + 비대칭 경직
tags:
  - ProjectKD
  - portfolio
  - code-study
  - hit-feedback
  - gas
created: 2026-06-15
status: 학습용 v1
related:
  - "[[ProjectKD-코드해설-03-사망처형데스블로]]"
  - "[[hit-feedback-decoupled-from-ge]]"
  - "[[combat-model-asymmetric-stagger]]"
---

# 코드 해설 04 — 히트 피드백 + 비대칭 경직

> **목적**: "손맛"을 코드로 어떻게 만드는가 + 왜 연출을 데미지에서 분리했는가.
> 줄 번호는 실제 파일 기준(2026-06-15). ⚠️ 이 시스템은 C++가 얇고 ABP/BP가 연출을 마무리 — 경계를 명시.

**관련 파일**
- `Source/Project_KD/Combat/HitFeedbackComponent.cpp` — 본 셰이크 구동(틱)
- `Source/Project_KD/Enemy/KDEnemyBaseCharacter.cpp` — `OnHitReceived`(피격 연출 분기)
- `AS_Combat.cpp` — 데미지/패링(해설 01) — 여기엔 *연출이 없음*이 포인트

---

## 1. 핵심 설계 — 연출을 데미지(GE)에서 분리

> 흔한 실수: 데미지 GameplayEffect 안에 히트스탑·셰이크·사운드를 다 박는 것. 그러면 패링(데미지 0)일 때 연출도 같이 사라지고, 칩 히트엔 연출이 안 나온다.

**우리 구조**: 데미지와 연출이 **다른 경로**로 흐른다.
```
OnWeaponHit (해설 01)
   ├─ ApplyGameplayEffectSpecToTarget  → 데미지(숫자) → AS_Combat
   └─ SendGameplayEventToActor(Event.Combat.Hit)  → 연출(피격자가 결정)
```
- 데미지는 GE로, 연출은 **`Event.Combat.Hit` 이벤트**로 따로 보냄(`GA_WeaponTraceBase.cpp:190~196`).
- **"피격자가 자기 반응을 결정한다"** — 때린 쪽은 연출을 강요 안 함. 디커플링.

---

## 2. 피격자의 연출 분기 — KDEnemyBaseCharacter::OnHitReceived

`cpp:401~433` (연출 부분만):
```cpp
const bool bStaggered = StaggerComp && StaggerComp->IsStaggered();

// 처형 히트만 제외 — 그건 처형 레인이 자기 연출(큐/몽타주)을 소유하니 중복 금지
const bool bExecutionHit = bStaggered && ExecutionComp->IsExecutionTrigger(Payload->InstigatorTags);
if (!bExecutionHit) {
    if (HitFeedback) HitFeedback->TriggerBoneShake();                       // 본 셰이크
    AbilitySystemComponent->ExecuteGameplayCue(GameplayCue_Combat_HitImpact_Light, Payload->ContextHandle); // VFX/SFX 큐
}

// 경직 중인 몸은 poise/넉백 스킵 — 단 위의 셰이크/큐는 이미 줬음(얼어붙지 않고 칩 반응)
if (bStaggered) return;
```
- **칩 히트도 연출은 나간다** — 경직 중에 맞아도 셰이크+사운드는 줘서 몸이 반응(얼어붙지 않게). 단 처형 강공만 제외(처형 연출과 겹치지 않게).
- **GameplayCue** = GAS의 "비주얼/오디오 전용 채널". 게임플레이 로직과 분리된 연출. `ExecuteGameplayCue`로 발화하면 등록된 BP 큐가 VFX/SFX/카메라를 처리. → CLAUDE.md §1-2 "비주얼/오디오 효과는 GameplayCue".

---

## 3. 본 셰이크 — HitFeedbackComponent (틱 구동)

`HitFeedbackComponent.cpp` 전체가 거의 이게 다:
```cpp
void TriggerBoneShakeParams(float Intensity, float Duration) {
    ActiveIntensity = Intensity;
    ActiveDuration = FMath::Max(0.05f, Duration);
    ShakeTimeRemaining = ActiveDuration;
    CurrentShakeAlpha = 1.0f;       // 셰이크 시작 = 알파 1
}

void TickComponent(float DeltaTime, ...) {
    if (ShakeTimeRemaining > 0.0f) {
        ShakeTimeRemaining -= DeltaTime;
        CurrentShakeAlpha = ShakeTimeRemaining / ActiveDuration;   // 시간 따라 1→0 감쇠
    }
    ...
}
```
- **C++는 "얼마나 흔들지"(`CurrentShakeAlpha`, 1→0)만 계산한다.** 실제 본을 흔드는 건 **ABP**가 이 알파를 읽어서 함. 역할 분리: C++=수치 구동, ABP=비주얼 적용.
- `TriggerBoneShakeParams(Intensity, Duration)` — 파라미터 버전이 이미 뚫려 있음. 지금은 고정값(`TriggerBoneShake`)이지만, hit 프로파일이 3개+ 쌓이면 공격별로 다른 셰이크를 줄 이음새. (데이터 전환은 YAGNI로 보류)

> ⚠️ **경계**: 본 셰이크의 *적용*(어느 본을 어떻게)은 ABP, VFX/SFX는 BP 큐. C++는 트리거+감쇠 곡선만. 면접에서 "C++로 다 했냐" 물으면 정직하게 "구동은 C++, 비주얼 적용은 ABP/BP"라고.

---

## 4. 비대칭 경직 — 적만 경직, 플레이어는 안 함

> 스텔라 블레이드식 비대칭. "내가 압박한다"는 손맛은 적만 경직되고 플레이어는 안 되는 데서 나온다.

코드에 박힌 비대칭:
1. **Poise는 적 전용** (`KDEnemyBaseCharacter::OnHitReceived:441~460`) — Poise 차감 로직이 적 Pawn에만 있음. 플레이어는 Poise/경직 개념 자체가 없음.
2. **패링 슬로모도 비대칭** (해설 01, `AS_Combat.cpp:68`) — 적 방어 패링은 슬로모 경로를 안 타서 플레이어는 무경직.
3. **friendly fire 차단** — "Player attacks only are registered" (cpp:442 주석). 적끼리 때려도 poise 안 깨짐(해설 01의 진영 게이트).

→ 경직 차단 태그(`State.Combat.SuperArmor` 등)도 **적 GA에만** 건다. 플레이어 레인은 안 건드림(페어 도메인 분리 시절의 잔재이자 의도적 비대칭).

---

## 5. 예상 면접 Q&A

**Q1. 왜 연출을 데미지 GE에서 분리했나요?**
> 데미지 GE에 히트스탑·셰이크·사운드를 박으면, 패링으로 데미지가 0이 될 때 연출도 같이 사라지고 칩 히트엔 연출이 안 나옵니다. 그래서 데미지는 GE로, 연출은 Event.Combat.Hit 이벤트로 따로 보냈습니다. 피격자가 자기 반응을 결정하는 구조라, 경직 중 칩 히트에도 셰이크는 주되 처형 강공만 제외하는 식으로 세밀하게 제어됩니다.

**Q2. GameplayCue를 쓴 이유는?**
> GAS에서 비주얼/오디오는 GameplayCue라는 전용 채널로 빼는 게 표준입니다. 게임플레이 로직과 연출이 섞이지 않고, 멀티에서 큐만 리플리케이트하면 되니까요. 프로젝트 규칙도 "비주얼/오디오 효과는 반드시 GameplayCue"입니다. 히트 임팩트, 처형 카메라, 전조 다 큐로 뺐습니다.

**Q3. 본 셰이크는 C++로 다 구현했나요?**
> 아니요, 역할을 나눴습니다. C++의 HitFeedbackComponent는 "얼마나 흔들지"를 시간에 따라 1에서 0으로 감쇠시키는 알파만 계산하고, 실제로 본을 흔드는 비주얼 적용은 ABP가 그 알파를 읽어서 합니다. VFX/SFX는 BP 큐고요. 수치 구동과 비주얼 적용을 분리한 겁니다.

**Q4. 비대칭 경직이 뭔가요?**
> 적만 경직되고 플레이어는 경직되지 않습니다. 스텔라 블레이드 모델이죠. Poise 시스템 자체가 적 Pawn에만 있고, 패링 슬로모도 적만 타고, 경직 차단 태그도 적 GA에만 겁니다. 플레이어가 "내가 압박하고 있다"는 손맛을 느끼게 하는 의도적 설계입니다.

---

## 6. 직접 설명 연습 (점검)

1. 데미지와 연출이 다른 경로(GE vs Event.Combat.Hit)로 흐르는 이유
2. 경직 중 칩 히트에 셰이크는 주되 처형 강공만 제외하는 이유
3. 본 셰이크에서 C++가 하는 일 vs ABP가 하는 일
4. 비대칭 경직이 코드 어디에 박혀 있는지 (3군데)

---

## 시리즈 완료
- [[ProjectKD-코드해설-01-WeaponTrace패링]]
- [[ProjectKD-코드해설-02-적AI직교모델]]
- [[ProjectKD-코드해설-03-사망처형데스블로]]
- [[ProjectKD-코드해설-04-히트피드백-비대칭경직]] (이 문서)
- 인덱스 → [[ProjectKD-코드해설-00-학습인덱스]]
- 개념 빠른복습 → [[ProjectKD-GAS핵심개념-치트시트]]
