---
title: "적 AI 포지셔닝 — 토큰/EQS/Crowd 직교 모델 + EQS 채택 기준"
tags: ["enemy-ai", "eqs", "positioning", "behavior-tree", "detour-crowd", "decision", "kiting", "token", "debugging", "ranged-enemy"]
created: 2026-06-04T15:24:56.299Z
updated: 2026-06-04T15:27:52.705Z
sources: ["Source/Project_KD/Enemy/AI/BTService_RequestAttackToken.cpp", "Source/Project_KD/Enemy/AI/BTTask_SelectAttack.cpp", "Source/Project_KD/Enemy/AI/BTService_FindPlayer.cpp", "Source/Project_KD/Enemy/AI/BTTask_FindKitingLocation.cpp"]
links: ["ranged-enemy-projectile-aiming.md"]
category: architecture
confidence: high
schemaVersion: 1
---

# 적 AI 포지셔닝 — 토큰/EQS/Crowd 직교 모델 + EQS 채택 기준

# 적 AI 포지셔닝 — 토큰/EQS/Crowd 직교 모델

> 2026-06-04 확정. 적 AI의 "어디에 설지/어떻게 접근할지"를 다루는 관심사 분리 원칙.
> 공유 BT 골격 + 데이터드리븐 아키텍처의 일부(god-tree 방지, CLAUDE.md §1).

## 직교 3축 (load-bearing 원칙)

세 시스템이 **서로 안 겹침** — 각자 다른 질문에 답한다. 한 축을 바꿔도 나머지에 영향 X.

```
토큰(EncounterSubsystem, A4) = "누가 칠지"    동시 공격자 ≤2, hold-time 2.5s 로테이션
EQS                          = "어디 설지/접근할지"   (신규)
MoveTo + Detour Crowd        = "그 목적지까지 경로 + 국소 회피"
```

- 토큰은 **공격 분기**에만 데코레이터로 걸림(`bHasAttackToken`). 포지셔닝과 무관.
- EQS는 목적지(BB Vector 키)만 고른다. **발동 여부는 SelectAttack + 토큰이 기존대로 게이트.**
- Detour Crowd(`CrowdFollowingComponent`, RVO 배타·NavMesh 전제)는 MoveTo 실행 중 국소 회피. EQS가 고른 점까지 가는 *방법*.
- → EQS 결과를 기존 kiting 노드가 쓰던 **그 `OutputLocationKey`에 그대로 기록**하면 스톡 MoveTo가 소비 = 드롭인.

**새 포지셔닝 로직 추가 시 이 3축 중 어디 소관인지 먼저 분류할 것.** 한 노드가 둘 이상 묶으면 분리 신호(예: kiting 노드가 "언제 후퇴(게이트)"와 "어디로(벡터)"를 묶고 있었음 → EQS 전환 시 후자만 교체, 게이트는 데코레이터로 잔존).

## EQS 채택 기준 (언제 EQS를 당기나)

스펙이 *"LOS 라인트레이스로 시작, 부족 시 EQS"*로 에스컬레이션 경로를 깔아둠. **"부족"의 트리거 = PIE에서 적이 바보처럼 보일 때**(직선 정면 접근 / 로봇 같은 직선 kiting / 코너 자가감금). 관측되면 도입 = 스펙 위반 아니라 에스컬레이션 따르기.

- 포폴 프로젝트에서 **AI 포지셔닝은 명시적 1순위 학습목표** → EQS는 부채가 아니라 자산. 여기선 YAGNI를 과하게 적용하지 말 것.
- 본격 포지셔닝 AI = 보스 마일스톤. 잡몹/엘리트는 필요한 만큼만.

## EQS 성능 가이드 (우리 규모 = 아레나 3~12마리)

**성능은 EQS 회피 사유가 아님.** 비용 = (생성 포인트 수) × (테스트 수) × (테스트당 비용, 예: LoS 트레이스). 비싸지는 건 *조밀 그리드 × 매 프레임 × 트레이스 많음*. 정상 설계면 스파이크 없음:

1. 쿼리를 **BTService 간격(0.5~1s)**으로. 매 프레임 X.
2. 풀그리드 대신 **플레이어 주변 Donut/링 제너레이터**로 포인트 수 제한(예 2링×N섹터 ≈ 12~24점).
3. **싼 테스트(Distance) 먼저 → 비싼 테스트(Trace) 나중.**

## 표준 EQS 설계 형태 (잡몹/엘리트)

- **제너레이터: Donut, 중심 = 플레이어(타겟)** — querier(적) 아님. "위협으로부터 X거리" = 내가 서고 싶은 위치라 의미 직결. standoff면 반경≈StandoffRange, melee 접근각이면 반경≈AttackRange~×2.
- **테스트 = 빌트인으로 충분(M2)**: Trace(LoS→플레이어, 쏠 수 있는 점만) + Pathfinding(navmesh 밖/코너 점 탈락) + Distance→querier(가까운 점 선호, 순간이동 방지). melee 포위는 +Distance→다른 적(분산). **커스텀 Test 0.**
- **커스텀 C++ 최소화**: Donut 중심을 플레이어로 잡으려면 `EnvQueryContext_Target`(BB TargetActor 읽기, ~20줄) 1개 필요. melee 포위 분산엔 `EnvQueryContext_AllyEnemies`(Team.Enemy 집합) 추가. 둘 다 AIModule 소속 → 모듈 의존성 추가 0.
- **BT 배선 = 빌트인 `Run EQS Query` 태스크** → BB Location 기록 → MoveTo. 신규 BT 노드 거의 0.

## 관련 코드

