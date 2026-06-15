---
title: UE 리플리케이션 & RPC
tags: [unreal, network, replication, rpc, gas, multiplayer]
created: 2026-06-16
status: 검증완료(2026-06-16)
---

# UE 리플리케이션 & RPC

## ① 한 줄 정의

**리플리케이션 = 서버의 게임 상태를 클라들에게 자동으로 복사해 주는 것. RPC = 한쪽 기계에서 함수를 호출하면 다른 쪽 기계에서 실행되게 하는 것.**

멀티플레이는 "서버 1대 + 클라 여러 대" 구조다. 서버가 진짜 게임 상태(authoritative)를 들고 있고, 클라는 그 복사본을 본다. 그 복사를 자동으로 해 주는 게 리플리케이션이다.

---

## ② 왜 필요한가 (구체 문제)

클라 두 명이 같은 방에서 싸운다고 하자. A의 화면에서 적 HP가 100인데, B의 화면에서는 60이면 게임이 깨진다. 누구 말이 맞나?

그래서 UE는 규칙을 정한다: **서버 1대만 진짜 값을 갖는다(권위, authority).** 클라는 서버가 보내준 값을 그대로 받아 표시만 한다.

문제는 "서버가 바뀐 값을 어떻게 클라에 전달하느냐"다. 매 프레임 모든 변수를 다 보내면 네트워크가 터진다. 그래서 두 가지 도구가 있다:

- **Property 리플리케이션** — "이 변수는 동기화 대상"이라고 표시해 두면, 값이 바뀔 때만 서버가 알아서 클라에 보낸다. (상태 동기화에 적합)
- **RPC** — "이 함수를 다른 기계에서 실행해라"라고 한 번 쏘는 것. (일회성 이벤트에 적합)

---

## ③ 어떻게 동작하나 (구체 예/순서)

### 권위(Authority) 모델

> 서버는 게임의 호스트로서, 단 하나의 진짜 권위 있는(authoritative) 게임 상태를 갖는다. (공식 문서)

- 서버에서 `HasAuthority()` == `true`
- 클라에서 `HasAuthority()` == `false` (그 액터는 서버가 만든 것의 **복제본/프록시**)
- **규칙: 게임플레이 값 변경은 서버에서만 한다.** 클라가 자기 멋대로 HP를 깎으면, 다음 동기화 때 서버 값으로 덮어써져서 도로 돌아온다.

### Property 리플리케이션 흐름 (예: HP 100 → 70)

1. 서버에서 `Health = 70` 으로 변경 (서버만 변경 가능)
2. 엔진이 "Health는 리플리케이트 대상"임을 알고, 다음 네트워크 업데이트 때 클라에 70을 보냄
3. 클라에서 Health가 70으로 갱신됨
4. (RepNotify를 걸어뒀다면) 클라에서 `OnRep_Health()` 함수가 자동 호출됨 → 여기서 HP바 UI 갱신 등

핵심: **값이 안 바뀌면 안 보낸다.** 100 그대로면 트래픽 0.

### RPC 3종 — 누가 부르고 어디서 실행되나

| 종류 | 부르는 쪽 | 실행되는 쪽 | 조건 |
|---|---|---|---|
| **Server** | 그 액터를 **소유한 클라** | 서버 | 액터를 클라가 owning |
| **Client** | 서버 | 그 액터의 **소유 클라 1명** | 유효한 owning connection |
| **NetMulticast** | 서버 | 서버 + **관련 있는 모든 클라** | 액터가 리플리케이트 |

흐름 예 (클라가 발사 이펙트를 모두에게 보이고 싶을 때):

1. 클라가 `ServerFire()` (Server RPC) 호출 → 서버에서 실행
2. 서버가 데미지 처리(권위) 후 `MulticastSpawnMuzzleFlash()` (NetMulticast RPC) 호출
3. 서버 + 모든 관련 클라에서 머즐 플래시 이펙트 재생

### Reliable vs Unreliable

- **RPC는 기본 Unreliable.** 패킷 손실 시 도착 안 할 수 있음. 대역폭 적고, 자주 쏴도 안전.
- `Reliable` 지정 시 도착 보장(소유 규칙을 지킨다는 전제). 대신 대역폭/지연 증가.
- 기준: **잦고 매 프레임 갱신되는 것(이동, 이펙트)** → Unreliable. **놓치면 안 되는 것(사망, 점수 획득)** → Reliable.

---

## ④ UE 실제 API · 코드

### 리플리케이션 켜기 + Property 선언

```cpp
// 생성자: 액터 자체를 리플리케이트 대상으로
AMyActor::AMyActor()
{
    bReplicates = true;
}

// 헤더: 동기화할 변수
UPROPERTY(Replicated)
int32 Health;

// RepNotify가 필요하면 ReplicatedUsing 사용
UPROPERTY(ReplicatedUsing = OnRep_Health)
int32 Health;
```

### GetLifetimeReplicatedProps + DOREPLIFETIME

```cpp
void AMyActor::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
    Super::GetLifetimeReplicatedProps(OutLifetimeProps);  // ★ Super 호출 필수
    DOREPLIFETIME(AMyActor, Health);
}
```

