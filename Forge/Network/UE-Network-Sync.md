---
title: UE 네트워크 동기화 기법
tags: [unreal, network, replication, prediction, gas, cmc]
created: 2026-06-16
status: 검증완료(2026-06-16)
---

# UE 네트워크 동기화 기법

## 1. 한 줄 정의

여러 PC가 같은 게임을 보게 만들되, **서버 1대만 진짜 정답을 갖고**(서버 권위), 클라이언트는 정답을 미리 흉내 내(예측) 끊김 없이 보여주는 기술 묶음.

---

## 2. 왜 필요한가 (구체 문제)

인터넷은 느리다. 내 PC에서 서버까지 신호가 갔다 오는 데 보통 30~150ms 걸린다(핑/RTT). 만약 클라이언트가 "버튼 누름 → 서버에 물어봄 → 답 올 때까지 캐릭터 멈춤"으로 처리하면, 누를 때마다 0.1초씩 캐릭터가 멈칫한다. 게임이 못 할 물건이 된다.

반대로 클라이언트가 마음대로 움직이게 두면 **치트**가 된다. "내 체력 9999, 나는 벽 통과" 같은 패킷을 서버가 그냥 믿어버리기 때문이다.

그래서 두 가지를 동시에 만족해야 한다:

1. **반응성** — 내 입력은 즉시 화면에 보여야 함 → 클라이언트 예측
2. **공정성/보안** — 진짜 결과는 서버가 결정 → 서버 권위

이 둘의 충돌을 푸는 게 이 문서 전체의 주제다.

---

## 3. 어떻게 동작하나 (구체 예/순서)

### 3-1. 서버 권위 (Server-Authoritative)

서버가 "단 하나의 진짜 게임 상태"를 들고 있다. 클라이언트는 서버에 있는 자기 Pawn을 RPC로 조종하고, 서버는 결과를 각 클라이언트에 **복제(Replication)** 해서 내려보낸다. 클라이언트는 받은 데이터로 서버 상태를 근사하게 재현한다.
(출처: Networking Overview)

UE에서 네트워크로 상태를 복제할 수 있는 진입점은 `AActor`다. 복제 데이터는 Actor 단위의 채널(ActorChannel)로 전송되므로, 일반 UObject는 단독으로 복제되지 않고 Actor의 **서브오브젝트**로 등록돼야 복제된다(`ReplicateSubobjects` / `bReplicateUsingRegisteredSubObjectList`). 즉 "복제의 1급 시민"은 Actor와 ActorComponent다.
(출처: Networking Overview, Replicating Object References)

### 3-2. Client-Side Prediction + 롤백 (때림 → 검증 → 보정)

핑 100ms 상황에서 내가 앞으로 걷는 장면을 순서대로 보면:

1. **0ms (클라):** W키 입력. 클라가 **기다리지 않고** 즉시 이동 물리(`PerformMovement`)를 돌려 화면에서 캐릭터를 앞으로 옮긴다. 이 이동 결과를 `FSavedMove_Character`에 기록하고 `SavedMoves` 큐에 쌓는다.
2. **0ms (클라→서버):** `ServerMove` RPC로 "이만큼 움직였다"를 서버에 보낸다. (이 RPC는 **unreliable** — 자주 보내서 reliable 버퍼가 넘치는 걸 막고, SavedMoves 큐가 알아서 재전송을 보장하기 때문)
3. **50ms (서버):** 서버가 받은 입력으로 똑같이 이동을 돌려 **진짜 위치**를 계산한다.
4. **서버 판정 — 두 갈래:**
   - 클라 예측과 서버 결과가 **같으면** → `ClientAckGoodMove` RPC로 "그 무브 OK". 클라는 해당 무브를 `SavedMoves`에서 지운다. (롤백 없음, 가장 흔한 경우)
   - **다르면**(벽에 막혔는데 클라가 통과했다든지) → `ClientAdjustPosition` 류 RPC로 "진짜 위치는 여기야"를 내려보낸다.