- `Source/Project_KD/Enemy/AI/BTTask_FindKitingLocation.{h,cpp}` — 현 벡터 기반 kiting(EQS가 벡터 부분만 교체 대상). 밴드 게이트(TriggerRatio/DangerRange/Backstep 분기)는 재사용.
- `Source/Project_KD/Enemy/AI/EncounterSubsystem.{h,cpp}` — 토큰(누가).
- `KDEnemyAIController` 생성자 `CrowdFollowingComponent` — Detour Crowd(어떻게).

---

## Update (2026-06-04T15:27:52.705Z)

## PIE 검증 교훈 — 공유 BT 거리밴드/분기 정합 함정 (2026-06-05, 활 잡몹)

공유 BT + 데이터드리븐에서 "적이 안 쏘고 따라옴 / 카이팅 지옥 / 첫발만 쏨"은 거의 다 **거리밴드 갭 + 분기 데코 조건 미스**. 코드는 멀쩡한데 데이터(거리/엔트리)·BT 배선이 안 맞는 케이스. 5개 함정 + 진단법:

1. **토큰은 근접 자원 — 원거리는 면제.** 토큰(동시공격 ≤N 제한)을 원거리에 적용하면, 토큰 못 받은 활이 reposition/chase로 빠져 플레이어한테 비빔. `BTService_RequestAttackToken`에서 `StandoffRange>0`(=원거리 kiter)이면 `HasToken=true` 강제 + `ShouldReposition=false`. (위 직교 모델의 토큰 축은 사실상 melee 전용)

2. **chase 데코에 거리 조건 필수.** chase 분기가 `target`만 보면 사거리 안에서도(사격 쿨다운 순간 등) chase로 빠져 비빔. chase 데코 = `target AND bCanAttack==false`(사거리 밖일 때만). melee/원거리 공통으로 옳음.

3. **공격 엔트리 MinRange가 "공격 사각"을 만든다.** 사격 Min=1000인데 그 아래를 덮는 엔트리(백스텝 0~250)만 있으면 250~1000은 후보0 → 못 쏘고 kiting/chase로 어정거림. 밴드를 빈틈없이 덮을 것.

4. **회피/백스텝의 자리 = SelectAttack 거리밴드 엔트리(Min0/Max250), BT 노드 아님.** 백스텝을 kiting Sequence에 노드로 박으면 매 틱 "백스텝→MoveTo→백스텝" 갈팡질팡. `SelectAttack→ActivateAbilityByTag` 파이프 재사용(거리로 자동 픽) + GA를 `StartupAbilities`에 부여 필수(누락 시 발동 0).

5. **거리 부등식 (활):** `0~Danger 백스텝 < Danger~Trigger kiting < Trigger~MaxRange 사격 ≤ AttackRange ≈ 사격MaxRange ≤ Sight`. `AttackRange`=`bCanAttack` 스위치(`BTService_FindPlayer`)=chase 멈춤거리. **`AttackRange > 사격MaxRange`면 그 사이가 데드존**(bCanAttack=true인데 후보0) → chase. `사격MaxRange ≥ AttackRange`로 맞출 것.

**진단법 (재사용):** `SelectAttack::ExecuteTask`에 임시 디버그 HUD — `#if !UE_BUILD_SHIPPING` + `GEngine->AddOnScreenDebugMessage`로 진입(`d=`, `entries=`), 엔트리별 탈락 사유(`OUTRANGE` / `NO SPEC`=StartupAbilities 누락 / `CD/blocked`), 픽 결과 출력. PIE 화면에서 "왜 공격 안 하는지" 즉시 특정됨(`d=134 entries=2 NO CANDIDATE` → 백스텝 엔트리 누락으로 특정한 실사례). `.cpp`만이라 Live Coding 반영, 진단 끝나면 제거.

**원거리 발사 조준은** → [[ranged-enemy-projectile-aiming]] 참고.

---

## Update (2026-06-06) — melee 포위 EQS 검증 완료 + Visibility 트레이스 함정

`EQ_MeleeSurround` PIE 검증 DONE("자연스러워짐"). **근본 함정 = Visibility Trace 테스트를 Filter로 쓰면 전멸.**

- **증상**: 전 도넛 포인트가 Visibility 트레이스에서 `'Boolean score doesn't match (expected TRUE got FALSE)' score:0.000` → `OutputLocation:(invalid)` → reposition 실패. *일부*가 아니라 **전멸**이면 occlusion(적끼리 가림)이 아니라 체계적 원인(필터 모드 / 바닥 긁는 트레이스).
- **해법(검증됨)**: Visibility Trace 테스트의 **TestPurpose = Score Only** (Filter And Score / Filter 아님). Filter면 LoS 막힌 점을 전부 *버려서* 후보 0 → invalid. Score Only는 점을 안 버리고 가시성 있는 점을 *선호*만 함 → 다 막혀도 최선의 점을 고름. **melee 포위는 "보이는 자리 선호"지 "안 보이면 금지"가 아님 → Score가 맞다.**
  - cf. line 55의 "Trace(쏠 수 있는 점만)"은 **원거리 standoff(쏴야 하니 LoS 필수)** 한정 = Filter 적합. **melee 포위엔 Score Only.** 같은 Trace 테스트라도 역할 따라 모드 다름.
- **Donut 파라미터(검증값)**: 호 각도(Arc Angle) **360°**(전방위 포위, 한쪽만 안 서게) + 반경 **200~450**(InnerRadius~OuterRadius, 적정 교전거리 링).
- **참고**: height offset(Item/Context Height Offset) 가설은 빗나감 — Score Only 전환이 바닥-긁힘까지 같이 우회(막힌 점도 안 버리므로). 전멸 디버깅 시 **Filter 모드부터 의심**할 것.
