---
title: 네트워크 기초 (게임 관점) — TCP vs UDP, 소켓, 패킷
tags: [네트워크, TCP, UDP, 소켓, 패킷, 지터, RTT, MTU, 언리얼, 멀티플레이, 면접준비]
created: 2026-06-16
status: 검증완료(2026-06-16)
---

# 네트워크 기초 (게임 관점)

> 참고: 이 문서는 [[Reliable-UDP]]의 토대다. 여기서 "왜 게임은 UDP를 쓰나"까지 깔고,
> 그 위에 "UDP인데 어떻게 신뢰성을 다시 만드나"를 reliable-udp 문서에서 다룬다.
> 연관: OS [[03-동기화-및-데드락]] (지터·버퍼 큐 개념이 닮았다)

---

## 목차

1. [한 줄 정의](#1-한-줄-정의)
2. [왜 필요한가 — 게임이 마주하는 구체 문제](#2-왜-필요한가--게임이-마주하는-구체-문제)
3. [소켓 — 통신의 출입구](#3-소켓--통신의-출입구)
4. [TCP vs UDP — 어떻게 다른가](#4-tcp-vs-udp--어떻게-다른가)
5. [패킷 손실 · 재정렬 · 지터](#5-패킷-손실--재정렬--지터)
6. [HOL Blocking — TCP의 결정적 약점](#6-hol-blocking--tcp의-결정적-약점)
7. [왜 실시간 게임은 UDP를 고르나](#7-왜-실시간-게임은-udp를-고르나)
8. [RTT · 대역폭 · MTU — 숫자 감각](#8-rtt--대역폭--mtu--숫자-감각)
9. [UE 실제 API — UDP 위에서 RPC](#9-ue-실제-api--udp-위에서-rpc)
10. [흔한 함정](#10-흔한-함정)
11. [면접 Q&A](#11-면접-qa)
12. [출처](#출처)

---

## 1. 한 줄 정의

**네트워크 통신**은 한 컴퓨터의 데이터를 작은 조각(**패킷**)으로 잘라 인터넷을 통해 다른 컴퓨터로 보내는 일이다.
이때 두 가지 배달 방식이 있다 — **TCP**(느려도 정확한 등기우편)와 **UDP**(빠르지만 분실 책임 안 지는 일반우편).

---

## 2. 왜 필요한가 — 게임이 마주하는 구체 문제

대전 게임에서 내 캐릭터가 움직이면, 그 위치를 상대 컴퓨터에도 알려줘야 한다.
문제는 인터넷이 **완벽하지 않다**는 것:

- 보낸 패킷이 **사라질 수 있다** (패킷 손실)
- 늦게 보낸 패킷이 **먼저 도착할 수 있다** (재정렬)
- 도착 간격이 **들쭉날쭉할 수 있다** (지터)

게임은 보통 1초에 **30~128번** 위치·입력을 보낸다. 33ms 전 패킷 하나가 사라졌다고
"그거 다시 보내줄 때까지 화면 멈춤"이면 게임이 안 된다. 그래서 게임은 배달 방식을
**직접 고르고**, 손실에 대응하는 자기만의 기술(보간, 최신 상태 통째 전송, 클라이언트 예측)을 쓴다.
이걸 이해하려면 먼저 TCP/UDP 차이를 알아야 한다.

---

## 3. 소켓 — 통신의 출입구

**소켓(socket)**은 프로그램이 네트워크로 데이터를 주고받기 위해 여는 "출입구"다.
운영체제가 제공하는 통신 끝점이고, **IP 주소 + 포트 번호**의 조합으로 식별된다.

```
192.168.0.10 : 7777
└─ IP 주소 ─┘  └ 포트 ┘
   (어느 컴퓨터)  (그 컴퓨터의 어느 프로그램)
```

- **IP 주소**: 인터넷에서 컴퓨터를 찾는 주소 (집 주소)
- **포트**: 한 컴퓨터 안에서 어느 프로그램인지 (몇 호실). 게임 서버는 흔히 7777 사용
- 한 PC에서 게임·브라우저·디스코드가 동시에 통신해도, 포트가 다르므로 안 섞인다

소켓에는 두 종류가 있다:
- **스트림 소켓** (`SOCK_STREAM`) → TCP. 끊김 없는 바이트 흐름
- **데이터그램 소켓** (`SOCK_DGRAM`) → UDP. 조각(데이터그램) 단위로 던짐

> 핵심: TCP냐 UDP냐는 **소켓을 만들 때** 정한다. 게임은 보통 UDP 소켓을 연다.

---

## 4. TCP vs UDP — 어떻게 다른가

| 항목 | TCP | UDP |
|------|-----|-----|
| **연결** | 연결 수립 후 통신 (3-way handshake) | 연결 없음, 바로 던짐 |
| **신뢰성** | 도착 보장 (안 오면 재전송) | 보장 안 함 (사라지면 끝) |
| **순서 보장** | O (보낸 순서대로 전달) | X (도착 순서 제멋대로) |
| **혼잡 제어** | O (네트워크 막히면 속도 자동 감속) | X (마음대로 쏨) |
| **속도/지연** | 느릴 수 있음 (대기 발생) | 빠름 (대기 없음) |
| **헤더 크기** | 20바이트 | 8바이트 |
| **HOL Blocking** | 발생함 (§6) | 없음 |
| **용도** | 웹·파일·로그인·채팅 | 실시간 게임·음성·영상 |

### 신뢰성 — TCP는 어떻게 "도착 보장"하나

TCP는 받은 쪽이 "잘 받았다(ACK)"고 답한다. 보낸 쪽은 일정 시간 안에 ACK가 안 오면
**그 패킷을 다시 보낸다(재전송)**. 그래서 절대 안 잃어버린다. 대신 이 재전송 대기가 지연을 만든다.

### 혼잡 제어 — TCP가 알아서 느려지는 이유

네트워크가 막히면 TCP는 전송 속도를 스스로 줄인다(혼잡 제어). 모두를 위한 매너지만,
게임 입장에선 "내가 안 막혔는데도 느려지는" 통제 불가 요소가 된다. UDP엔 이게 없다.

---

## 5. 패킷 손실 · 재정렬 · 지터

게임 네트워크 코드가 싸우는 3대 적:

**① 패킷 손실 (Packet Loss)**
- 패킷이 중간에 사라짐. 라우터 혼잡, 무선 간섭 등이 원인
- TCP: 재전송으로 메움 (느려짐)
- UDP: 그냥 없어짐 (앱이 알아서 대응)

**② 재정렬 (Reordering)**
- 패킷 #1, #2, #3 보냈는데 #1, #3, #2 순서로 도착
- 경로가 패킷마다 다를 수 있어서 발생
- TCP: 순서 맞춰 재정렬 후 전달 (그동안 대기 → §6)
- UDP: 도착하는 대로 앱에 넘김 (순서 책임은 앱)

**③ 지터 (Jitter)**
- 패킷 도착 **간격**이 불규칙한 것
- 예: 16ms마다 와야 하는데 10ms, 25ms, 8ms, 40ms로 들쭉날쭉
- 캐릭터가 끊기거나 순간이동하는 것처럼 보이는 원인
- 대응: 받는 쪽에 작은 버퍼(jitter buffer)를 둬서 잠깐 모았다 일정 간격으로 꺼냄

```
보낸 간격 (서버):  ●---●---●---●---●   (균일 16ms)
받은 간격 (클라):  ●--●------●-●----●   (지터 = 불규칙)
                  → 버퍼로 모았다가 다시 균일하게 재생
```

> 비유: §6 OS 문서의 "버퍼에 쌓였지만 앞 게 안 와서 못 꺼냄"과 똑같은 구조가 TCP HOL이다.

---

## 6. HOL Blocking — TCP의 결정적 약점

**HOL(Head-Of-Line) Blocking** = "줄 맨 앞 사람이 막히면 뒷사람 전부 못 감".

TCP는 **무조건 순서대로** 전달한다. 그래서 이런 일이 벌어진다:

```
서버가 보냄:   [#5][#6][#7][#8]
#5만 분실:         ↓
받은 쪽 버퍼:  [   ][#6][#7][#8]   ← #6,#7,#8 이미 도착해 있음!
                ↑
        하지만 앱은 #6,#7,#8 을 못 받는다.
        #5 가 재전송되어 도착할 때까지 전부 대기.
```

#6~#8은 이미 메모리에 와 있는데도, TCP가 "순서 보장" 약속 때문에 안 넘겨준다.
#5 재전송에 한 번의 왕복 시간(RTT, 예: 80ms)이 더 걸리고, 그동안 화면은 멈춘다.
이 멈춤이 쌓여 **끊김(stutter)과 입력 지연**으로 체감된다.

게임에선 #5가 "33ms 전 위치"라면 이미 낡은 정보다. **그걸 기다릴 바엔 버리고
#8(최신 위치)을 쓰는 게 낫다.** 그런데 TCP는 그걸 못 한다 → 그래서 UDP를 쓴다.

---

## 7. 왜 실시간 게임은 UDP를 고르나

핵심 한 줄: **게임에서 "오래된 데이터"는 가치가 없다. 늦게 정확한 것보다 지금 대충이 낫다.**

- 캐릭터 위치는 매 프레임 바뀐다. 33ms 전 위치를 재전송받아 봐야 이미 쓸모없음
- 잃어버린 패킷을 **기다리는 비용 > 그 패킷의 가치** → 차라리 버리고 다음 최신 패킷 사용
- UDP는 HOL Blocking이 없어서, 도착한 패킷을 즉시 게임 로직에 넘긴다
- 혼잡 제어가 없어 게임이 송신 타이밍을 **직접 통제**한다

대신 UDP는 신뢰성이 없으니, 게임은 **필요한 부분만 골라** 신뢰성을 직접 구현한다:
- 위치 업데이트 → 신뢰성 불필요 (다음 패킷이 곧 옴, 손실 무시)
- "플레이어 사망", "아이템 획득" → 한 번 놓치면 치명적 → 신뢰성 필요

이 "UDP 위에 골라서 신뢰성 얹기"가 바로 [[Reliable-UDP]] 주제이고, 언리얼에선
**reliable / unreliable RPC**로 노출된다 (§9).

---

## 8. RTT · 대역폭 · MTU — 숫자 감각

게임 네트워크를 말할 때 자주 쓰는 3가지 수치:

### RTT (Round-Trip Time, 왕복 시간)
- 패킷이 갔다 오는 데 걸리는 시간. 흔히 말하는 **핑(ping)**
- 예: RTT 60ms = 내 입력이 서버 갔다 결과 돌아오는 데 60ms
- 좋은 환경 20~50ms, 100ms 넘으면 체감 렉, 같은 대륙 내 보통 30~80ms
- 편도 지연(latency)은 대략 RTT의 절반

### 대역폭 (Bandwidth)
- 단위 시간당 보낼 수 있는 데이터 양 (예: 10 Mbps)
- "도로 폭". 넓어도 거리가 멀면 RTT(지연)는 그대로다 → **대역폭 ≠ 지연**
- 게임은 보통 대역폭은 적게 쓰고(초당 수~수십 KB) 지연에 민감

### MTU (Maximum Transmission Unit, 최대 전송 단위)
- 한 번에 보낼 수 있는 패킷의 최대 크기. 이더넷에서 보통 **1500바이트**
- 이걸 넘으면 패킷이 쪼개진다(IP fragmentation) → 조각 하나만 잃어도 전체 재조립 실패
- 그래서 게임은 한 패킷을 MTU 안에 욱여넣으려 한다. UDP 페이로드 실용 상한은
  헤더(IP 20 + UDP 8)를 빼고 대략 1472바이트 ⚠️(환경별로 다름, 보수적으로 더 작게 잡기도)

```
[ IP 헤더 20B ][ UDP 헤더 8B ][      게임 데이터 (≤1472B)      ]
└──────────────────── 1500B (MTU) 이내 ────────────────────┘
```

---

## 9. UE 실제 API — UDP 위에서 RPC

언리얼 엔진의 멀티플레이는 **UDP** 위에서 동작하며, 데이터를 **비트 단위(bit-level)로 직렬화**해
대역폭을 아낀다(예: bool 1개를 1바이트가 아닌 1비트로 패킹). 개발자는 raw 소켓을 직접 만지지 않고,
**RPC(원격 프로시저 호출)**로 "한 컴퓨터에서 함수를 호출해 다른 컴퓨터에서 실행"한다.

RPC는 `UFUNCTION` 매크로의 지정자로 정의한다. 방향 지정자(`Server`/`Client`/`NetMulticast`)와
신뢰성 지정자(`Reliable`/`Unreliable`)를 함께 붙인다:

```cpp
// 클라이언트 → 서버로 보내는 RPC. 신뢰성 보장(도착 보장).
// Server RPC는 이 액터를 소유한 클라이언트에서만 호출 가능. 본문은 _Implementation에 구현.
// 보안이 필요하면 WithValidation을 붙이고 _Validate를 구현(검증 실패 시 클라이언트 강제 disconnect).
UFUNCTION(Server, Reliable, WithValidation)
void Server_FireWeapon(FVector_NetQuantize TargetLocation);

// 서버 → 모든 클라이언트(멀티캐스트). 자주 불리므로 Unreliable.
UFUNCTION(NetMulticast, Unreliable)
void Multicast_PlayHitEffect(FVector_NetQuantize HitLocation);

// 서버 → 특정 클라이언트. 중요한 1회성 이벤트라 Reliable.
UFUNCTION(Client, Reliable)
void Client_OnMatchEnded(int32 WinnerTeamId);
```

```cpp
// 구현부 — 지정자가 붙은 RPC는 _Implementation 접미사로 실제 본문을 작성한다.
void AMyCharacter::Server_FireWeapon_Implementation(FVector_NetQuantize TargetLocation)
{
    // 이 코드는 서버에서 실행된다.
}

// WithValidation을 붙였으면 _Validate도 반드시 구현. false 반환 시 클라이언트가 disconnect된다.
bool AMyCharacter::Server_FireWeapon_Validate(FVector_NetQuantize TargetLocation)
{
    return true; // 예: 사거리·쿨다운 등 서버 권위 검증
}
```

공식 문서 핵심 규칙:
- **RPC는 기본이 unreliable**이다. `Reliable`을 명시해야 신뢰성이 붙는다.
- **Reliable RPC**는 수신자가 ACK할 때까지 재전송되어 **반드시 전달**된다. 단 큐를 점유하므로 대역폭을 더 쓴다.
- **Unreliable RPC**는 패킷이 드롭되면 실행되지 않지만, 더 자주·더 빠르게 보낼 수 있다.
- **캐릭터 이동(CharacterMovementComponent의 ServerMove)은 unreliable RPC로 보낸다** — 매 tick 호출되므로
  reliable이면 큐가 넘쳐 연결이 끊긴다. (참고: 일반 Actor의 위치/회전은 RPC가 아니라
  **프로퍼티 복제**(ReplicatedMovement)로 동기화된다. "이동 = unreliable RPC"는 Character 한정 규칙)
- 액터의 **Tick 함수처럼 매우 자주 호출되는 RPC는 unreliable로** 만들라고 공식 권장.
- **Server RPC는 `WithValidation`으로 검증 함수를 붙일 수 있고**, 검증 실패 시 해당 클라이언트는 서버에서 강제 연결 해제된다.

이것이 §7의 "위치는 손실 무시, 중요 이벤트만 신뢰성"이 언리얼에서 구현된 모습이다.

---

## 10. 흔한 함정

**① "UDP니까 빠르다"고 무조건 UDP**
→ 로그인·결제·채팅처럼 손실되면 안 되는 건 TCP(또는 reliable RPC)가 맞다.
   UDP는 "손실돼도 다음 게 곧 오는" 데이터에만 유리하다.

**② 매 Tick마다 Reliable RPC 호출**
→ ⚠️ 공식 경고. Reliable은 도착할 때까지 큐에 쌓인다. 플레이어가 버튼을 연타하거나
   Tick에서 reliable을 쏘면 **큐가 넘쳐(overflow)** 연결이 끊길 수 있다. 자주 부르는 건 unreliable.

**③ 대역폭과 지연을 혼동**
→ "인터넷 빠른데 왜 렉?" — 대역폭(도로 폭)이 넓어도 거리가 멀면 RTT(지연)는 그대로다.
   게임 렉의 주범은 보통 대역폭이 아니라 RTT와 패킷 손실.

**④ 큰 패킷을 MTU 무시하고 전송**
→ 1500B(MTU)를 넘으면 IP 단편화가 일어나고, 조각 하나만 잃어도 전체가 버려진다.
   한 패킷은 MTU 안에 맞춰라.

**⑤ UDP가 순서를 지켜줄 거라 가정**
→ UDP는 순서 보장이 전혀 없다. #1,#2,#3을 보내도 #3,#1,#2로 올 수 있다.
   순서가 필요하면 **시퀀스 번호를 직접** 붙여야 한다 → [[Reliable-UDP]]에서 다룸.

---

## 11. 면접 Q&A

**Q1. 실시간 게임은 왜 TCP가 아니라 UDP를 쓰나요?**
> TCP는 순서 보장 때문에 HOL Blocking이 발생합니다. 패킷 하나가 손실되면 그 뒤에
> 이미 도착한 패킷들까지 재전송이 완료될 때까지 앱에 전달되지 않아 멈춤(stutter)과
> 입력 지연이 생깁니다. 게임에서 33ms 전 위치 같은 낡은 데이터는 가치가 없으므로,
> 그걸 기다릴 바엔 버리고 최신 패킷을 쓰는 게 낫습니다. UDP는 HOL Blocking이 없고
> 혼잡 제어도 없어 송신 타이밍을 직접 통제할 수 있습니다. 손실에 민감한 일부 데이터만
> UDP 위에 신뢰성을 직접 얹습니다(언리얼의 reliable RPC).

**Q2. HOL Blocking이 정확히 뭔가요?**
> Head-Of-Line Blocking. 줄 맨 앞이 막히면 뒤가 전부 못 가는 현상입니다. TCP는
> 순서를 보장하므로, 5번 패킷이 손실되면 6·7·8번이 이미 수신 버퍼에 도착해 있어도
> 5번이 재전송돼 채워질 때까지 앱에 넘겨주지 않습니다. 이 대기가 지연과 지터를 키웁니다.

**Q3. 대역폭이 충분한데 게임이 렉이 걸리는 이유는?**
> 대역폭(초당 데이터량)과 지연(RTT)은 다른 축입니다. 대역폭이 넓어도 서버가 물리적으로
> 멀면 RTT는 그대로입니다. 게임 체감 렉의 주원인은 대역폭 부족보다 높은 RTT와 패킷
> 손실·지터인 경우가 많습니다. 게임은 데이터 양 자체는 적게(초당 수십 KB) 쓰는 편입니다.

**Q4. 언리얼에서 캐릭터 이동은 reliable로 보내나요 unreliable로 보내나요?**
> Unreliable입니다. CharacterMovementComponent는 매 tick마다 ServerMove RPC를 unreliable로 보냅니다.
> 자주 호출되므로 reliable이면 큐가 넘쳐 연결이 끊기고, 한두 패킷이 손실돼도 saved move 버퍼가
> 재전송·재평가하기 때문입니다. 공식 문서도 Tick처럼 자주 호출되는 RPC는 unreliable로 하라고
> 권장합니다. 반대로 "사망"·"매치 종료" 같은 1회성 중요 이벤트는 reliable로 보냅니다.
> (참고: 일반 Actor의 위치는 RPC가 아니라 프로퍼티 복제로 동기화됩니다.)

---

## 출처

- [Networking Overview for Unreal Engine | Epic 공식 문서](https://dev.epicgames.com/documentation/en-us/unreal-engine/networking-overview-for-unreal-engine) — RPC reliable/unreliable 동작, "자주 호출되는(Tick) RPC는 unreliable" 권장
- [Remote Procedure Calls in Unreal Engine | Epic 공식 문서](https://dev.epicgames.com/documentation/en-us/unreal-engine/remote-procedure-calls-in-unreal-engine) — Server/Client/NetMulticast + Reliable/Unreliable 지정자, "RPCs are unreliable by default", _Implementation 패턴, Server RPC는 owning client에서만 호출, WithValidation 검증 실패 시 disconnect
- [Understanding Networked Movement in the CharacterMovementComponent | Epic 공식 문서](https://dev.epicgames.com/documentation/en-us/unreal-engine/understanding-networked-movement-in-the-character-movement-component-for-unreal-engine) — ServerMove가 unreliable RPC인 이유(빈도/큐 overflow), saved move 버퍼 재전송
- [Networking and Multiplayer in Unreal Engine | Epic](https://dev.epicgames.com/documentation/en-us/unreal-engine/networking-and-multiplayer-in-unreal-engine) — 복제(Replication) 개념
- [UDP vs TCP / HOL Blocking in multiplayer gaming (systemdr)](https://systemdr.substack.com/p/udp-vs-tcp-in-multiplayer-gaming) — HOL Blocking이 게임 stutter·jitter로 체감되는 메커니즘, 30~128Hz 송신
- [UDP vs TCP Complete Guide (Pinggy)](https://pinggy.io/blog/udp_vs_tcp_complete_guide/) — TCP/UDP 헤더 크기, 혼잡 제어, 신뢰성/순서 차이

> 검증 메모 (2026-06-16, 적대적 사실검증 통과):
> - **공식 문서로 직접 확인됨**: RPC는 기본 unreliable / `Reliable` 명시 시 ACK까지 재전송 /
>   `_Implementation` 본문 패턴 / Server RPC는 owning client에서만 호출 / `WithValidation` 검증 실패 시
>   클라이언트 강제 disconnect / "tick처럼 자주 호출되는 RPC는 unreliable로" 권장 / reliable RPC를
>   플레이어 입력에 묶으면 큐 overflow 위험. (Remote Procedure Calls / Networking Overview 페이지)
> - **CharacterMovementComponent의 ServerMove가 unreliable RPC**라는 점은 "Understanding Networked
>   Movement" 공식 문서로 확인됨. 단 일반 Actor 위치는 프로퍼티 복제(ReplicatedMovement)이므로,
>   "이동 = unreliable RPC"를 모든 Actor에 일반화하지 않도록 본문에서 한정함.
> - **bit-level serialization**: UDP 사용 + 비트 단위 직렬화는 다수 출처에서 일관 확인. fetch한 공식
>   페이지 본문에서 단어 그대로 인용하지는 못해 ⚠️ 표기 유지 (개념 자체는 정설).
> - **TCP 헤더 20B / UDP 헤더 8B / 이더넷 MTU 1500 / UDP 페이로드 1472**: 표준 네트워킹 값.
>   경로·터널링(VPN/PPPoE) 환경에 따라 실효 MTU는 더 작을 수 있어 보수적으로 잡는다. ⚠️ 환경 의존.