5. **100ms (클라, 보정/롤백):** 보정을 받으면 클라는 서버가 준 위치로 되돌리고(rewind), `SavedMoves`에 남아 있던 **그 이후 입력들을 다시 적용(replay)** 해서 현재까지 따라잡는다. 화면에서는 살짝 순간이동(rubber-banding)처럼 보일 수 있다.

핵심: **틀렸을 때만 보정**한다. 대부분의 무브는 일치해서 롤백이 안 일어나고, 그래서 평소엔 입력이 즉각 반응하는 것처럼 느껴진다.
(출처: Understanding Networked Movement)

### 3-3. Interpolation / Extrapolation (남의 캐릭터)

내 캐릭터는 예측으로 그리지만, **다른 사람 캐릭터**(Simulated Proxy)는 서버가 내려주는 위치 업데이트를 받아서 그린다. 그런데 서버 업데이트는 초당 30번쯤(30Hz) 오는데 모니터는 초당 60~240번 그린다. 업데이트 사이를 그냥 두면 뚝뚝 끊겨 보인다.

- **Interpolation(보간):** 받은 과거 위치 두 개 사이를 부드럽게 채운다. 약간 과거를 보여주는 대신 매끄럽다.
- **Extrapolation(외삽):** 마지막 속도로 다음 위치를 추정해 미리 그린다. 캐릭터가 멈출 때 등에서 쓰지만, 빗나가면 튄다.

UE에서는 `NetworkSmoothingMode`로 이 스무딩 방식을 고른다. 모드는 3가지: `Disabled`(스무딩 없음, 받은 위치로 즉시 스냅) / `Linear`(소스→타깃 선형 보간) / `Exponential`(타깃에서 멀수록 빠르게 따라붙음, 기본값). 시뮬 프록시는 `SmoothClientPosition`이 이 모드에 따라 목표 위치까지 부드럽게 따라간다. 주의: 이는 외삽(extrapolation)이 아니라 **이미 받은 위치로 수렴**시키는 보간이다.
(출처: Understanding Networked Movement, NetworkSmoothingMode API)

### 3-4. Lag Compensation (지연 보상)

FPS에서 "분명 맞췄는데 안 맞음" 문제. 내 화면의 적은 핑 때문에 **과거 위치**다. 내가 쏜 순간 서버에서 적은 이미 다른 곳에 있다.

해결: 서버가 각 캐릭터의 **과거 위치 히스토리(버퍼)** 를 들고 있다가, 클라가 "이 시각에 쐈다"고 보고하면 서버가 그 시각으로 **시간을 되감아(rewind)** 그때 적이 있던 자리에서 명중 판정을 한다. "쏜 사람 화면 기준"으로 맞춰주는 것.
(출처: SnapNet 블로그 — 커뮤니티)

> ✅ **검증됨(2026-06-16):** UE5는 캐릭터 히트용 lag compensation을 **엔진 표준으로 제공하지 않는다.** 그래서 Epic의 ShooterGame·Lyra 샘플조차 lag compensation 없이 client-authoritative hit detection을 쓴다. 서버 되감기(rewind) 방식이 필요하면 직접 구현하거나 서드파티(예: SnapNet, RB Lag Compensation, GMC 플러그인)를 쓴다. 위 rewind 개념 설명은 커뮤니티 출처(SnapNet) 기준이며 메커니즘 자체는 표준적이다.

---

## 4. UE 실제 API / 코드

### 4-1. 변수 복제 (Replication 기본)

```cpp
// 헤더: 복제할 변수 + RepNotify
UPROPERTY(ReplicatedUsing = OnRep_Health)
float Health;

UFUNCTION()
void OnRep_Health();   // 클라에서 Health가 갱신될 때 호출

// 모든 복제 변수는 여기 등록 필수 (안 하면 복제 안 됨)
virtual void GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const override;

// .cpp
void AMyActor::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
    Super::GetLifetimeReplicatedProps(OutLifetimeProps);
    DOREPLIFETIME(AMyActor, Health);
}
```

