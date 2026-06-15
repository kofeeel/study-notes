---
title: ProjectKD — GAS 핵심개념 치트시트 (빠른 복습)
tags:
  - ProjectKD
  - portfolio
  - code-study
  - gas
  - cheatsheet
  - flashcard
created: 2026-06-15
status: 학습용 v1
related:
  - "[[ProjectKD-코드해설-00-학습인덱스]]"
---

# GAS 핵심개념 치트시트

> 해설 문서에 반복해서 나오는 개념을 **카드 형식**으로. 피곤할 때 이것만 훑어도 됨.
> 사용법: 질문만 보고 답을 가린 뒤 떠올려보고, 안 되면 펼쳐서 확인. 우리 코드의 실제 위치도 같이.

---

## 🃏 1. 동기 사망 (synchronous death) ★최중요

**Q. `SetNumericAttributeBase(Health, 0)`을 부르면 무슨 일이 벌어지나?**

> 어트리뷰트 변화 델리게이트가 **같은 콜스택에서 즉시(동기)** 발화한다. 그래서 그 한 줄이 `OnHealthChanged → HandleDeath`(랙돌, 토큰반납, brain정지)를 통째로 실행하고 *나서야* 다음 줄로 넘어간다.
> → 그 줄 다음에 "적이 아직 살아있겠지" 가정한 死후 로직을 넣으면 안 됨.
> 📍 `ExecutionComponent.cpp:141` → `KDEnemyBaseCharacter.cpp:194 OnHealthChanged → :202 HandleDeath`

**Q. 비동기였다면 왜 다른가?** → 비동기면 Set 후 다음 줄이 먼저 돌고 죽음 처리는 나중. 우리는 동기라 죽음이 끼어들어 끝난다.

---

## 🃏 2. 메타 어트리뷰트 (meta attribute)

**Q. `IncomingDamage`는 왜 Health랑 다른가?**

> Health는 영속 값, `IncomingDamage`는 **1회용 통로**. 데미지가 들어올 때마다 채웠다 즉시 0으로 비운다. 데미지의 *단일 진입점*을 만들어서, `PostGameplayEffectExecute` 한 곳에서 패링→방어→경감→치명→사망을 순서대로 처리.
> 📍 `AS_Combat.cpp:25~29`

**한 줄**: "데미지가 들어오는 단 하나의 문. 그 문이 열려야 패링 판정도 돈다."

---

## 🃏 3. 패링 게이팅

**Q. "패링은 트레이스 히트에 100% 게이팅된다"가 무슨 뜻?**

> 패링 *감지 코드*(`PostGameplayEffectExecute`)는 `IncomingDamage`가 들어와야만 실행된다. 그 값은 트레이스 히트가 있어야 들어온다. → **히트가 없으면 패링 판정 함수 자체가 안 돈다.**
> 인과 주의: "패링이라 트레이스를 안 한다"가 ❌. "트레이스가 안 맞아서 패링을 감지할 기회가 없었다"가 ✅.
> → 디버깅: "패링 안 됨"의 90%는 상류 "히트 안 박힘".
> 📍 `GA_WeaponTraceBase.cpp:185`(데미지 적용) → `AS_Combat.cpp:21`(게이트)

---

## 🃏 4. CDO (Class Default Object)

**Q. 진영별 비대칭(`bOncePerActor`)을 왜 `if(적)` 아니라 생성자에 뒀나?**

> 진영의 *정적 성질*이지 런타임 조건이 아니라서. 생성자 = "클래스 디폴트"를 두는 자리(CDO에 박힘). GA는 Actor가 아니라 `BeginPlay`가 없으니 정적 디폴트는 생성자. BP child가 상속/오버라이드하기도 자연스러움.
> 📍 `GA_EnemyWeaponTraceBase.cpp:15` (플레이어=true 유지, 적=false)

**한 줄**: "분기로 매번 묻지 말고, 클래스가 자기 디폴트로 들고 있게."

---

## 🃏 5. SetByCaller

**Q. 데미지 값을 GE에 하드코딩 안 하고 어떻게 넘기나?**

> `AssignTagSetByCallerMagnitude`로 런타임 계산값(공격력)을 GE 스펙에 주입. GE 에셋은 "여기 SetByCaller 값 들어옴"만 알고, 실제 숫자는 코드가 매번 넣음.
> 복잡한 계산은 ExecCalc로, 단순 케이스는 SetByCaller 1회성(프로젝트 규칙).
> 📍 `GA_WeaponTraceBase.cpp:183`

---

## 🃏 6. GameplayCue

**Q. 비주얼/오디오를 왜 큐로 빼나?**

> 게임플레이 로직과 연출을 분리하는 GAS 전용 채널. 멀티에서 큐만 리플리케이트하면 됨. 프로젝트 규칙 "비주얼/오디오는 반드시 GameplayCue". 히트 임팩트·처형 카메라·전조가 다 큐.
> 📍 `KDEnemyBaseCharacter.cpp:431 ExecuteGameplayCue(...HitImpact_Light)`

