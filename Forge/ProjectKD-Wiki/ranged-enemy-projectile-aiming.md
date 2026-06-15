---
title: "Ranged Enemy Projectile Aiming — 원거리 적 발사체 조준/궤적"
tags: ["projectile", "ranged-enemy", "gas", "aiming", "gameplay-ability"]
created: 2026-06-04T15:29:01.358Z
updated: 2026-06-04T15:29:01.358Z
sources: []
links: ["ai-eqs-crowd-eqs.md"]
category: pattern
confidence: medium
schemaVersion: 1
---

# Ranged Enemy Projectile Aiming — 원거리 적 발사체 조준/궤적

# 원거리 적 발사체 — 조준/궤적 (직선 vs 포물선)

`AKDProjectile`(`UProjectileMovementComponent` 보유) + 발사 GA(`GA_EnemyRangedAttack`)가 몽타주 발사 프레임에 스폰. (2026-06-05 검증)

## 조준 = muzzle→타겟 방향 (actor forward 금지)
- `Avatar->GetActorRotation()`(수평 forward)으로 쏘면 **높이차/점프(더블점프 공중) 플레이어를 못 맞춤.** 컨트롤러 SetFocus는 yaw(수평)만 추적.
- spawn rotation = `(TargetLoc - MuzzleLoc).GetSafeNormal().Rotation()`.
- `ProjectileMovement`는 `InitialSpeed>0` + `bRotationFollowsVelocity=true`라 **spawn rotation의 forward로 자동 발사** → 별도 velocity 안 박아도 그 방향 직진.
- 타겟 = 싱글이면 `UGameplayStatics::GetPlayerPawn(WorldContext, 0)`의 **현재 위치**(리딩 없음, M2 단순). 점프는 순간 위치라 잘 맞고, 빠른 횡이동만 살짝 빗나감 → 필요 시 속도×비행시간 리딩 추가.

## 직선 vs 포물선
- **직선** = `ProjectileGravityScale=0`. 액션게임 잡몹 활 표준 — 빠른 직사 = 플레이어 회피/패링 타이밍 게임.
- **포물선 = 베지에 아니라 `ProjectileGravityScale>0`(물리 중력).** 베지에는 정해진 경로(유도탄/연출)라 자유 사격에 부적합. 단순 중력만 켜면 직사 발사 후 낙하라 먼 거리 안 맞음 → 발사각/탄도 계산 필요 = 복잡(리딩과 결합).
- **권장: 직선 유지.** 포물선은 차지샷을 "착탄 예고형"으로 차별화하는 카드로만 보류(일반 사격은 직사).

## 높이차 대응 우선순위
높이차 맵에서 안 맞으면 — **포물선보다 "조준 방향을 타겟으로"가 먼저**다(직선 유지하며 위/아래 명중). 발사체 궤적 변경은 그다음.

관련: [[ai-eqs-crowd-eqs]] (포지셔닝/거리밴드/디버깅)
