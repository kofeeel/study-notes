---
title: "언리얼에서 UDP를 TCP처럼 쓰는 법 (NetDriver / Reliable RPC)"
tags: [unreal, network, udp, replication, rpc, netdriver]
created: 2026-06-16
status: 검증완료(2026-06-16)
---

# 언리얼에서 UDP를 TCP처럼 쓰는 법

## 1. 한 줄 정의

언리얼은 **UDP**로 패킷을 보내면서, 엔진이 직접 **순서 번호 + ACK(받았다는 답) + 재전송**을 붙여서 "꼭 도착해야 하는 데이터"만 TCP처럼 보장한다. 이걸 **Reliable RPC / Reliable Bunch**라고 부른다.

---

## 2. 왜 필요한가 (구체 문제)

게임은 TCP를 잘 안 쓴다. 이유는 **head-of-line blocking(앞막힘)**이다.

TCP는 "보낸 순서대로, 하나도 안 빠지고" 도착하는 걸 보장한다. 좋아 보이지만 게임엔 독이다.

- 캐릭터 위치를 1초에 30번 보낸다고 하자.
- 3번째 패킷이 도중에 사라졌다.
- TCP는 **3번이 다시 올 때까지 4, 5, 6번을 앱에 안 넘긴다.** 다 받아놓고도 막아둔다.
- 재전송에 100ms가 걸리면, 그동안 캐릭터가 화면에서 **멈췄다가 순간이동**한다.

근데 위치 데이터는 사실 3번이 사라져도 상관없다. 어차피 4번에 더 최신 위치가 들어있으니까. **오래된 정보를 기다리느라 최신 정보가 막히는 것**, 이게 앞막힘이다.

> TCP는 "정확한 전달"에 최적화돼 있고 "제때 전달"엔 최적화돼 있지 않다. 재전송·순서 정렬 때문에 몇 초씩 멈추는 일이 흔하다. 실시간 게임엔 안 맞는다. (출처: BeyondUnreal Wiki)

그래서 언리얼은 UDP를 쓴다. UDP는 그냥 패킷을 던진다. 도착 보장 없음, 순서 보장 없음. 대신 **막히지 않는다.** 위치 같은 데이터는 잃어버려도 다음 패킷이 덮어쓰니까 괜찮다.

**문제:** 그럼 "꼭 도착해야 하는 것"(예: "플레이어 사망", "문 열림 RPC")은 어쩌나? UDP는 보장을 안 해주는데.

**답:** 그 부분만 엔진이 직접 신뢰성을 입힌다. UDP의 빠름은 유지하고, 필요한 데이터에만 TCP스러운 보장을 씌운다.

---

## 3. 어떻게 동작하나 (구조 + 순서)

### 3-1. 3층 구조

언리얼 네트워킹은 3개 클래스로 나뉜다.

| 클래스 | 역할 | 개수 |
|---|---|---|
| `UNetDriver` | 전체 송수신 관리, replication tick 구동 | 월드당 1개 |
| `UNetConnection` | 한 상대(머신)와의 논리적 연결, 패킷 처리 | 서버=클라 수만큼, 클라=1개(서버) |
| `UChannel` | ChannelID로 데이터를 올바른 대상에 라우팅 | 연결당 여러 개 |

- 서버는 클라이언트가 4명이면 `UNetConnection` 4개를 들고, 각각을 tick 돈다.
- 채널은 종류가 있다: `UControlChannel`(핸드셰이크/접속), `UVoiceChannel`(음성), `UActorChannel`(복제되는 액터당 1개). 채널은 `ChannelID`로 구분된다.

### 3-2. Packet vs Bunch (이게 핵심)

이게 헷갈리는데, 둘은 다른 층이다.

- **Packet(패킷)** = UDP로 실제 와이어에 나가는 한 덩어리. 헤더(순서 번호, ACK 정보) + 여러 bunch.
- **Bunch(번치)** = "특정 액터 하나의 프로퍼티 변경 + RPC 묶음". 패킷 안에 들어가는 논리 단위.

> 공식 문서: "a bunch is a collection of property changes and RPCs for a particular replicated object, such as an actor." 네트워크 패킷은 여러 bunch로 구성된다. (출처: Epic 공식 RPC 문서)

작은 bunch 여러 개를 패킷 하나에 담을 수 있고, bunch가 너무 크면 쪼개서(partial) 여러 패킷에 나눠 담는다.

**왜 두 층으로 나누나?** 신뢰성을 두 단계로 처리하려고:

1. **Packet 층** = 순서 번호(sequence number)와 ACK/NAK를 담당. "이 패킷 받았다/못 받았다".
2. **Bunch 층** = packet 층의 ACK 결과를 보고 "내 reliable bunch가 도착했나?" 판단. 안 도착했으면 재전송.

### 3-3. Reliable bunch가 도착을 보장하는 순서 (예시)

서버가 클라에 reliable RPC("문 열림")를 보낸다고 하자. bunch에 `bReliable = true`가 박힌다.