---

## 🃏 7. 컴포넌트 통신 — 직접 포인터 금지

**Q. 경직/처형 컴포넌트끼리 어떻게 통신하나?**

> **델리게이트로만, 직접 포인터 0개.** Pawn이 중개(컴포넌트A→Pawn→컴포넌트B). 컴포넌트→ASC도 캐스팅 안 하고 `IAbilitySystemInterface`(GetAbilitySystemComponent) 경유 → 어느 캐릭터에든 붙음.
> 의존성을 단방향(Pawn→컴포넌트)으로 강제 = SOLID/God-class 방지(CLAUDE.md §1-3).
> 📍 `KDEnemyBaseCharacter.cpp:118~132` 배선 / `StaggerComponent.cpp:25` 인터페이스 접근

---

## 🃏 8. 데이터드리븐 / OCP (개방-폐쇄)

**Q. 새 적/새 공격을 코드 0줄로 어떻게?**

> 코드는 *메커니즘*만, 차이는 *데이터*가. 
> - 공격 선택: `SelectAttack`이 적별 DataAsset 공격 엔트리(`{태그,거리,가중치}`)를 거리밴드+가중랜덤으로 픽. 📍 `BTTask_SelectAttack.cpp:52~107`
> - 처형: `ExecutionProfile.bSurvivable` 한 칸이 잡몹(즉사)/엘리트(생존) 가름. 📍 `ExecutionComponent.cpp:72`
> → 새 적 = DataAsset만 채움, 코드/BT 무변경.

---

## 🃏 9. 직교 3축 (적 AI)

**Q. 적 AI를 어떤 축으로 쪼갰나?**

> 서로 안 겹치는 3축, 각자 BB 키 하나만 책임:
> - **토큰**(EncounterSubsystem) = "누가 칠지" (동시공격 ≤2)
> - **EQS** = "어디 설지"
> - **MoveTo + Crowd** = "어떻게 갈지"
> → god-tree 방지. 새 포지셔닝 로직은 "어느 축 소관?"부터 분류.

---

## 🃏 10. AbilityTask / AnimNotifyState

**Q. 몽타주 재생·트레이스를 왜 AbilityTask로?**

> 시간이 걸리는 비동기 작업(몽타주, 이벤트 대기, 트레이스)은 AbilityTask로. 능력이 끝나면 태스크도 자동 정리 → 누수 방지. 직접 타이머보다 GAS 수명주기에 안전.

**Q. 윈도우별 트레이스 오버라이드(`ANS_WeaponTrace`)의 핵심?**

> 트레이스 파라미터(소켓/반지름/모드)를 GA 전역이 아니라 **AnimNotify 윈도우 단위**로 내림 → 한 몽타주가 윈도우마다 다른 모양. 비우면 GA 디폴트 폴백. 게임플레이 이벤트 페이로드(`OptionalObject`)로 동기 전달.
> 📍 `GA_WeaponTraceBase.cpp:113~125`

---

## 🃏 11. 토큰 weak 포인터

**Q. 토큰 holder를 왜 weak 포인터로?**

> 적이 죽으면 자동 null → 죽은 적이 슬롯을 영원히 잡는 걸 방지. 강한 포인터면 시체를 붙들어 GC도 안 되고 슬롯도 안 풀림.
> 📍 `EncounterSubsystem.cpp:24` (`!Slot.Holder.IsValid()` 청소)

---

## 🃏 12. 비대칭 경직

**Q. 경직 비대칭이 코드 어디에?**

> 적만 경직, 플레이어 안 함(스텔라 블레이드식). ① Poise는 적 Pawn에만(`OnHitReceived:444`) ② 패링 슬로모도 적만(`AS_Combat.cpp:68`) ③ friendly fire 차단(진영 게이트).

---

## 🔑 면접 한 줄 요약 (제일 자주 묻힐 것)

| 개념 | 한 줄 |
|---|---|
| 동기 사망 | "SetHealth(0)이 같은 콜스택에서 HandleDeath를 끌고 온다 — 대부분 모르는 GAS 깊이" |
| 패링 게이팅 | "패링 감지는 데미지가 들어와야 돈다 → 안 되면 상류 히트부터 의심" |
| 데이터드리븐 | "코드는 메커니즘, 차이는 DataAsset — 새 적 코드 0줄" |
| 컴포넌트 분리 | "델리게이트로만 통신, 직접 포인터 0 → 단방향 의존성" |
| 비대칭 | "if 분기 아니라 CDO/데이터로 진영·적종 차이 표현" |
| 근본원인 추적 | "증상≠원인. 콜스택/데이터 흐름 따라가 진범 분리(패링=트레이스, 꺾임=PhysicsAsset)" |