> `Super::`를 빼먹으면 부모 클래스에서 상속된 리플리케이트 프로퍼티가 동기화되지 않는다. (공식 문서)

### OnRep_ (RepNotify) 함수

```cpp
// 헤더
UFUNCTION()
void OnRep_Health();          // 기본형

UFUNCTION()
void OnRep_Health(int32 OldHealth);  // 이전 값을 받는 형도 가능
```

`OnRep_Health()`는 **클라에서** Health가 동기화될 때 자동 호출된다. (서버 자신은 기본적으로 안 불림 — 서버에서도 같은 처리가 필요하면 값 변경 후 직접 호출해야 한다.)

### RPC 선언 (시그니처)

```cpp
// Server RPC — 클라가 호출, 서버에서 실행. 입력 검증 권장
UFUNCTION(Server, Reliable, WithValidation)
void ServerFire();
void ServerFire_Implementation();   // 실제 구현
bool ServerFire_Validate();         // false 반환 시 연결 끊김(치트 방어)

// Client RPC — 서버가 호출, 소유 클라에서 실행
UFUNCTION(Client, Reliable)
void ClientNotifyResult();
void ClientNotifyResult_Implementation();

// NetMulticast RPC — 서버가 호출, 모든 관련 클라 + 서버에서 실행
UFUNCTION(NetMulticast, Unreliable)
void MulticastSpawnEffect();
void MulticastSpawnEffect_Implementation();
```

**중요: `_Implementation` 접미사에 실제 코드를 쓴다.** 호출은 접미사 없는 이름(`ServerFire()`)으로 한다. 엔진이 자동 생성한 글루 코드가 네트워크로 보낸 뒤 반대편에서 `_Implementation`을 실행한다.

### Ownership / Relevancy / Authority API

```cpp
HasAuthority();                 // 서버(권위)에서 true
GetLocalRole() == ROLE_Authority;

SetOwner(PlayerController);      // Server RPC를 보낼 권한의 근거 = Owner의 NetConnection

// 관련성(Relevancy) 제어
bAlwaysRelevant = true;          // 항상 모든 클라에 동기화
bOnlyRelevantToOwner = true;     // 소유 클라에게만
NetCullDistanceSquared;          // 이 거리(제곱)보다 멀면 동기화 끊김
```

**Relevancy(관련성):** 서버는 모든 액터를 모든 클라에 보내지 않는다. `AActor::IsNetRelevantFor()`가 클라마다 "이 액터가 이 클라에 필요한가"를 판정한다. 판정 순서(요약):

1. `bAlwaysRelevant`이거나, 그 클라의 Pawn/PlayerController가 소유/본인/Instigator면 → 관련
2. `bOnlyRelevantToOwner`면 소유자 외에는 비관련
3. 거리 기반 컬링: `NetCullDistanceSquared`보다 멀면 비관련

**Ownership과 NetConnection:** Server RPC를 보내려면 그 액터를 호출하는 클라가 **소유(Owner)**하고 있어야 한다. Owner 체인을 따라가 PlayerController의 NetConnection이 있어야 RPC가 그 클라↔서버로 라우팅된다. 소유 안 된 액터에서 Server RPC를 부르면 무시된다.

### "Property vs RPC" 언제 뭘 쓰나

| 상황 | 선택 | 이유 |
|---|---|---|
| 지속 **상태** (HP, 위치, 진영, 플래그) | **Replicated Property** | 늦게 접속한 클라도 현재 값을 받음. 값 안 바뀌면 트래픽 0 |
| 일회성 **이벤트** (발사, 폭발 사운드, 점프) | **RPC** | "그 순간"만 의미 있음. 상태로 둘 필요 없음 |
| 상태 + 그 변화 순간의 연출 | **Property + OnRep_** | 값은 동기화로, 연출은 RepNotify에서 |

핵심 직관: **나중에 접속한 클라가 알아야 하면 Property, 그 순간에만 의미 있으면 RPC.** RPC는 발사 시점에 관련 없던 클라에는 안 간다(놓치면 끝).

---

## ⑤ 흔한 함정

1. **클라에서 게임플레이 값 변경** — 클라가 Health를 바꿔도 다음 동기화 때 서버 값으로 덮어써짐. **항상 `HasAuthority()` 게이트** 후 서버에서만 변경.
2. **`Super::GetLifetimeReplicatedProps` 누락** — 부모의 리플리케이트 프로퍼티가 안 보내짐.
3. **`bReplicates = true` 안 켬** — 프로퍼티에 `Replicated` 붙여도 액터 자체가 리플리케이트 안 하면 무의미. RPC도 안 감.
4. **소유 안 된 액터에서 Server RPC** — Owner 없으면 RPC 무시됨. Pawn/PlayerController 체인 확인.
5. **Multicast를 매 프레임 Reliable로 도배** — Reliable 큐가 차면 연결 끊김. 잦은 연출은 Unreliable.
6. **RepNotify가 서버에서 안 불린다고 착각** — `OnRep_`은 기본적으로 클라 전용. 서버에도 같은 로직 필요하면 값 변경 직후 직접 호출.
7. **NetMulticast인데 클라에서 호출** — 클라가 Multicast를 부르면 로컬에서만 실행됨(전파 안 됨). Multicast는 서버에서 불러야 모두에게 간다.

