# UE 5.7 Game Animation Sample (GASP) + Mover Plugin 학습 가이드

> **작성일**: 2026-03-20
> **대상 엔진**: Unreal Engine 5.7
> **GASP 다운로드**: [Fab 링크](https://www.fab.com/listings/880e319a-a59e-4ed2-b268-b32dac7fa016)
> **주의**: Mover Plugin은 현재 **Experimental** 상태. API 변경 가능성 있음.

---

## 목차

1. [[#1. GASP란 무엇인가]]
2. [[#2. GASP 5.4 → 5.7 변경사항 요약]]
3. [[#3. Mover Plugin 핵심 개념]]
4. [[#4. CMC vs Mover 비교표]]
5. [[#5. Mover 아키텍처 상세]]
6. [[#6. 새 프로젝트에 Mover 적용하기]]
7. [[#7. 애니메이션 파이프라인]]
8. [[#8. Motion Matching + Chooser 시스템]]
9. [[#9. 실전 구현 패턴]]
10. [[#10. 마이그레이션 전략]]
11. [[#11. 공식 리소스 & 참고자료]]

---

## 1. GASP란 무엇인가

**Game Animation Sample Project (GASP)**는 Epic Games가 UE 5.4부터 제공하는 무료 프로젝트로:

- **500+ AAA급 애니메이션** 무료 제공
- 캐릭터 애니메이션 **베스트 프랙티스** 시연
- Motion Matching, Mover, Control Rig 등 **최신 시스템 활용 사례**
- 누구든 자신의 프로젝트에 **분해해서 재사용** 가능

커뮤니티에서는 줄여서 **"GASP"** 로 부름.

---

## 2. GASP 5.4 → 5.7 변경사항 요약

### 핵심 변경점 한눈에 보기

| 영역 | 5.4 (최초 릴리즈) | 5.7 (최신) |
|------|-------------------|------------|
| **이동 시스템** | CharacterMovementComponent (CMC) | **Mover Plugin** (Experimental) |
| **캐릭터** | CMC 기반 캐릭터 1종 | CMC 캐릭터 + **Mover 캐릭터** 추가 |
| **애니메이션 수** | ~100+ | **500+ (400개 신규 추가)** |
| **로코모션 데이터셋** | 반응성 최우선 (품질 희생) | **반응성 + 품질 균형** (데이터량도 감소) |
| **이동 모드** | 기본 걷기/달리기/점프 | **Spring Walking, Smooth Walking, Sliding** 추가 |
| **스마트 오브젝트** | 없음 | **Smart Object 레벨 + NPC 셋업** |
| **Control Rig** | 기본 IK | **Foot Placement Control Rig 노드** 추가 |
| **Chooser 시스템** | 기본 | **Pose Search Column** (Motion Matching 통합) |
| **UAF** | 미포함 | 기반 작업 시작 (5.8에서 본격 공개 예정) |

### 신규 추가 기능 상세

#### 2.1 Mover Plugin 통합
- CMC를 대체할 **차세대 이동 시스템**
- Rollback Networking 기본 지원
- Mover 전용 캐릭터가 GASP에 추가됨

#### 2.2 새로운 Walking Mode 2종
- **Simple Spring Walking Mode**: 속도/회전 각각 독립 스프링으로 자연스러운 움직임
- **Smooth Walking Mode**: 레거시 CMC와 유사하지만 더 부드러운 속도/회전 전환
- 둘 다 `SimpleWalkingMode` 추상 베이스 클래스에서 파생 → **C++/BP로 확장 가능**

#### 2.3 슬라이드 메카닉
- 바닥 슬라이딩 + **경사면에 따른 속도 변화**
- Blueprint로 Mover 확장하는 **예제 역할**

#### 2.4 Smart Object + NPC
- NPC가 벤치에 **여러 각도에서 접근하여 앉는** 데모
- Smart Object + Warping 개선 시연

#### 2.5 Foot Placement Control Rig
- AnimBP 내 프로시저럴 노드를 **Control Rig로 이전** 시작
- UAF(Unreal Animation Framework) 전환 준비

#### 2.6 Pose Search Column in Choosers
- Chooser Table에서 **PoseSearchDatabase를 동적 선택** 가능
- Motion Matching과 Chooser 시스템의 **긴밀한 통합**

---

## 3. Mover Plugin 핵심 개념

### 3.1 Mover란?

> Mover는 Actor의 이동을 모듈식으로 처리하며, Network Prediction Plugin 또는 Chaos Networked Physics를 통한 **롤백 네트워킹**을 지원하는 플러그인이다.

**핵심 설계 목표:**
- 게임플레이 개발자가 **네트워킹 전문가 없이** 모션을 제작할 수 있도록
- 어떤 Actor 타입이든 사용 가능 (ACharacter에 종속되지 않음)
- 모듈식 아키텍처로 **모드 단위 확장** 가능

### 3.2 핵심 클래스 구조

```
UMoverComponent                    ← 이동 관리의 "두뇌"
├── Movement Modes                 ← 이동 규칙 정의
│   ├── WalkingMode               ← 지상 보행
│   ├── FallingMode               ← 낙하/공중
│   ├── FlyingMode                ← 비행
│   ├── SwimmingMode              ← 수영
│   ├── SimpleSpringWalkingMode   ← 스프링 기반 (신규)
│   ├── SmoothWalkingMode         ← 부드러운 보행 (신규)
│   └── [Custom Modes...]        ← 사용자 확장
│
├── Layered Moves                  ← 임시 추가 이동
│   ├── AnimRootMotion            ← 몽타주 루트모션
│   ├── JumpImpulse               ← 점프 임펄스
│   ├── Launch                    ← 발사 (넉백 등)
│   ├── LinearVelocity            ← 직선 속도 적용
│   ├── MoveTo / MoveToDynamic    ← 목표 지점 이동
│   └── RadialImpulse             ← 방사형 밀기/당기기
│
├── Input Producers                ← 입력 제공자
│   └── IMoverInputProducerInterface
│
└── MoverBlackboard                ← 시스템간 상태 공유
```

### 3.3 세 가지 핵심 구성요소

#### Movement Mode (이동 모드)
- 입력을 관찰하여 **어떻게 이동할지 결정**하는 객체
- 한 번에 **하나만 활성화** 됨
- 예: Walking → 바닥 감지 실패 시 자동으로 → Falling

#### Layered Move (레이어드 무브)
- **일시적인 추가 이동**을 나타내는 객체
- 점프, 넉백, 루트 모션 등
- ProposedMove를 생성하고 **활성 모드가 실행**
- **복수 동시 활성 가능**

#### Input Producer (입력 생산자)
- **CMC와 가장 큰 패러다임 차이!**
- CMC: 플레이어가 직접 `AddMovementInput()`을 **Push**
- Mover: Mover가 매 틱마다 Input Producer에게 입력을 **Pull (요청)**

```
CMC 방식 (Push):
  플레이어 입력 → 직접 AddMovementInput() → 즉시 이동

Mover 방식 (Pull):
  플레이어 입력 → Pawn에 캐시 → Mover가 매 틱 ProduceInput() 호출 → 이동
```

### 3.4 실행 흐름

```
매 시뮬레이션 틱마다:

1. ProduceInput()      → Input Producer에서 입력 데이터 수집
       │
2. TransitionCheck()   → 모드 전환 필요 여부 판단
       │
3. GenerateMove()      → 활성 모드가 ProposedMove 생성
       │
4. MixLayeredMoves()   → Layered Move들과 결합
       │
5. SimulationTick()    → 물리/이동 시뮬레이션 실행
       │
6. Finalize()          → Actor Transform에 적용
```

---

## 4. CMC vs Mover 비교표

| 항목 | **Mover** | **CMC** |
|------|-----------|---------|
| **네트워크 물리** | 지원 (Chaos) | 제한적 |
| **시뮬레이션 주도** | 서버 주도 (Unified) | 클라이언트 RPC |
| **상태 접근** | 보호됨 (API 통해서만) | 직접 접근 가능 |
| **클라이언트 보정** | 클라이언트 측 보정 | 서버 측 보정 |
| **롤백** | 통합 롤백 | 격리된 롤백 |
| **충돌 형태** | 어떤 형태든 가능 | 캡슐만 가능 |
| **Actor 타입** | 아무 Actor | ACharacter 필수 |
| **아키텍처** | 모듈식 (조립) | 모놀리식 (하나의 거대 클래스) |
| **Z축 가정** | 없음 (어떤 축이든 "위") | +Z가 위로 가정 |
| **멀티스레드** | 지원 예정 | 미지원 |
| **안정성** | ⚠️ Experimental | ✅ 수많은 출시작에서 검증 |

### 핵심 차이 요약

```
CMC = "만능 스위스 아미 나이프" 
  → 기능 많지만 무겁고, 수정 어려움, 캡슐+ACharacter 강제

Mover = "레고 블록 조립식"
  → 필요한 모드만 조합, 어떤 Actor든 사용, 네트워크 우선 설계
```

---

## 5. Mover 아키텍처 상세

### 5.1 왜 ACharacter 대신 APawn을 사용하는가?

`ACharacter`의 C++ 소스를 보면, 이 클래스는 다음과 하드코딩으로 결합되어 있다:
- 캡슐 충돌
- 스켈레탈 메시
- **CharacterMovementComponent (CMC)**

Mover라는 "새 심장"을 이식하려는데, "옛 심장(CMC)"이 깊이 박혀 있으므로:

> **베스트 프랙티스**: 최소한의 `APawn`을 상속받아 필요한 컴포넌트만 직접 조립

### 5.2 권장 클래스 계층 (3레이어)

```
① AGCFPawn (Pawn의 본질)
   └─ 책임: 컨트롤러 빙의 수락, 이동 의도 캐싱
   └─ 형태: 최소한의 Sphere Collision

② AGCFAvatarPawn (충돌 형태에 무관한 몸체)
   └─ 책임: SkeletalMesh, 점프 같은 범용 액션
   └─ 확장성: 4족 보행, 비행 드론도 가능

③ AGCFHumanoid (인간형 전용 도메인)
   └─ 책임: CapsuleComponent (앉기 등 캡슐 의존 기능)
   └─ DoNotCreateDefaultSubobject로 부모 Sphere 제거
```

### 5.3 Input Producer 구현 패턴

#### 입력 전달 흐름 ("버킷 릴레이")

```
① Controller [Push]
   └─ Enhanced Input으로 입력 감지
   └─ 카메라 기준 방향 계산
   └─ 인터페이스를 통해 Pawn에 "의도" 전달

        ↓

② Pawn [Cache]
   └─ 물리적 이동 없이 변수에 저장
   └─ CachedMoveInput, CachedMoveRotation
   └─ CachedTargetMovement 사전 계산

        ↓

③ Input Producer [Pull]
   └─ Mover가 매 틱 ProduceInput() 호출
   └─ Pawn에서 캐시된 값을 가져와 FMoverInputCmdContext로 변환
   └─ Mover에 전달
```

#### Input Producer 코드 예시

```cpp
void UMyInputProducer::ProduceInput_Implementation(
    int32 SimTime, FMoverInputCmdContext& InputCmdResult)
{
    APawn* OwnerPawn = Cast<APawn>(GetOwner());
    if (!OwnerPawn) return;

    FCharacterDefaultInputs& InputData = 
        InputCmdResult.InputCollection
        .FindOrAddMutableDataByType<FCharacterDefaultInputs>();
    
    FVector DesiredMove = FVector::ZeroVector;

    // 로컬 플레이어만 입력 생성 (시뮬레이티드 프록시는 네트워크 동기화)
    if (OwnerPawn->IsLocallyControlled())
    {
        if (OwnerPawn->Implements<UMyInputProvider>())
        {
            DesiredMove = IMyInputProvider::Execute_GetDesiredMovementVector(OwnerPawn);
        }
    }

    InputData.SetMoveInput(EMoveInputType::DirectionalIntent, DesiredMove);
    InputData.OrientationIntent = OwnerPawn->GetControlRotation().Vector();
}
```

#### 자동 등록 매직

Input Producer를 **컴포넌트화**하면, `UMoverComponent::BeginPlay()`에서 자동 감지:

```cpp
// 엔진 내부 코드 (MoverComponent.cpp)
for (UActorComponent* Component : MyActor->GetComponents())
{
    if (Component->GetClass()->ImplementsInterface(
        UMoverInputProducerInterface::StaticClass()))
    {
        InputProducers.AddUnique(Component);  // 자동 등록!
    }
}
```

→ 블루프린트에서 컴포넌트만 붙이면 수동 바인딩 불필요

---

## 6. 새 프로젝트에 Mover 적용하기

### 6.1 플러그인 활성화

```
Edit → Plugins → 검색: "Mover"
  ✅ Mover
  ✅ Mover Examples
에디터 재시작
```

### 6.2 프로젝트 설정

```
Edit → Project Settings → Network Prediction:

  ☑ Preferred Ticking Policy: Fixed
  ☑ Simulated Proxy Network LOD: Interpolated
  ☑ Enable Fixed Tick Smoothing: True
  ☑ Fixed Tick Interpolation Buffered MS: 100
```

> ⚠️ 이 설정이 없으면 멀티플레이어에서 심한 **스터터링** 발생!
> 변경 시 `Config/DefaultNetworkPrediction.ini` 자동 생성 → Git 커밋에 포함할 것

### 6.3 Blueprint 셋업 (가장 쉬운 방법)

1. **Mover Examples의 `BaseAnimatedMannyPawn`** 을 부모로 BP 생성
2. 또는: 커스텀 Pawn에 다음 컴포넌트 추가
   - `CharacterMoverComponent` (MoverComponent의 캐릭터 전문화)
   - `LocomotionInputProducer` (입력 변환기)

3. CharacterMoverComponent Details 패널에서:
   - **Backend Class**: `MoverNetworkPredictionLiaisonComponent`
   - **Starting Movement Mode**: `Falling` (공중 스폰 시 안전 착지)

4. `CharacterMoverComponent`를 사용하면 **Walking, Falling, Flying** 모드가 자동 셋업됨

### 6.4 C++ 최소 셋업

```cpp
UCLASS()
class AMyMoverPawn : public APawn
{
    GENERATED_BODY()

public:
    AMyMoverPawn()
    {
        // 캡슐 충돌
        CapsuleComp = CreateDefaultSubobject<UCapsuleComponent>(TEXT("Capsule"));
        CapsuleComp->InitCapsuleSize(40.f, 90.f);
        RootComponent = CapsuleComp;

        // 스켈레탈 메시
        MeshComp = CreateDefaultSubobject<USkeletalMeshComponent>(TEXT("Mesh"));
        MeshComp->SetupAttachment(RootComponent);
        MeshComp->SetCollisionEnabled(ECollisionEnabled::NoCollision); // 중요!

        // Mover
        MoverComp = CreateDefaultSubobject<UCharacterMoverComponent>(TEXT("Mover"));
    }
};
```

> ⚠️ **메시 충돌을 NoCollision으로!** 자식 컴포넌트에 충돌이 있으면 Mover의 텔레포트 로직과 충돌하여 버그 발생 (알려진 이슈 UE-363516)

### 6.5 Shared Settings 파라미터 조정

Movement Mode의 **Shared Settings** (`CommonLegacyMovementSettings`)에서:
- Max Walk Speed
- Jump Force
- Max Walkable Slope
- Ground/Air Movement Mode Name

→ 여러 모드 간 **공유 파라미터**로 효율적 조정

### 6.6 Transition 규칙

> Transitions 배열은 **비워둬도 된다!**

각 Movement Mode 내부 C++ 코드에서 물리 체크 수행:
- "바닥이 사라지면 Air 모드로"
- "착지하면 Ground 모드로"

실제 전환 대상은 Shared Settings의 프로퍼티 문자열로 결정 → **완전한 Data-Driven 메커니즘**

---

## 7. 애니메이션 파이프라인

### 7.1 전체 데이터 흐름

```
플레이어 입력
    │
    ▼
┌─────────────────────┐
│   MOVER COMPONENT   │ ← 차세대 이동 (CMC 대체)
│  Movement Modes     │     • Walking (Spring/Smooth)
│  Layered Moves      │     • Falling, Sliding, Flying
└──────────┬──────────┘
           │ State Query API
           ▼
┌─────────────────────┐
│  ANIM BLUEPRINT     │
│  • Pose History     │ ← 과거 포즈 롤링 윈도우
│  • Trajectory Calc  │ ← 미래 위치 예측 (Motion Matching용)
│  • Mover State      │ ← 현재 이동 모드 쿼리
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   CHOOSER TABLE     │ ← Pose Search Column (5.7 신규)
│  Context Variables  │ ← 상황별 DB 동적 선택
│  Smart Objects      │ ← NPC 행동 통합
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  MOTION MATCHING    │
│  PoseSearchDatabase │ ← 최적 애니메이션 프레임 선택
│  Schema Channels    │ ← Velocity, Trajectory, Bones
│  Blend Transitions  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  CONTROL RIG        │ ← Foot Placement IK (5.7 신규)
│  Procedural Nodes   │ ← Warping, IK, 지형 적응
└──────────┬──────────┘
           │
           ▼
    최종 애니메이션 출력
```

### 7.2 AnimInstance에서 Mover 데이터 읽기

#### 핵심 원칙: 태그 기반 상태 관리 (캐스팅 금지!)

```cpp
// ❌ 나쁜 방법: 특정 클래스로 캐스팅 → 컴포넌트 교체 시 깨짐
auto* CharMover = Cast<UCharacterMoverComponent>(MoverComp);
bool bFalling = CharMover->IsAirborne();

// ✅ 좋은 방법: GameplayTag로 상태 쿼리 → 어떤 Mover든 동작
bool bFalling = MoverComp->HasGameplayTag(Mover_IsFalling, true);
bool bCrouched = MoverComp->HasGameplayTag(Mover_IsCrouching, true);
```

#### 스레드 안전 AnimInstance 구현

```cpp
// ① 게임 스레드: 컴포넌트에서 안전하게 데이터 수집
void UMyAnimInstance::NativeUpdateAnimation(float DeltaSeconds)
{
    Super::NativeUpdateAnimation(DeltaSeconds);
    if (!MoverComponent) return;

    // 게임 스레드에서만 컴포넌트 접근
    Velocity = MoverComponent->GetVelocity();
    CachedActorRotation = OwningPawn->GetActorRotation();
    CurrentMovementMode = MoverComponent->GetMovementModeName();

    // 태그 기반 상태 판단
    bIsFalling = MoverComponent->HasGameplayTag(Mover_IsFalling, true);
    bIsCrouched = MoverComponent->HasGameplayTag(Mover_IsCrouching, true);
    bHasAcceleration = HasAcceleration();
}

// ② 워커 스레드: 캐시된 데이터로 무거운 계산
void UMyAnimInstance::NativeThreadSafeUpdateAnimation(float DeltaSeconds)
{
    Super::NativeThreadSafeUpdateAnimation(DeltaSeconds);

    VerticalVelocity = Velocity.Z;
    GroundSpeed = Velocity.Size2D();
    MovementDirection = UKismetAnimationLibrary::CalculateDirection(
        Velocity, CachedActorRotation);
    bShouldMove = GroundSpeed > 3.0f;
}
```

#### 멀티플레이어 가속도 판단

```cpp
bool UMyAnimInstance::HasAcceleration() const
{
    if (!MoverComponent || !OwningPawn) return false;

    // 다른 플레이어 화면 (Simulated Proxy)
    if (OwningPawn->GetLocalRole() == ROLE_SimulatedProxy)
    {
        // 네트워크 동기화된 SyncState에서 이동 의도 읽기
        auto* SyncState = MoverComponent->GetSyncState()
            .SyncStateCollection.FindDataByType<FMoverDefaultSyncState>();
        if (SyncState)
            return !SyncState->MoveDirectionIntent.IsNearlyZero(0.01f);
    }
    // 내 화면 (Local / Authority)
    else
    {
        // 최신 InputCmd에서 직접 이동 의도 읽기
        FMoverInputCmdContext LastInput = MoverComponent->GetLastInputCmd();
        auto* Inputs = LastInput.InputCollection
            .FindDataByType<FCharacterDefaultInputs>();
        if (Inputs)
            return !Inputs->GetMoveInput().IsNearlyZero(0.01f);
    }
    return false;
}
```

---

## 8. Motion Matching + Chooser 시스템

### 8.1 Motion Matching이란?

> State Machine의 복잡성을 제거하고, 데이터베이스에서 **현재 궤적(Trajectory)에 가장 맞는 애니메이션 프레임을 자동 선택**하는 시스템.

### 8.2 핵심 구성요소

| 구성요소 | 역할 |
|---------|------|
| **PoseSearchDatabase** | 애니메이션 시퀀스를 특성 데이터와 함께 인덱싱 |
| **Motion Matching Node** | DB를 쿼리하여 최적 포즈 선택 |
| **Pose History** | 과거 포즈의 롤링 버퍼 (연속성 유지) |
| **Schema** | 매칭할 특성 정의 (속도, 본 위치, 궤적) |

### 8.3 Schema Channels (매칭 기준)

```
Schema Channels:
├── Trajectory        → 미래 위치/회전 예측
├── Velocity          → 캐릭터 속도
├── Bone Positions    → 특정 본 상태
└── Custom Channels   → 프로젝트 맞춤 특성
```

### 8.4 Chooser Table (동적 에셋 선택)

| Chooser 타입 | 용도 |
|-------------|------|
| **Proxy/Proxy Table** | 애니메이션 세트 선택 (무기 타입별 등) |
| **Chooser Table** | 개별 애니메이션 선택 (피격 반응 등) |
| **Pose Search Column** (5.7 신규) | **Chooser + Motion Matching 통합** |

5.7의 Pose Search Column으로:
- 게임 상태에 따라 **PoseSearchDatabase를 동적 전환**
- Smart Object와 결합하여 **NPC 행동 제어**

### 8.5 Warping 기법

| Warping 종류 | 목적 |
|-------------|------|
| **Stride Warping** | 보폭을 속도에 맞춤 |
| **Orientation Warping** | 캐릭터를 이동 방향으로 회전 |
| **Slope Warping** | 경사면에 적응 |
| **Velocity Warping** | 속도 변화에 세밀 조정 |

5.7에서 이들을 **Control Rig**로 구현 → UAF 전환 준비

---

## 9. 실전 구현 패턴

### 9.1 점프 구현 (Opt-In 설계)

**핵심**: 인터페이스 기반으로 "점프할 수 있는 Pawn"만 점프 지원

```cpp
// Controller → Pawn으로 점프 의도 Push
void UMyActionComponent::Input_Jump(const FInputActionValue& Value)
{
    APawn* Pawn = GetPawn<APawn>();
    if (Pawn && Pawn->Implements<UMyJumpHandler>())
    {
        IMyJumpHandler::Execute_HandleJumpInput(Pawn, true);
    }
}

// Pawn에서 캐시
void AMyPawn::HandleJumpInput_Implementation(bool bPressed)
{
    CachedWantsToJump = bPressed;
}

// Input Producer에서 Pull
InputData.bWantsToJump = OwnerPawn->GetCachedJumpInput();
```

> **UHT 매직**: `BlueprintNativeEvent` 함수를 구현하지 않은 클래스에서는 자동으로 기본값(`false`) 반환 → 빈 함수를 쓸 필요 없음

### 9.2 슬라이드 구현 (커스텀 Movement Mode 예시)

GASP 5.7의 슬라이딩은 **Blueprint로 Mover를 확장하는 방법**을 보여주는 예제:
- `SimpleWalkingMode`를 상속
- 경사면 감지 + 속도 가감속 로직 추가
- Blueprint에서 완전히 제작 가능

### 9.3 CMC/Mover 듀얼 모드 설계

```cpp
// bUseMoverComponent 플래그로 런타임 전환
void AMyPawn::HandleMoveInput(const FVector2D& Input, const FRotator& Rotation)
{
    if (bUseMoverComponent)
    {
        // Mover 방식: 캐시만 함 (Pull 대기)
        CachedMoveInput = Input;
        CachedMoveRotation = Rotation;
        UpdateCachedTargetMovement();
    }
    else
    {
        // CMC 방식: 즉시 Push
        AddMovementInput(Rotation.RotateVector(FVector::ForwardVector), Input.X);
        AddMovementInput(Rotation.RotateVector(FVector::RightVector), Input.Y);
    }
}
```

→ 하나의 Blueprint에서 **체크박스 하나로 CMC/Mover A/B 테스트** 가능

---

## 10. 마이그레이션 전략

### 10.1 시나리오별 권장 전략

| 시나리오 | 권장 |
|---------|------|
| **새 프로젝트** (UE 5.7+) | ✅ Mover로 시작 (학습 투자 가치 있음) |
| **멀티플레이어 중심** | ✅ Mover (롤백 네트워킹 기본 제공) |
| **출시 임박한 프로젝트** | ⚠️ CMC 유지 (Mover는 Experimental) |
| **기존 CMC 프로젝트** | 점진적 마이그레이션 (아래 참조) |

### 10.2 점진적 마이그레이션 방법

```
Phase 1: 공존
  └─ ACharacter + CMC 유지
  └─ CharacterMoverComponent 추가 (CMC와 나란히)
  └─ CMC tick 비활성화 또는 movement mode "None"
  └─ 두 시스템 간 데이터 중계

Phase 2: 전환
  └─ 새 Pawn 클래스에 Mover + AnimNext 구성
  └─ 애니메이션 그래프를 UAF로 마이그레이션
  └─ CMC 의존 GAS Ability를 MoverData 기반으로 교체

Phase 3: 정리
  └─ CMC 코드 제거
  └─ ACharacter → APawn 전환
```

### 10.3 주요 주의사항

- 일부 애니메이션 노드 (루트 모션, 슬로프 워핑)에 **커스텀 대체 필요**
- GAS Character Jump Ability → **MoverData 기반 교체 필요**
- 멀티스레드 시뮬레이션 아직 개발 중
- **API 변경 가능** (Beta 전까지)

---

## 11. 공식 리소스 & 참고자료

### 공식 문서

| 리소스 | URL |
|--------|-----|
| Epic Tech Blog (메인 발표) | [GASP 5.7 업데이트](https://www.unrealengine.com/en-US/tech-blog/explore-the-updates-to-the-game-animation-sample-project-in-ue-5-7) |
| GASP 공식 문서 | [Game Animation Sample](https://dev.epicgames.com/documentation/en-us/unreal-engine/game-animation-sample-project-in-unreal-engine) |
| Mover 공식 문서 | [Mover in UE](https://dev.epicgames.com/documentation/en-us/unreal-engine/mover-in-unreal-engine) |
| Mover vs CMC 비교 | [Comparing Mover and CMC](https://dev.epicgames.com/documentation/en-us/unreal-engine/comparing-mover-and-character-movement-component-in-unreal-engine) |
| Mover 기능 & 개념 | [Features and Concepts](https://dev.epicgames.com/documentation/en-us/unreal-engine/mover-features-and-concepts-in-unreal-engine) |
| Mover Examples 가이드 | [Mover Examples](https://dev.epicgames.com/documentation/en-us/unreal-engine/mover-examples-in-unreal-engine) |
| GASP 다운로드 (Fab) | [Fab 링크](https://www.fab.com/listings/880e319a-a59e-4ed2-b268-b32dac7fa016) |

### 영상 자료

| 영상 | URL |
|------|-----|
| **GASP - It's Mover! Inside Unreal Q&A** (3시간) | [YouTube](https://www.youtube.com/watch?v=i27eY7LbRzc) |
| GASP 5.7 공식 업데이트 | [YouTube](https://www.youtube.com/watch?v=kmXaVIANa-c) |
| Unreal Fest 2024: Mover Plugin 소개 | [YouTube](https://www.youtube.com/watch?v=P4IKS5k47Wg) |
| Unreal Fest: 네트워크 물리 기반 캐릭터 이동 | [YouTube](https://www.youtube.com/watch?v=_jRLlTDqoGI) |
| GASP 5.7 Breakdown | [YouTube](https://www.youtube.com/watch?v=2yIrG6Ex9vc) |
| How to Import Mover and GASP | [YouTube](https://www.youtube.com/watch?v=yAJh7NBSsxc) |

### 커뮤니티 튜토리얼

| 자료 | URL |
|------|-----|
| Mover 2.0 + UAF + Motion Matching 셋업 | [David Martinez 가이드](https://farravid.github.io/posts/How-to-setup-Mover-2.0-+-Unreal-Animation-Framework-(UAF)-+-Motion-Matching-in-5.7/) |
| ACharacter에서 벗어나기 (커스텀 Pawn) | [Zenn 기사](https://zenn.dev/munimaru62o/articles/d1aeecc164aff4) |
| Crouch 구현 딥 다이브 | [Zenn 기사](https://zenn.dev/munimaru62o/articles/aa57704c553945) |
| DaftMover (팩토리 게임 이동) | [GitHub](https://github.com/daftsoftware/DaftMover) |
| 새로운 이동 모델 (Spring 기반) | [TheOrangeDuck](https://theorangeduck.com/page/new-movement-model) |

### 학습 권장 순서

```
1단계: 개념 이해
  └─ 이 문서 통독
  └─ Mover 공식 문서 읽기
  └─ Inside Unreal Q&A 영상 시청 (3시간, 핵심 파트 스킵 가능)

2단계: 실습 시작
  └─ GASP 5.7 Fab에서 다운로드
  └─ Mover Examples 플러그인 활성화 후 예제 맵 탐색
  └─ David Martinez 튜토리얼 따라하기

3단계: 자체 프로젝트 적용
  └─ 빈 프로젝트에 Mover + Mover Examples 활성화
  └─ BaseAnimatedMannyPawn 기반 BP 생성
  └─ Network Prediction 프로젝트 설정
  └─ 입력 시스템 연결 + AnimBP 구성

4단계: 심화
  └─ 커스텀 Movement Mode 제작 (슬라이딩, 벽타기 등)
  └─ APawn 기반 클린 아키텍처 구축
  └─ Motion Matching + Chooser 커스텀 셋업
  └─ 멀티플레이어 테스트
```

---

## UAF 미리보기 (UE 5.8 예정)

**Unreal Animation Framework (UAF)**는 AnimBP를 대체할 차세대 애니메이션 시스템:

| 에셋 | 역할 |
|------|------|
| **Animation Graph** | 데이터 컴포지션 기반 애니메이션 로직 |
| **System** | 이벤트 로직 (EventGraph 대체) |
| **Workspace** | 여러 에셋을 편집하는 에디터 샌드박스 |
| **AnimNext Component** | UAF 활성화 Actor 컴포넌트 |

- 5.7에서 **기반 작업** 시작 (Control Rig 통합)
- **5.8에서 GASP에 UAF 전용 캐릭터 포함 예정**
- UAF는 5.8에서도 Experimental이지만 커뮤니티가 직접 사용해볼 수 있을 정도의 완성도 목표

---

> **마지막 참고**: Mover Plugin은 Experimental이지만, Epic Games가 CMC의 공식 후계자로 지정한 시스템입니다. CMC는 Mover가 Production-Ready가 된 이후에도 **당분간 계속 지원**될 예정이므로, 당장 마이그레이션을 서두를 필요는 없습니다. 하지만 새 프로젝트라면 **Mover로 시작하는 것이 장기적으로 유리**합니다.
