---
title: "엘리트 도끼 패링 시스템 — WeaponTrace 게이팅 / 윈도우별 오버라이드 / 패링 관대함 분리"
tags: ["weapon-trace", "parry", "gas", "motion-warping", "anim-notify", "combat", "elite", "design", "data-driven", "portfolio"]
created: 2026-06-09T12:49:48.309Z
updated: 2026-06-09T12:49:48.309Z
sources: []
links: []
category: architecture
confidence: medium
schemaVersion: 1
---

# 엘리트 도끼 패링 시스템 — WeaponTrace 게이팅 / 윈도우별 오버라이드 / 패링 관대함 분리

# 엘리트 도끼 패링 시스템 — WeaponTrace 게이팅 / 윈도우별 오버라이드 / 패링 관대함 분리

> 출처: 2026-06-09 세션. 엘리트 도끼 공격 패링이 간헐적으로 되던 문제를 근본 원인부터 추적해 3건 수정 + 설계 원칙 1건 정립. 코드 전부 Build.bat green, PIE 검증 후 submit.

---

## 0. 핵심 통찰 (제일 중요)

**플레이어 패링은 "적 WeaponTrace가 히트를 박는 것"에 100% 게이팅된다.**

패링 판정은 `AS_Combat::PostGameplayEffectExecute`에서 `IncomingDamage`가 들어올 때만 발화한다. 그 데미지는 `GA_WeaponTraceBase::OnWeaponHit`이 트레이스가 플레이어를 맞췄을 때만 적용한다. 따라서:

```
트레이스 안 맞음 → 데미지 GE 적용 X → 패링 게이트 호출조차 안 됨
```

→ **"패링이 안 된다"의 90%는 GE/패링 로직이 아니라 상류 트레이스 문제다.** 디버깅 시 GE를 먼저 의심하지 말고 "히트가 실제로 박혔나 / 언제 박혔나"를 먼저 본다. (`bDrawDebug=true`로 빨간 캡슐 확인.)

---

## 1. 문제 1 — 다단공격 1타만 패링됨

**증상:** 도끼 3연찍기·2연휘두르기에서 1타만 패링, 2타+ 무반응.

**원인:** once-per-actor 필터가 2겹인데 스코프가 어긋남.
- Task 레벨(`AT_WeaponTrace::AlreadyHitActors`): TraceBegin마다 새 태스크 → **스윙당** 리셋 (정상)
- GA 레벨(`GA_WeaponTraceBase::AlreadyHitActors`, `bOncePerActor`): `ActivateAbility`/`OnCleanup`에서만 리셋 → **몽타주 전체** 스코프

→ 다단 몽타주(TraceBegin/End 쌍 여러 개 = 1 GA 활성화)에서 1타가 플레이어를 `AlreadyHitActors`에 추가 → 2·3타는 `Contains() → return`으로 데미지 안 나감 → 패링 게이트 도달 못 함.

**비대칭 핵심:**
- 플레이어: 1 GA 활성화 = 1스윙 (콤보 입력마다 재발동, `GA_PlayerAttackBase.cpp`) → "1스윙=1히트"가 정답, `bOncePerActor=true` 유지
- 적: 1 GA 활성화 = 몽타주 = 여러 스윙 → "스윙(window)당 1히트" 필요

**해결:** `GA_EnemyWeaponTraceBase` **생성자(CDO)** 에서 `bOncePerActor = false`. 적 근접 전부 "trace window당 1히트"가 디폴트. 단발(Rush/Smash)은 window 1개라 Task 레벨 dedup으로 여전히 1히트(무영향). 플레이어 베이스는 `true` 상속.

**배운 점:** GA는 Actor가 아니라 `PossessedBy`/`BeginPlay` 없음. **정적 디자인 디폴트/튜닝값 = 생성자(CDO)** 가 정석 (부모의 `TraceMode=Sweep`과 같은 자리). 비대칭은 분기가 아니라 **데이터(per-faction CDO 디폴트)** 로 표현.

---

## 2. 문제 2 — Rush 모션워핑 스테일 오버슈트

**증상:** 플레이어가 다가왔는데 적이 앞으로 달렸다가 갑자기 뒤로 와서 공격.

**원인:** `GA_EnemyRushAttack::OnActivated`이 워프 타겟을 **발동 순간** 플레이어 위치로 1회 계산해 월드 고정점으로 박음. 실제 워프(루트모션)는 윈드업 뒤 MotionWarping 노티 구간에서 실행 → 그 사이 플레이어가 접근하면 고정점이 플레이어 너머가 됨 → 오버슈트 후 되돌아옴.

