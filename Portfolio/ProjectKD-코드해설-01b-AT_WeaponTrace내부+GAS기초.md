---
title: ProjectKD 코드 해설 01b — AT_WeaponTrace 내부 구현 + GAS/UObject 기초
tags:
  - ProjectKD
  - portfolio
  - code-study
  - gas
  - weapon-trace
  - ability-task
created: 2026-06-25
status: 학습용 v1 (세션2)
related:
  - "[[ProjectKD-코드해설-01-WeaponTrace패링]]"
  - "[[ProjectKD-코드해설-00-학습인덱스]]"
---

# 코드 해설 01b — AT_WeaponTrace 내부 + GAS/UObject 기초

> **이 노트의 범위**: [[ProjectKD-코드해설-01-WeaponTrace패링|01]]은 *패링 게이트(AS_Combat)·데미지 파이프라인* 중심.
> **01b(이 노트)는 트레이스 "일꾼" `AT_WeaponTrace`의 내부 구현**(서브스텝 수학·트레이스 종류·람다)과 그 밑의 **GAS/UObject 기초**(AbilityTask 수명·UPROPERTY=GC)를 다룬다.
> 줄 번호 = 실제 파일 기준 (2026-06-25). 옆에 소스 열고 대조.

**관련 파일**
- `AbilitySystem/Tasks/AT_WeaponTrace.{h,cpp}` — 트레이스 일꾼(이 노트 주인공)
- `AbilitySystem/Abilities/GA_WeaponTraceBase.cpp` — 일꾼을 켜는 지휘자(몽타주·윈도우)
- `AbilitySystem/AnimNotifies/ANS_WeaponTrace.{h,cpp}` — 윈도우 오버라이드 발신
- `AbilitySystem/Abilities/Enemy/GA_EnemyWeaponTraceBase.cpp` — per-faction CDO

---

## 1. AT_WeaponTrace 한눈 구조

`UAT_WeaponTrace : public UAbilityTask`. 콜백 3개로 산다:

| 콜백 | 시점 | 하는 일 |
|---|---|---|
| `Activate()` | 태스크 시작 | 무기 유효 검사, `bHasPrevFrame=false`, `AlreadyHitActors.Reset()` |
| `TickTask()` | **매 프레임** | 칼 위치 읽고 prev→cur 궤적 검사 (본체) |
| `OnDestroy()` | 종료 | 기록 정리 |

- 생성자에서 `bTickingTask = true` → **이게 있어야 매 프레임 `TickTask` 호출됨.** AbilityTask는 기본은 안 돈다.

### TickTask 흐름 6줄 요약
```
1. 무기 유효? 아니면 종료
2. 지금 소켓 위치 읽기 (CurStart, CurEnd)
3. [첫 틱] Prev에 저장만 하고 return  ← 판정 안 함
4. 준비물: 자기무시 Params / 찾을타입(Pawn,Destructible) / 벽검사 람다 / 히트처리 람다
5. 모드 분기: TipLine(칼끝 한 줄) / Sweep(궤적 1~8조각 캡슐 스윕)
6. Prev = Cur  (다음 틱 대비)
```

---

## 2. 줄별 해설 (AT_WeaponTrace.cpp)

### 2-1. 첫 틱 시드 (L64~70)
```cpp
if (!bHasPrevFrame) { PrevStart=CurStart; PrevEnd=CurEnd; bHasPrevFrame=true; return; }
```
- 트레이스는 **prev·cur 두 점**이 있어야 "쓸기"가 됨. 첫 틱엔 prev가 없음.
- prev를 초기값 `(0,0,0)`(월드 원점)으로 두고 검사하면 → 원점~칼끝 거대한 선을 쓸어 엉뚱한 적 히트.
- 그래서 첫 틱은 **현재 위치를 prev에 심고 return** → 둘째 틱부터 진짜 판정.