### 4-2. 권위 체크 / RPC

```cpp
// 서버에서만 실행되는지 게이트
if (HasAuthority())
{
    // 진짜 상태 변경은 여기서
}

// RPC 종류
UFUNCTION(Server, Reliable)    void ServerDoThing();   // 클라 → 서버
UFUNCTION(Client, Reliable)    void ClientNotify();    // 서버 → 소유 클라
UFUNCTION(NetMulticast, Unreliable) void MulticastFx(); // 서버 → 전원
```

(출처: Has Authority API, Networking Overview)

### 4-3. CharacterMovementComponent 내부 (예측의 핵심)

이건 직접 부르는 API라기보다 CMC가 내부에서 쓰는 구조/함수다. 커스텀 무브먼트 만들 때 이 이름들을 오버라이드한다.

```cpp
// 클라 예측 데이터 (저장된 무브 큐 보관)
class FNetworkPredictionData_Client_Character;
struct FSavedMove_Character;     // 한 프레임의 이동 결과 1개

// 클라: 이동을 돌리고 기록 → 서버로 전송
void UCharacterMovementComponent::ReplicateMoveToServer(float DeltaTime, const FVector& NewAcceleration);

// 클라 → 서버 (unreliable): 입력/이동 보고
// 실제 시그니처는 버전마다 다름. 보정 진입점:
void UCharacterMovementComponent::ServerMoveHandleClientError(...);

// 서버 → 클라: 위치 강제 보정 (롤백 트리거)
// ClientAdjustPosition / ClientAckGoodMove 계열 RPC
```

(출처: Understanding Networked Movement)

### 4-4. GAS Prediction Key

```cpp
// 예측 스코프 — 같은 논리 구간 동안 클라/서버가 같은 prediction key 공유
FScopedPredictionWindow ScopedPrediction(AbilitySystemComponent, /*bCanGenerateNewKey=*/true);

// 클라가 만든 키를 서버 RPC로 넘겨, 서버가 같은 키로 처리
ASC->ServerInputRelease(ScopedPrediction.ScopedPredictionKey);
```

GAS가 **예측하는 것:** Ability 활성화, 애님 몽타주 재생, Attribute의 CurrentValue 변경, GameplayTag 부여, GameplayCue 스폰, RootMotionSource 이동.

GAS가 **예측 못 하는 것:** GameplayEffect의 쿨다운(→ 핑 높으면 발사 속도 손해), GameplayEffect **제거**, 주기적(Periodic) 효과, 퍼센트 기반 변경.
(출처: FScopedPredictionWindow API, FPredictionKey API, tranek GASDocumentation)

---

## 5. 흔한 함정

1. **`GetLifetimeReplicatedProps`에 등록 안 함** — `UPROPERTY(Replicated)`만 붙이고 `DOREPLIFETIME`을 빼면 복제가 조용히 안 된다. 컴파일은 통과해서 더 헷갈린다.
2. **클라에서 상태 변경** — 게임플레이 결정(데미지, 체력, 아이템 소모)을 `HasAuthority()` 게이트 없이 클라에서 하면 화면만 바뀌고 서버는 모른다 → 동기화 깨짐/치트.
3. **시뮬 프록시 위치를 서버 기준으로 정확히 재현하려는 시도** — 시뮬 프록시는 보간/외삽/스무딩이 끼어 있어 서버가 본 포즈와 절대 정확히 일치하지 않는다. 정밀 판정은 서버에서 lag compensation으로 풀어야지, 클라 위치를 믿으면 안 된다. (출처: Understanding Networked Movement)
4. **GAS 쿨다운이 예측될 거라 기대** — 쿨다운은 예측 불가라 핑 높은 플레이어가 손해 본다. 발사 속도가 핑에 민감하면 이걸 의심.
5. **ServerMove를 reliable로 바꾸기** — 매 프레임 오는 무브를 reliable로 보내면 버퍼가 넘친다. unreliable이 정상이고, 재전송은 SavedMoves가 보장한다. (출처: Understanding Networked Movement)
6. **싱글 프로토타입에 멀티 코드 미리 깔기** — 우리 프로젝트(싱글) 기준 §CLAUDE.md: 멀티 확정 전엔 복제/예측 코드 생략 가능. YAGNI.