**해결:** `Event.Rush.Warp` 태그 신설 + 워프 계산을 `UpdateWarpTarget()`로 추출.
- `OnActivated`: 초기 1회 조준(폴백) + `Event.Rush.Warp` WaitGameplayEvent 리스너 등록
- 몽타주의 돌진 윈도우 시작에 둔 `AN_SendGameplayEvent`(EventTag=`Event.Rush.Warp`)가 발화 → **그 순간 현재 플레이어 위치로 재조준**

"리딩 없는 단발 조준 / 회피 보상" 설계 철학 유지 — 단지 *발동*이 아니라 *돌진 시작* 기준으로 옮긴 것. 노티 미배치 시 옛 동작으로 graceful degrade.

---

## 3. 문제 3 — 세로/대각 찍기 패링 어려움 + 윈도우별 트레이스 오버라이드 (아키텍처)

**증상:** 가로 휘두르기는 패링 정확, 세로(위→아래)/대각/공중 찍기는 패링 안 됨. 캡슐 반지름 키워도 안 됨.

**원인 (반지름이 레버가 아님):**
- 캡슐은 무기축(StartSocket→EndSocket)을 따라 만들어짐. 소켓이 **자루 전체**면 캡슐이 세로로 길쭉.
- 가로 휘두르기: 캡슐 길이가 수평 밴드를 공짜로 커버 → 첫 접촉 ≈ 시각 타격 → 패링 타이밍 자연스러움
- 세로 찍기: 길쭉한 세로 캡슐이 내려옴 → **날 끝이 플레이어 머리 높이 들어오는 순간(도끼 아직 위) 이미 히트** → 데미지/패링 게이트가 시각보다 **먼저** 발화 → 플레이어가 "몸에 닿을 때" 누르면 늦음
- **반지름 = 폭(WHERE), 패링 난이도 = 타이밍(WHEN). 폭으론 타이밍 못 고침.**

**해결 (아키텍처) — 트레이스 윈도우별 파라미터 오버라이드:**

트레이스 파라미터(소켓/반지름/모드)를 `ANS_WeaponTrace`(AnimNotifyState)에 얹어 **윈도우마다 자기 값**을 들고 오게 함. 한 몽타주가 윈도우별로 다른 트레이스 모양을 그릴 수 있음.

전달 경로:
```
ANS_WeaponTrace.NotifyBegin → Payload.OptionalObject = this  (노티 인스턴스)
GA_WeaponTraceBase.OnTraceBeginEvent → Cast<UANS_WeaponTrace>(Payload.OptionalObject)
  → StartSocketOverride/EndSocketOverride/CapsuleRadiusOverride/bOverrideTraceMode 읽음
  → 비었으면(None/0/uncheck) GA 디폴트 폴백
```
동기 디스패치(같은 콜스택)라 노티 인스턴스 수명 안전, 저장 안 함.

**적용:** 혼합 몽타주(찍기+스윕)에서
- 찍기 윈도우: `StartSocketOverride = trace_head`(도끼 머리 시작 소켓 1개 추가, `trace_tip` 재활용) → 히트 시점 = 머리 도달 = 시각 일치 → 패링 가능
- 스윕 윈도우: 비움 → GA 디폴트(`trace_base→trace_tip` 자루 전체)

**override 사용 규칙:** 의무 아님. **같은 몽타주 안에서 윈도우마다 값이 달라야 할 때만.** 공격 전체가 한 값이면 GA 디폴트에서 설정. 필드별 독립(필요한 칸만, 나머지 GA 상속). 평소엔 전부 비워두는 게 정상.

**확장 열림:** 새 파라미터 = 노티 UPROPERTY 1개 + GA 머지 1줄. 보스/도술 다중 모드 = `ETraceMode` enum 항목 추가하면 `TraceModeOverride`로 선택. 공유 베이스라 플레이어 공중공격에도 자동 적용.

---

## 4. 설계 원칙 — 히트 정확도 ⊥ 패링 관대함 (분리)

**함정:** 적 무기 캡슐 크기/윈도우 길이 하나가 두 가지를 동시에 조종.
1. 히트가 언제·어디서 박히나 (시각 정확도)
2. 패링이 얼마나 관대한가 (오버랩 길수록 아무 때나 눌러도 잡힘)

→ 캡슐 키우면 패링 쉬워지지만 도끼 안 닿았는데 맞음(싸구려). 줄이면 정확하지만 패링 빡셈. **한 손잡이에 두 개가 묶여 서로 잡아먹음.**

**원칙: 적 히트는 정확하게(머리 소켓+타이트 윈도우), 패링 관대함은 플레이어 쪽에서.**