### 2-2. 자기 무시 — Params (L72, L79~81)
```cpp
AActor* Owner = GetAvatarActor();
FCollisionQueryParams Params(SCENE_QUERY_STAT(WeaponTrace), false, Owner);
Params.AddIgnoredActor(Owner);
Params.bReturnPhysicalMaterial = false;
```
- `FCollisionQueryParams` = 트레이스를 *어떻게* 검사할지 담는 설정. **무시 목록에 Owner를 넣으면** 엔진이 결과에서 그 액터를 통째로 뺌.
- 안 빼면 스윕 캡슐이 내 캡슐과 겹쳐 **자기 자신을 히트로 보고 → 자해.**
- `SCENE_QUERY_STAT(WeaponTrace)` = 프로파일러 통계 태그(로직 무관). `bReturnPhysicalMaterial=false` = 표면 재질 불필요(약간 성능).

### 2-3. 채널 vs ObjectType (L86~88) ★기초
```cpp
FCollisionObjectQueryParams ObjectParams;
ObjectParams.AddObjectTypesToQuery(ECC_Pawn);
ObjectParams.AddObjectTypesToQuery(ECC_GameTraceChannel2); // Destructible (DefaultEngine.ini)
```
충돌 컴포넌트는 둘을 가짐:
- **오브젝트 타입** = "나는 무엇인가" (딱 1개. WorldStatic/Pawn/커스텀 Destructible…)
- **응답(Response)** = "각 채널에 어떻게 반응하나" (Ignore/Overlap/Block)

| 검사 | 묻는 것 | 기준 |
|---|---|---|
| **ByChannel** | "이 채널에 어떻게 반응?" | 각 액터의 **응답 설정** (Ignore면 놓침) |
| **ByObjectType** | "정체가 이 타입?" | 액터의 **타입** (응답 무관) |

→ WeaponTrace는 **ByObjectType**. "응답 설정"이 아니라 "정체(Pawn/Destructible)"로 잡아서 캐릭터마다 채널 세팅 안 맞춰도 안정적 + 전용 트레이스 채널 유지보수 불필요.
- **Destructible**: 엔진 폐기된 `ECC_Destructible`이 아니라 **프로젝트가 직접 만든 커스텀 오브젝트 타입**(`GameTraceChannel2`, `DefaultEngine.ini`). 그래서 트레이스가 적(Pawn)+깨지는 소품을 함께 때림. ini 주석: *"WeaponTrace is NOT a channel"*.
- ⚠️ 옛 소스 주석의 "GASP 캡슐 Pawn=Ignore" 근거는 **stale**(GASP 미사용). 결정 자체는 위 일반 이유로 여전히 유효 → 면접 땐 GASP 언급 말 것.

### 2-4. 벽 가림(LoS) 람다 — WorldStatic만 (L93~98)
```cpp
WallParams.AddObjectTypesToQuery(ECC_WorldStatic);
auto IsWallBlocking = [&](Origin, End)->bool { return World->LineTraceTestByObjectType(Origin, End, WallParams, Params); };
```
- **WorldStatic** = 안 움직이는 배경(벽/바닥/기둥). Pawn과 다른 타입.
- 벽 검사에 WorldStatic만 → **다른 적(Pawn)은 시야를 안 막음.** 군집전에서 앞 적이 뒤 적을 가려 안 맞는 문제 회피. 진짜 벽만 타격 차단.

### 2-5. 히트 처리 람다 — ProcessHits (L101~115)
```cpp
auto ProcessHits = [&](const TArray<FHitResult>& Hits) {
  for (Hit : Hits) {
    if (!HitActor || HitActor==Owner) continue;             // 자기/없음
    if (AlreadyHitActors.Contains(HitActor)) continue;      // 이번 윈도우 중복
    if (IsWallBlocking(Owner->loc, Hit.ImpactPoint)) continue; // 벽
    AlreadyHitActors.Add(HitActor);
    if (ShouldBroadcastAbilityTaskDelegates()) OnHit.Broadcast(Hit);
  }
};
```
- **람다** = 함수 안에 즉석 정의하는 작은 함수. `[&]` = 주변 변수 참조 캡처(인자 안 넘겨도 씀).
- TipLine·Sweep **둘 다** 같은 거르기+브로드캐스트가 필요 → 한 번 정의해 양쪽서 호출(L128, L172).
- **벽검사/히트처리를 나눈 이유**: `IsWallBlocking`은 *부작용 없는 예/아니오 질의*, `ProcessHits`는 *기록·브로드캐스트(부작용)*. ProcessHits가 히트마다 IsWallBlocking을 호출(포함관계). 단일 책임 + 가독성.

