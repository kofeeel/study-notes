---
title: Motion Warping 개념서
tags: [UE5, animation, motion-warping, root-motion, study]
created: 2026-05-16
updated: 2026-05-16
sources:
  - https://dev.epicgames.com/documentation/en-us/unreal-engine/motion-warping-in-unreal-engine
  - https://dev.epicgames.com/documentation/en-us/unreal-engine/root-motion-in-unreal-engine
  - https://dev.epicgames.com/community/learning/tutorials/r9bP
---

# Motion Warping 개념서

## 목표
- 보정 대상(Root Motion) 이해 / 3종 Modifier 차이 / Warp Target 동적 갱신 흐름 / **보스 공격 추적 패턴**

## 핵심

### 시스템 구성

| 개념 | 역할 | 디테일 |
|---|---|---|
| **MotionWarpingComponent** | Character에 부착. Warp Target 관리 + Root Motion Delta 가공 | `AddOrUpdateWarpTargetFromTransform()` 코드로 추가 |
| **Warp Target** | 도달할 Transform (FName 키로 등록) | 매 프레임 갱신 가능. 이름이 키 |
| **AnimNotifyState_MotionWarping** | Montage 구간에 부착해 Modifier + Target Name 지정 | Notify 시작 시점이 적용 시작점 |
| **Root Motion Source** | Component가 만든 Delta를 캐릭터 이동에 주입 | CharacterMovement의 RootMotionSource 큐 활용 |

### Modifier 종류

| 종류 | 작동 방식 | 사용 케이스 |
|---|---|---|
| **SimpleWarp** | Notify 구간 끝의 Root Motion이 Target에 정확히 도달하도록 평행 이동 보정 | 점프 착지, 그랩 시작 위치 등 **끝점만 중요할 때** |
| **Skew Warp** | Translation을 Target 쪽으로 비스듬히 보간. 경로 자체를 기울임 | 옆/대각선 닷지, 회전 추적 공격 등 **경로 자연스러움 중요할 때** |
| **Scale Root Motion** | Root Motion Translation/Rotation 크기를 곱셈으로 조정 | 같은 동작 다른 거리. 작은/큰 보스 |

**작동 흐름**: Montage 시작 → Notify 진입 → Component가 FName으로 Target 조회 → 매 프레임 Root Motion Delta 가공 → Notify 종료 시 Target에 정확히 도달

### 인간형 보스에 쓰나? — **YES, 거의 필수**

소울라이크/액션 보스는 Motion Warping이 **메인 시스템**. Motion Matching보다 훨씬 자주 쓰임.

| 보스 공격 패턴 | Motion Warping 사용 방식 |
|---|---|
| **점프 슬램** (위에서 내려찍기) | 점프 시작 시 플레이어 위치를 Warp Target으로 등록 → SimpleWarp로 정확히 착지 |
| **그랩 어택** (잡기) | 돌진 구간에 Warp Target = 플레이어 위치 → Skew Warp로 정확한 거리 조정 |
| **회전 베기** | 공격 시작 시 플레이어 방향 → Orient만 Warp (회전 보정) |
| **돌진 공격** | 시작 시 플레이어 위치 등록 → 거리에 따라 Scale Root Motion으로 동작 늘림/줄임 |
| **다단 콤보** | 각 타격마다 새 Warp Target 갱신 → 콤보 중에도 플레이어 추적 |

**실무 패턴 코드** (Pseudo):
```cpp
// 보스 공격 시작 시
FTransform Target = Player->GetActorTransform();
MotionWarpingComp->AddOrUpdateWarpTargetFromTransform(
    FName("EnemyTarget"), Target);
PlayAnimMontage(Attack_Jump_Slam_Montage);

// Montage 내부 AnimNotifyState_MotionWarping:
//   WarpTargetName = "EnemyTarget"
//   WarpMode = SimpleWarp 또는 Skew Warp
```

**Sekiro / Elden Ring / Stellar Blade**: 모든 보스 공격이 이 패턴. 플레이어가 백스텝해도 보스가 매 프레임 거리 보정하며 추적.

### Dodge 방향 자연화 패턴 (너의 데모용)

Root Motion Dodge Montage 1개 + 매 프레임 Warp Target을 입력 방향 회전값으로 갱신 → **8방향 동적 닷지 완성** (Dodge Montage 8개 만들 필요 X)

### Motion Matching과의 분담

| 시스템 | 작동 시점 | 주체 |
|---|---|---|
| Motion Matching | 일반 이동 (입력 따라) | DB 검색 결과 프레임 |
| Motion Warping | Root Motion Montage 재생 중 | Montage Root Motion을 Target에 맞춤 |

→ **두 시스템은 직교.** 서로 충돌 안 함. 같이 쓰는 게 표준.

### 한계

Root Motion 없는 클립엔 적용 X / Target 너무 멀면 애니 늘어남 / Capsule이 Mesh 못 따라가면 Replication 어긋남 (서버 권한) / Multi-target 시 Modifier 순서 의존성 / 너무 잦은 Warp = 동작 일관성 깨짐

## 체크
1. SimpleWarp vs Skew Warp 차이? → Simple은 끝점만 정확, Skew는 경로 전체를 기울임
2. Warp Target 갱신 시점? → Notify 진입 + 매 프레임 (Component가 FName으로 조회)
3. Dodge 방향 어색 시 1차 해법? → Orient Warp Target 매 프레임 입력 방향으로 갱신
4. 보스 점프 슬램에 쓸 Modifier? → SimpleWarp (끝점만 정확하면 충분)
5. 보스 그랩 어택에 쓸 Modifier? → Skew Warp (경로 자연스러움 + 거리 보정)
6. Motion Matching과 충돌? → 직교. MW는 Root Motion Montage에만, MM은 일반 이동에만 작동
7. Replication 주의점? → Root Motion Source 서버 권한. Warp Target도 서버 동기화 필요