| 원하는 것 | 맞는 레버 |
|---|---|
| 적 안 맞추기 쉽게(회피 가능) | 전조 길이/가독성, 공격 속도·추적 |
| 전투 긴장감 | 데미지·포이즈·빈도, 노랑(언블록) 섞기 |
| 패링 난이도 | **플레이어 퍼펙트 윈도우 길이**(`PerfectParryWindowGE` Duration) |

코드에 이미 2단 안전망: 홀드=상시 50% 블록(`BlockGE`) / 퍼펙트 0.15s=완전 무효+클래시(`PerfectParryWindowGE`). 퍼펙트는 원래 어려운 게 디자인, 50% 블록이 보험. (StellarBlade/Sekiro 모델: 적 히트박스 정직, 디플렉트 *윈도우*가 관대함 레버.)

---

## 5. 맨몸격투 확장 이음새 (YAGNI — 나중)

맨몸은 타격면이 여러 본(좌/우권·팔꿈치·무릎·발) → **더미 웨폰 ❌, 바디 본 소켓 ✓** + 윈도우별 오버라이드(이미 있음). 막힌 건 "어느 메시에서 소켓 읽나" 하나: `OnTraceBeginEvent`가 "Weapon" 태그 별도 메시를 찾음 → 맨몸은 `ACharacter::GetMesh()`(바디) 폴백 5줄 필요. **맨몸 공격 실제로 생길 때 추가.** 일관 프롭(너클/건틀릿)은 그냥 무기 취급(현 경로).

---

## 6. 관련 파일 (코드 포인터)

- `AbilitySystem/Attributes/AS_Combat.cpp` — `PostGameplayEffectExecute` 패링 게이트(정면판정/언블록/퍼펙트/50%블록)
- `AbilitySystem/Abilities/GA_WeaponTraceBase.cpp` — `OnWeaponHit`(데미지+히트이벤트), `OnTraceBeginEvent`(윈도우 오버라이드 적용), `bOncePerActor`
- `AbilitySystem/Abilities/Enemy/GA_EnemyWeaponTraceBase.cpp` — 생성자 `bOncePerActor=false`
- `AbilitySystem/Abilities/Enemy/GA_EnemyRushAttack.cpp` — `UpdateWarpTarget`/`OnRushWarpEvent` 워프 재조준
- `AbilitySystem/AnimNotifies/ANS_WeaponTrace.h/.cpp` — 윈도우별 오버라이드 필드 + `OptionalObject` 전달
- `AbilitySystem/Tasks/AT_WeaponTrace.cpp` — 실제 캡슐 sweep/TipLine, 서브스텝
- `KDGameplayTags.*` — `Event.Rush.Warp`

---

## 7. 포폴/자소서 각도 (engineering narrative)

이 세션이 보여주는 역량:

1. **근본 원인 추적 (증상≠원인):** "패링 간헐적"이라는 모호한 버그를 "패링은 트레이스 히트에 100% 게이팅"이라는 데이터 흐름으로 환원 → GE(무죄)와 트레이스(범인)를 분리 진단. 추측 수정 대신 콜스택을 따라감.

2. **데이터드리븐 비대칭:** 플레이어=1스윙1히트, 적=스윙당히트라는 요구 차이를 `if` 분기가 아니라 **per-faction CDO 디폴트(생성자)** 로 표현. GAS CDO/직렬화 이해.

3. **확장에 열린 설계:** 트레이스 파라미터를 GA 전역에서 AnimNotify 윈도우 단위로 내림 → 한 몽타주가 윈도우별로 다른 모양. 게임플레이 이벤트 페이로드(`OptionalObject`)로 동기 전달, 폴백으로 하위호환. 플레이어/보스/맨몸까지 같은 메커니즘 재사용.

4. **타이밍 vs 지오메트리 구분:** 패링 실패를 "판정이 좁다"(폭)가 아니라 "히트가 이르다"(타이밍)로 정확히 진단 — 캡슐 길이가 세로 찍기에서 조기 히트를 만드는 기하학적 원인까지.

5. **시스템 설계 판단:** "적을 안 맞게 + 긴장감 + 패링 가능" 3목표가 충돌한다는 인식 → 한 레버(적 히트박스)에 묶인 책임을 **히트 정확도(적) / 패링 관대함(플레이어 윈도우)** 로 분리. 결합도 제거 = SOLID/관심사 분리의 전투 도메인 적용.

> 한 문장: *모호한 "패링이 가끔 안 됨" 버그를 GAS 데미지 파이프라인의 데이터 흐름까지 추적해, 트레이스-패링 게이팅이라는 근본 구조를 규명하고, per-faction 디폴트·윈도우별 트레이스 오버라이드·히트/패링 관심사 분리라는 3계층 해결로 확장 가능한 전투 트레이스 아키텍처를 정립.*