### 2-6. Sweep 서브스텝 (L137~183) ★핵심
```cpp
const float TravelDist = (CurMid - PrevMid).Size();
constexpr float StepDist = 5.0f;
const int32 SubSteps = FMath::Clamp(FMath::CeilToInt(TravelDist/StepDist), 1, 8);
for (Step=0; Step<SubSteps; ++Step) {
  A0=(float)Step/SubSteps; A1=(float)(Step+1)/SubSteps;
  S1=Lerp(PrevStart,CurStart,A1); E1=Lerp(PrevEnd,CurEnd,A1);  // (S0/E0=A0)
  Mid0=(S0+E0)*0.5; Mid1=(S1+E1)*0.5;
  Axis1=E1-S1; HalfH1=max(Axis1.Size()*0.5, R); Rot1=MakeFromZ(Axis1).ToQuat();
  SweepMultiByObjectType(Hits, Mid0, Mid1, Rot1, ObjectParams, MakeCapsule(R,HalfH1), Params);
}
```
**왜 쪼개나 — 2단계로 이해:**
- **(1) 점 체크 → sweep**: 매 틱 *현재 위치만* 체크(Overlap)하면 프레임 간 이동 갭에 적이 끼어 **통과(터널링)**. sweep은 prev→cur를 **연속으로** 훑어 이 평행이동 터널링을 해결. (터널링은 sweep이 아니라 점샘플링의 약점)
- **(2) 단일 sweep → substep**: 그런데 단일 sweep은 **직선(현)·고정 회전** 한 번뿐. 칼은 **호(arc)** 를 그리며 **회전**함 → 곡선 바깥·각도 어긋난 칼날에 걸친 적을 놓침. 그래서 호를 짧은 직선 여러 개로 근사(미분처럼)하고 **조각마다 위치+회전 보간**.

> ⚠️ 면접 함정: "sweep은 연속인데 왜 통과?" → 터널링은 *점 체크* 약점이고, *sweep끼리*의 문제는 호·회전이라고 2단계로 답할 것.

수학 함수 실체:
- `Lerp(A,B,t) = A + (B-A)*t` — A·B 잇는 직선 위 비율 t 지점. t=0→A, 1→B, 0.5→중간. (조각 경계 위치를 뽑는 도구)
- `CeilToInt(x)` = 올림. 6cm 이동 → 6/5=1.2 → 올림 2조각(버림이면 모자람).
- `Clamp(v,1,8)` = 최소 1(가만있어도 1회), 최대 8(성능 상한).
- `(float)` 캐스팅 필수: int/int면 정수나눗셈으로 전부 0.
- `5cm` = **손으로 고른 튜닝 상수**. 캡슐 지름(반지름3×2=6)보다 살짝 작게 → 연속 캡슐이 겹쳐 빈틈 없음.
- `Axis1=E1-S1` 칼 방향+길이. `HalfH1=max(길이/2, R)` 캡슐 반높이(찌그러짐 방지 하한). `MakeFromZ(Axis).ToQuat()` = **로컬 Z를 칼 방향으로** 돌림(MakeCapsule이 캡슐 긴축을 Z로 두므로) → 캡슐을 칼 각도에 정렬. 조각마다 위치+회전 둘 다 보간.

**숫자 예**: 30cm 이동 → 30/5=6 → 6조각. 4번째: A0=3/6=0.5, A1=4/6≈0.667 → 50%·66.7% 칼 위치 사이 캡슐 스윕.

### 2-7. TipLine (L119~136)
```cpp
LineTraceMultiByObjectType(Hits, PrevEnd, CurEnd, ObjectParams, Params);
```
- 칼끝 경로(PrevEnd→CurEnd)만 한 줄 LineTrace. **서브스텝 불필요**(직선이라). 얇은 무기/SB 톤.