1. 서버: bunch에 **bunch sequence 번호**를 붙여 packet에 담아 UDP로 던진다. 동시에 **"아직 ACK 못 받은 reliable bunch" 목록에 보관**해 둔다.
2. 패킷이 네트워크에서 사라진다.
3. 클라는 그 다음 패킷의 순서 번호를 보고 "어? 번호가 건너뛰었네 → 빠진 게 있다"를 안다. 빠진 packet ID에 대해 **NAK(못 받았다)**를 서버에 보낸다.
4. 서버: NAK를 받으면, 보관해 둔 reliable bunch를 **새 packet ID로 다시 보낸다.** 단 **bunch sequence 번호는 그대로** 유지 → 클라가 "이게 그때 그 데이터구나" 안다.
5. 클라가 받으면 ACK를 보낸다. 서버는 보관 목록에서 그 bunch를 지운다. 끝.

**순서 보장:** reliable bunch는 sequence 번호 순서대로만 처리된다. 4번이 먼저 도착해도 3번이 올 때까지 큐에 넣고 기다린다. (이건 reliable에만 적용 — unreliable은 순서 안 따짐.)

> 공식 문서 Reliable 설명: "This RPC is re-sent until it is acknowledged by the receiver. All subsequent RPC executions are suspended until this RPC is acknowledged." (출처: Epic 공식 RPC 문서)

즉 reliable RPC는 **앞 RPC가 ACK될 때까지 뒤 RPC 실행을 멈춘다.** 이게 reliable 채널 안에서의 TCP스러운 순서 보장이다. (단, 이 보장은 reliable 묶음 안에서만이고, unreliable과 전체 패킷에는 적용 안 됨.)

---

## 4. UE 실제 API / 코드

### 4-1. RPC 선언 (가장 많이 쓰는 진입점)

```cpp
// 서버에서 실행, 소유 클라가 호출. Reliable = 도착 보장
UFUNCTION(Server, Reliable, WithValidation)
void Server_OpenDoor(AActor* Door);

// 모든 클라에서 실행. Unreliable = 드롭되면 그냥 버림 (위치/이펙트용)
UFUNCTION(NetMulticast, Unreliable)
void Multicast_PlayHitEffect(FVector_NetQuantize Loc);

// 소유 클라에서만 실행
UFUNCTION(Client, Reliable)
void Client_ShowDeathScreen();
```

- `Reliable` / `Unreliable` 중 하나는 **반드시** 명시. 안 쓰면 컴파일 에러.
- 공식 문서: "RPCs are unreliable by default. Reliable RPCs require additional bandwidth, as such, use them sparingly." → **reliable은 비싸다. 꼭 필요한 데만.**
- `WithValidation`: 서버 RPC는 클라가 보낸 값을 검증. 실패하면 그 클라는 **접속이 끊긴다**(trust and verify 정책).

RPC 종류 (공식 문서):
- **Server**: 서버에서 실행, 소유 클라가 호출
- **Client**: 그 액터의 소유 클라 연결에서만 실행
- **NetMulticast**: 서버 + 그 액터가 relevant한 모든 접속 클라에서 실행

### 4-2. 내부 저수준 함수 (엔진 소스, 직접 부를 일은 거의 없음)

| 함수 | 역할 |
|---|---|
| `UNetDriver::TickDispatch()` | 들어오는 패킷 처리 진입점 |
| `UNetDriver::TickFlush()` | 나가는 replication 처리 |
| `UNetConnection::ReceivedPacket()` | 패킷 처리의 일꾼 — ACK/NAK, 채널 sequence 번호 읽음 |
| `UChannel::SendBunch()` | bunch를 보낼 준비를 하는 일꾼 |
| `UChannel::ReceivedBunch()` | bunch 수신 처리 (채널 종류별 override) |
| `FObjectReplicator::ReceivedRPC()` | RPC를 받아 `Object->ProcessEvent()` 호출 |

---

## 5. 흔한 함정

1. **모든 걸 Reliable로 깔기.** reliable은 ACK 받을 때까지 메모리에 들고 있고 대역폭을 더 쓴다. 위치·이펙트·사운드는 **Unreliable**이 맞다. 잃어버려도 다음 갱신이 덮으니까.

2. **`RELIABLE_BUFFER` 초과 → 연결 끊김.** 아직 ACK 안 된 reliable bunch + 현재 나가는 bunch 합이 **256(기본값)**을 넘으면 엔진이 그 연결을 **drop**한다. reliable RPC를 한 프레임에 폭탄처럼 쏘면 클라가 튕긴다. (출처: Devtricks)

3. **bunch가 너무 큼.** `NetMaxConstructedPartialBunchSizeBytes`(기본 64KB)를 넘으면 `UChannel::SendBunch`가 막는다(`IsBunchTooLarge`). 큰 데이터를 RPC 한 방에 밀어넣지 말 것.

4. **"reliable = TCP"라고 착각.** reliable RPC의 순서 보장은 **reliable 묶음 안에서만**이다. reliable RPC와 unreliable 프로퍼티 사이엔 순서 보장이 없다. "사망 처리"와 "위치 갱신"의 선후를 reliable로 보장받을 거라 믿으면 버그 난다.