---

## ⑥ GAS / 우리 프로젝트 규칙과 연결

우리 `CLAUDE.md §2`의 Replication 규칙은 위 원리의 GAS 적용판이다:

- **`HasAuthority()` 게이트** — GAS 효과(GE 적용, 어트리뷰트 변경)는 서버에서만. ③의 권위 모델 그대로.
- **`UPROPERTY(Replicated)` / `OnRep_*` / `GetLifetimeReplicatedProps`** — 일반 액터 프로퍼티 동기화에 위 ④ 코드 그대로 적용.
- **`GAMEPLAYATTRIBUTE_REPNOTIFY`** — AttributeSet 어트리뷰트의 OnRep 안에서 호출하는 매크로. 서버가 보낸 새 base value를 받아, ASC의 ActiveGameplayEffects 컨테이너를 거쳐 current(final) value를 다시 계산하고 변경 델타를 ASC에 통지한다. 예측(prediction)으로 클라가 값을 미리 바꿔둔 경우 로컬 값이 서버 값과 같아도 OnRep이 호출돼야 하므로, 어트리뷰트는 `DOREPLIFETIME_CONDITION_NOTIFY(..., REPNOTIFY_Always)`로 등록한다. (일반 `OnRep_`의 GAS 버전)

> ⚠️ 우리 프로젝트는 현재 **싱글 프로토타입**이라 `CLAUDE.md §2` 단서대로 Replication은 생략 가능 상태. 멀티 확장이 결정되는 시점부터 위 4종(Replicated / OnRep_ / GetLifetimeReplicatedProps / GAMEPLAYATTRIBUTE_REPNOTIFY)을 적용한다.

GAS의 멀티 동기화는 일반 액터보다 복잡하다(prediction, ASC replication mode 등). 이 문서는 그 **밑바탕인 일반 리플리케이션/RPC**를 다룬다.

---

## ⑦ 면접 Q&A

**Q1. Replicated Property와 RPC는 언제 각각 쓰나?**
A. 지속되는 **상태**(HP, 위치, 진영)는 Replicated Property — 나중에 접속한 클라도 현재 값을 받고, 값이 안 바뀌면 트래픽이 안 든다. 그 순간에만 의미 있는 **일회성 이벤트**(발사, 폭발음)는 RPC — 발사 시점에 관련 없던 클라에는 안 가도 되고, 상태로 들고 있을 필요가 없다. 직관은 "나중 접속자가 알아야 하면 Property, 순간만이면 RPC".

**Q2. Server / Client / NetMulticast RPC 차이와 호출/실행 위치는?**
A. Server는 소유 클라가 호출해서 서버에서 실행(클라→서버, 권위 행동 요청 + WithValidation으로 치트 방어). Client는 서버가 호출해서 그 액터의 소유 클라 1명에서 실행. NetMulticast는 서버가 호출해서 서버 + 관련 있는 모든 클라에서 실행(연출 전파). 기본은 모두 Unreliable이고, 놓치면 안 되는 건 Reliable을 붙인다.

**Q3. 클라가 "내 HP를 줄여" 하면 왜 도로 원복되나? OnRep_은 언제 불리나?**
A. 권위는 서버에만 있다. 클라가 로컬에서 Health를 바꿔도 그 액터는 서버가 만든 복제본이라, 다음 리플리케이션 때 서버의 진짜 값으로 덮어써진다. 그래서 변경은 `HasAuthority()` 게이트 후 서버에서만 한다. `OnRep_`(RepNotify)는 서버가 보낸 값이 **클라에서** 갱신될 때 자동 호출되며 — UI 갱신 같은 연출을 여기서 한다. 서버 자신은 기본적으로 안 불리므로, 서버에도 같은 처리가 필요하면 값 변경 직후 직접 호출한다.

---

## 출처

- [Remote Procedure Calls in Unreal Engine — Epic 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/remote-procedure-calls-in-unreal-engine)
- [Replicate Actor Properties in Unreal Engine — Epic 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/replicate-actor-properties-in-unreal-engine)
- [Actor Replication (4.27) — Epic 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/actor-replication?application_version=4.27)
- [Networking Overview for Unreal Engine — Epic 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/networking-overview-for-unreal-engine)
- [Actor Relevancy and Priority in Unreal Engine — Epic 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/actor-relevancy-and-priority-in-unreal-engine?application_version=5.2)
- [Replicated Properties vs RPCs — Epic Developer Community KB](https://dev.epicgames.com/community/learning/knowledge-base/D7yR/unreal-engine-replicated-properties-vs-rpcs)
- [ACharacter::GetLifetimeReplicatedProps (5.6 API) — Epic 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/ACharacter/GetLifetimeReplicatedProps)
- 우리 프로젝트 `CLAUDE.md §2` (GAS Replication 규칙)