### 2-8. Prev 갱신 (L185~186)
```cpp
PrevStart = CurStart; PrevEnd = CurEnd;
```
- 이번 틱 끝에 현재를 직전으로 → 다음 틱이 이어서 쓸기.

---

## 3. GA 쪽 몽타주 배선 (GA_WeaponTraceBase.cpp, 보강)

### 3-1. 몽타주 태스크 생성·바인딩 (L49~59)
```cpp
MontageTask = UAbilityTask_PlayMontageAndWait::CreatePlayMontageAndWaitProxy(this, NAME_None, AttackMontage, EffRate, ...);
MontageTask->OnCompleted.AddDynamic(this, &OnMontageCompleted);
MontageTask->OnInterrupted.AddDynamic(this, &OnMontageInterrupted);
MontageTask->OnCancelled.AddDynamic(this, &OnMontageInterrupted);
MontageTask->ReadyForActivation();
```
- `...Proxy` = AbilityTask를 만드는 **정해진 공장 함수**(BP "Play Montage and Wait" 노드의 C++ 본체). `new`로 직접 못 만듦(§4).
- `AddDynamic` = 이벤트에 콜백 **구독 등록**(함수 포인터 추가).
- **결과 3종 → 핸들러 2개**: Completed=정상끝(bWasCancelled=false). Interrupted(다른 몽타주가 끼어듦)·Cancelled(어빌리티 취소)=**둘 다 "중단"이라 같은 처리**(bWasCancelled=true). 셋 다 바인딩하는 이유 = 어떻게 끝나든 어빌리티가 **반드시 종료**돼야(안 그럼 매달림). 백업=SafetyTimer.

### 3-2. EffRate (L41)
```cpp
const float EffRate = GetEffectiveMontagePlayRate(); // virtual, 자식이 공격속도 반영 가능
```
- **한 번 뽑아 두 곳에 공유**: 몽타주 재생속도(L50) + 세이프티 타이머(L71). 타이머는 "실제 소요=길이÷rate"라 같은 값을 써야 어긋남 없음.
- `Eff` 접두사 = "오버라이드 반영된 최종값" 컨벤션(EffStartSocket/EffMode/EffRadius도 동일).

### 3-3. 윈도우 오버라이드 왕복 (요약, 상세는 01 §2-2)
노티(`ANS_WeaponTrace`)가 `Payload.OptionalObject=this`로 **자기(오버라이드 포함)** 를 실어 `Event.Montage.TraceBegin` 발신 → GA의 `WaitGameplayEvent`가 받아 `OnTraceBeginEvent` → **GA 기본값 위에 채워진 칸만 덮어씀**(NAME_None/0/false=상속) → 그 최종값으로 AT_WeaponTrace 생성. 같은 몽타주가 윈도우마다 다른 소켓/반경/모드. **동기 디스패치**라 노티 수명 안전(저장 안 함).

---

## 4. GAS/UObject 기초 ★면접 단골

### 4-1. UPROPERTY = "에디터 메타데이터"가 아니라 리플렉션 등록
- 본질 역할 = **GC 추적.** GC는 UPROPERTY로 등록된 포인터만 "참조"로 인식.
- 맨몸 `UObject* p;` → GC 눈에 안 보임(안 살림 + 죽어도 null 안 함=댕글링). `UPROPERTY() UObject* p;` → 살려두고 죽으면 자동 null.
- 에디터 노출은 **지정자(EditAnywhere 등) 붙였을 때만**. 맨몸 `UPROPERTY()` = "GC만 추적, 노출 X". (`ActiveTraceTask`/`AlreadyHitActors`가 그 케이스)