---

## 6. 면접 Q&A

**Q1. 서버 권위인데 어떻게 입력이 즉각 반응하나요?**
클라가 서버 응답을 기다리지 않고 먼저 이동 물리를 돌려 화면에 보여주고(client-side prediction), 그 무브를 SavedMoves에 저장해 둡니다. 서버가 "OK"면 그대로, "틀림"이면 서버 위치로 되돌린 뒤 저장된 이후 입력들을 다시 replay 해서 따라잡습니다. 보정이 큰 순간만 고무줄처럼 보이고, 대부분은 일치해서 끊김이 없습니다.

**Q2. 내 캐릭터와 남의 캐릭터를 다르게 그리는 이유는?**
내 캐릭터는 Autonomous Proxy라 예측으로 즉시 그립니다. 남의 캐릭터는 Simulated Proxy라 서버 업데이트(예: 30Hz)만 받으므로, 그 사이를 interpolation으로 부드럽게 채우거나 extrapolation으로 추정합니다. UE에서는 NetworkSmoothingMode/SmoothClientPosition가 담당합니다.

**Q3. "분명 맞췄는데 안 맞는" 문제는 왜 생기고 어떻게 푸나요?**
내 화면 속 적은 핑만큼 과거 위치라, 내가 쏜 순간 서버에선 적이 이미 이동했기 때문입니다. Lag compensation으로 풀며, 서버가 각 캐릭터의 과거 위치 히스토리를 들고 있다가 클라가 보고한 시각으로 되감아(rewind) 그때 자리에서 명중 판정을 합니다. 단, UE5는 이 기능을 엔진 표준으로 내장하지 않습니다(ShooterGame·Lyra도 미사용) — 직접 구현하거나 서드파티 플러그인을 씁니다.

---

## 출처

- [Networking Overview for Unreal Engine](https://dev.epicgames.com/documentation/unreal-engine/networking-overview-for-unreal-engine?lang=en-US) — 서버 권위, 복제, Actor 네트워킹
- [Understanding Networked Movement in the Character Movement Component](https://dev.epicgames.com/documentation/unreal-engine/understanding-networked-movement-in-the-character-movement-component-for-unreal-engine) — SavedMoves, ServerMove, 예측/보정, 시뮬 프록시 스무딩
- [Client-Server Model](https://dev.epicgames.com/documentation/en-us/unreal-engine/client-server-model?application_version=4.27) — 클라/서버 모델
- [Has Authority (Blueprint API)](https://dev.epicgames.com/documentation/unreal-engine/BlueprintAPI/Networking/HasAuthority?lang=en-US) — HasAuthority
- [NetworkSmoothingMode (API)](https://docs.unrealengine.com/5.1/en-US/API/Runtime/Engine/GameFramework/UCharacterMovementComponent/NetworkSmoothingMode/) — 시뮬 프록시 스무딩 모드
- [FScopedPredictionWindow (API)](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Plugins/GameplayAbilities/FScopedPredictionWindow) — GAS 예측 스코프
- [FPredictionKey (API)](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Plugins/GameplayAbilities/FPredictionKey) — GAS prediction key
- [tranek/GASDocumentation](https://github.com/tranek/GASDocumentation) — GAS가 예측하는 것/못 하는 것 (커뮤니티, 광범위 인용 표준)
- [Performing Lag Compensation in Unreal Engine 5 — SnapNet](https://snapnet.dev/blog/performing-lag-compensation-in-unreal-engine-5/) — lag compensation rewind (커뮤니티)