5. **클라에서 Server RPC 검증 무시.** `WithValidation` 검증 실패는 경고가 아니라 **강제 접속 종료**다. 검증 함수에서 가볍게 true만 반환하면 치트 방어가 0이 된다.

---

## 6. 면접 Q&A

**Q1. 게임은 왜 TCP 대신 UDP를 쓰나요?**
A. head-of-line blocking(앞막힘) 때문입니다. TCP는 순서·완전 전달을 보장하느라, 패킷 하나가 빠지면 그게 재전송될 때까지 뒤에 도착한 최신 패킷들을 앱에 안 넘기고 막습니다. 게임에서 위치 같은 데이터는 오래된 걸 기다리느니 최신으로 덮는 게 맞는데, TCP는 그걸 못 합니다. UDP는 그냥 던지므로 막히지 않고, 꼭 필요한 데이터만 엔진이 따로 신뢰성을 붙입니다.

**Q2. 그럼 "꼭 도착해야 하는" RPC는 UDP에서 어떻게 보장하나요?**
A. 언리얼이 직접 sequence 번호 + ACK/NAK + 재전송을 구현합니다. reliable bunch는 `bReliable` 플래그가 붙고, ACK 받기 전까지 송신 측이 보관합니다. 수신 측이 sequence 번호 구멍으로 손실을 감지해 NAK를 보내면, 송신 측이 같은 bunch sequence로 새 패킷에 다시 실어 보냅니다. ACK가 올 때까지 그 다음 reliable RPC 실행은 중단됩니다.

**Q3. Packet과 Bunch의 차이는?**
A. Packet은 UDP로 실제 나가는 와이어 단위로, 헤더(sequence/ACK) + 여러 bunch로 구성됩니다. Bunch는 특정 액터 하나의 프로퍼티 변경과 RPC를 묶은 논리 단위입니다. 신뢰성을 두 층으로 나누려는 설계로, packet 층이 ACK/NAK를 담당하고 bunch 층이 그 결과를 보고 재전송 여부를 정합니다. 작은 bunch는 한 패킷에 여럿 담기고, 큰 bunch는 쪼개져 여러 패킷에 나뉩니다.

---

## 출처

- [Remote Procedure Calls in Unreal Engine — Epic 공식 문서](https://dev.epicgames.com/documentation/en-us/unreal-engine/remote-procedure-calls-in-unreal-engine) — RPC 기본 unreliable, Reliable 키워드 정의("re-sent until acknowledged, subsequent executions suspended"), bunch 정의, RPC 종류(Server/Client/NetMulticast), WithValidation 정책
- [Networking Overview for Unreal Engine — Epic 공식 문서](https://dev.epicgames.com/documentation/en-us/unreal-engine/networking-overview-for-unreal-engine) — 클라-서버 구조, network mode, replication 개념
- [Everything you ever wanted to know about replication — BeyondUnreal Wiki](https://wiki.beyondunreal.com/Everything_you_ever_wanted_to_know_about_replication_(but_were_afraid_to_ask)) — UDP 사용 이유, TCP가 실시간에 부적합한 이유, 엔진이 직접 손실/중복/순서 검사
- [Low-Level Networking Overview — Gamedev Guide (ikrima.dev)](https://ikrima.dev/ue4guide/networking/low-level-networking/low-level-networking-overview/) — UNetDriver/UNetConnection/UChannel 역할, packet/bunch 구조, reliable vs unreliable bunch, ReceivedPacket/SendBunch/TickDispatch/TickFlush 함수
- [Multiplayer data streaming in Unreal Engine — Devtricks (vorixo)](https://vorixo.github.io/devtricks/data-stream/) — `RELIABLE_BUFFER` 256 기본값 초과 시 연결 drop, `NetMaxConstructedPartialBunchSizeBytes` 64KB, `UChannel::SendBunch`/`IsBunchTooLarge`

### ⚠️ 미검증 항목
- NAK 시 "같은 bunch sequence, 새 packet id로 재전송" 메커니즘의 세부 동작은 2차 출처(웹 검색 요약)에서만 확인되었고 공식 문서 원문/엔진 소스로 직접 대조하지 못함. 큰 그림(packet 층 ACK/NAK → bunch 층 재전송)은 여러 출처가 일치하나, packet id 재할당 디테일은 **미검증**.
- 본문 §3-3은 손실 감지를 "수신 측 sequence gap → NAK" 경로로만 서술함. UE 재전송이 NAK 외에 타임아웃 기반 경로도 갖는지는 출처에서 명시 확인하지 못함 — **미검증**(엔진 일반론상 timeout 재전송이 통상이나 UE 구현 디테일 미대조).
- 보조 상수: `RELIABLE_BUFFER = 256`, `MAX_CHSEQUENCE = 1024`(RELIABLE_BUFFER보다 큰 2의 거듭제곱, 손실/순서뒤바뀜 구간 커버)는 2차 출처(Devtricks/Gamedev Guide) 일치 확인. 엔진 소스 직접 대조는 안 함.