### 4-2. AbilityTask 수명·소유 — 왜 NewAbilityTask로 만드나
AbilityTask는 **자기를 시작한 함수보다 오래 사는 비동기 객체**(`ActivateAbility`는 즉시 리턴, 몽타주 태스크는 수 초간 산다). 그래서 누가 붙잡고·정리해야:
1. **GC 방지**: 로컬 포인터는 함수 끝나면 사라짐 → 등록 안 하면 실행 중 GC가 삭제. `NewAbilityTask<>`가 **소유 어빌리티의 활성 태스크 목록**(`TArray<TObjectPtr<UGameplayTask>> ActiveTasks;` = 맨몸 UPROPERTY 배열)에 등록 → GC가 안 지움.
2. **어빌리티 종료 시 자동 정리**: `EndAbility` → GAS가 그 목록을 돌며 모든 태스크 `EndTask/OnDestroy`. 소유 등록이 *유일한 이유*. 안 하면 죽은 어빌리티에 콜백 쏨(use-after-free).
3. **틱·브로드캐스트 게이팅**: `ShouldBroadcastAbilityTaskDelegates()`가 "주인 어빌리티 살아있나"를 답할 수 있는 것도 소유를 알기 때문.
- 그래서 `new`/`NewObject` 금지, **팩토리(→NewAbilityTask) + ReadyForActivation** 필수.

### 4-3. GameplayTask vs AbilityTask
```
UGameplayTask  (범용 비동기, 소유자 = IGameplayTaskOwnerInterface 아무거나; AI MoveTo 등)
   └── UAbilityTask  (GAS 어빌리티 전용; 소유자=GameplayAbility 필수)
          └── UAT_WeaponTrace, PlayMontageAndWait, WaitGameplayEvent ...
```
- AbilityTask = GameplayTask + "나는 이 어빌리티 소속, GAS 수명에 묶임"(주인 어빌리티/ASC 포인터, ShouldBroadcast…, 자동 정리). ASC는 `UGameplayTasksComponent`를 상속해 태스크 본부 역할(틱 구동).

---

## 5. per-faction CDO + 2층 중복 방지

**CDO(Class Default Object)** = 모든 UCLASS의 "기본값 견본 인스턴스". 생성자에서 준 값이 기본값. 인스턴스는 여기서 복사.
```cpp
// GA_WeaponTraceBase.h        : bool bOncePerActor = true;   (플레이어 기본)
// GA_EnemyWeaponTraceBase()   : bOncePerActor = false;       (적만 덮음)
```
런타임 `if(적)` 분기가 아니라 **클래스 기본값**으로 비대칭 표현. 새 진영=새 자식+기본값만, 분기 안 늘어남.

**중복 히트 방지 2층:**
- **1층 (항상)**: AT의 `AlreadyHitActors` = **한 윈도우(태스크) 안** 중복 차단. 트레이스가 매 프레임 도니 안 막으면 닿는 동안 매 프레임 히트(틱 연사).
- **2층 (`bOncePerActor`)**: GA의 `AlreadyHitActors` = **공격 1회 전체**(여러 윈도우) 중복 차단.
  - 플레이어 `true`: 윈도우 3개여도 한 공격 = 같은 적 1대(깔끔 콤보).
  - 적 `false`: 2층 끔 → 윈도우마다 1대(다단 3연속이면 최대 3대). **이유**: 베기 하나하나가 독립 패링 대상이어야 재밌음(2층 켜지면 첫 타만 진짜 타격 → 2·3타 패링 불가). 틱 연사는 1층이 막아줌.

---

## 6. 예상 면접 Q&A (01b 범위)

**Q1. 서브스텝은 왜 필요한가요? 엔진 sweep이 연속인데?**
> 한 번의 sweep은 시작점과 끝점을 직선으로만 잇습니다. 칼은 호를 그리며 휘둘러져서, 한 번 sweep이면 곡선 바깥의 적을 놓칩니다. 그래서 prev→cur 궤적을 이동거리÷5cm로 1~8조각 내고, 각 조각마다 위치와 회전을 보간해 캡슐을 스윕합니다. 저프레임 빠른 스윙에서도 호를 따라 빈틈없이 잡힙니다.

**Q2. 5cm는 어떻게 정했나요?**
> 유도값이 아니라 튜닝 상수입니다. 캡슐 지름(반지름 3 → 6cm)보다 살짝 작게 잡아 연속 캡슐이 겹치게 했습니다. 작을수록 정확하지만 횟수가 늘어 최대 8로 상한을 뒀습니다. (개선안: StepDist를 반지름에서 유도하면 큰 적 히트구의 과샘플링을 없앨 수 있음 — 미구현 아이디어)

**Q3. 왜 ByChannel이 아니라 ByObjectType인가요?**
> 채널 검사는 각 액터의 채널 응답 설정에 의존해서, 어떤 캡슐이 그 채널을 Ignore로 두면 놓칩니다. ObjectType 검사는 "정체가 Pawn/Destructible인가"로 판정해 응답 설정과 무관하게 잡힙니다. 전용 트레이스 채널을 유지보수할 필요도 없고요. Destructible은 프로젝트에서 만든 커스텀 오브젝트 타입이라 적과 파괴물을 한 번에 때립니다.

**Q4. 벽 검사를 WorldStatic으로만 한 이유는?**
> 다른 폰이 시야를 막지 않게 하려고요. 벽(WorldStatic)만 가림으로 치면, 군집 전투에서 앞 적 뒤의 적도 맞고 진짜 벽 뒤 적만 안 맞습니다.

**Q5. AbilityTask는 왜 NewObject 대신 팩토리로 만드나요?**
> AbilityTask는 시작 함수보다 오래 사는 비동기 객체입니다. 팩토리(NewAbilityTask)가 소유 어빌리티의 활성 태스크 목록에 등록해줘야 (1) GC가 실행 중에 안 지우고 (2) 어빌리티가 끝날 때 자동으로 같이 정리됩니다. 안 그러면 죽은 어빌리티에 콜백이 날아가 크래시가 납니다.

**Q6. UPROPERTY를 붙이는 이유는요? (맨몸 포인터와 차이)**
> 핵심은 GC 추적입니다. UPROPERTY가 없으면 GC가 그 포인터를 못 봐서 대상을 살려두지도, 죽었을 때 null로 바꿔주지도 않습니다. 에디터 노출은 EditAnywhere 같은 지정자를 붙였을 때의 부가 기능이고요.

**Q7. 적/플레이어 히트 규칙 차이를 왜 if가 아니라 CDO로?**
> "플레이어=1스윙1히트, 적=윈도우당1히트"는 진영의 정적 성질이라 매 히트 if를 타는 것보다 클래스 기본값이 맞습니다. 중복 방지는 2층인데, 태스크 레벨(윈도우 내)은 항상 켜고, GA 레벨(공격 전체)만 bOncePerActor로 켜고 끕니다. 적은 꺼서 다단 베기 하나하나가 독립 패링 대상이 됩니다.

---

## 7. 직접 설명 연습 (점검)

보지 말고 소리내어:
1. 첫 틱에 판정을 건너뛰는 이유 (prev 없음 → 원점 쓸기)
2. 서브스텝이 푸는 문제 (직선 sweep이 호 바깥을 놓침) + Lerp/CeilToInt/Clamp 각각 뭐 하는지
3. ByChannel vs ByObjectType 차이를 한 문장
4. 벽 LoS를 WorldStatic으로만 한 이유
5. AbilityTask를 팩토리로 만드는 3가지 이유(GC/자동정리/게이팅)
6. 중복 방지 2층 + per-faction CDO

> 6개 막힘없이 = AT_WeaponTrace 내부를 "네 것"으로 설명 가능.

---

## 참고 — 이번 세션 곁가지(코드 외)
- 저작 정정: `ANS_CancelWindow`=본인(태그 기반 캔슬 토대), `ANS_EnemyAttackWindow`·`ANS_MovementCancel`=팀원.
- 인프라 해체(2026-06-25): Perforce/AWS/버전관리 정리. 포폴엔 "구축·운영했다" 과거형 유지.
- 미학습(다음): GA_EnemyWeaponTraceBase의 OnActivated 플랜트/토큰반납/텔레그래프 큐 → 그리고 ② EncounterSubsystem.
