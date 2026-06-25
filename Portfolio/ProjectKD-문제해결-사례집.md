---
title: ProjectKD — 문제 해결 / 디버깅 사례집
tags:
  - ProjectKD
  - portfolio
  - debugging
  - root-cause
  - case-study
created: 2026-06-15
status: 포폴용 v1
related:
  - "[[ProjectKD-담당작업-시스템정리]]"
  - "[[ProjectKD-전투시스템-원페이저]]"
---

# ProjectKD — 문제 해결 / 디버깅 사례집

> 포트폴리오 "이터레이션 스토리" 소재. 각 사례를 **증상 → 근본 원인 → 해결 → 배운 점**으로 정리.
> 공통 테마: *증상 ≠ 원인*. 추측 수정 대신 데이터 흐름/콜스택을 따라가 진범을 분리한다.

| # | 사례 | 한 줄 | 난이도 |
|---|---|---|---|
| 1 | 엘리트 도끼 패링이 간헐적 | 패링은 트레이스 히트에 100% 게이팅 — 3계층 해결 | ★★★ |
| 2 | 사망 시 몸이 이상하게 꺾임 / 공중 정지 | 진범=PhysicsAsset(코드 무죄), 비물리 사망 시도 전부 실패 | ★★★ |
| 3 | 적이 안 쏘고 따라옴 / kiting 지옥 | 거리밴드 갭 + 분기 데코 미스(코드 멀쩡) | ★★ |
| 4 | 복제한 적 BP가 "피규어처럼" 움직임 | BP Duplicate가 AnimInstance tick 꼬임 — 3시간 삽질 | ★★ |
| 5 | patrol 복귀 중 플레이어 보며 뒷걸음 | FindPlayer가 거리 무관 매 틱 SetFocus | ★★ |
| 6 | 처형 생존 복귀 후 stagger 모션 노출 | 처형 경로가 불필요한 스태거 몽타주 재생 | ★ |
| 7 | 히트스탑 영구 정지 (콤보 치명적) | SetPlayRate(0)+캡처 → 재진입 영구정지 | ★★ |
| 8 | BT 런타임 로직 introspection 막힘 | protected 멤버 벽 → Gameplay Debugger로 선회 | ★ |
| 9 | 포위전 토큰 없는 적이 플레이어한테 등 돌림 | 사례 5의 facing 게이트(195)가 EQS 포위 도넛(200~450)과 충돌 | ★★ |
| 10 | 패링(가드) 홀드 중 이동이 전혀 안 됨 | 코드 무죄 — 블록 몽타주 루트모션 ON + 상하체 슬롯 미지정 | ★ |

---

## 1. 엘리트 도끼 패링이 간헐적

**증상** — 도끼 다단 공격(3연찍기·2연휘두르기)에서 1타만 패링, 2타+ 무반응. 세로/대각 찍기는 캡슐 반지름을 키워도 패링 안 됨.

**근본 원인** — "패링이 안 된다"를 GE/패링 로직이 아니라 상류 트레이스 문제로 환원. 세 갈래:
1. **다단 1타만 패링**: once-per-actor 필터가 2겹인데 스코프가 어긋남. GA 레벨 `bOncePerActor`가 몽타주 *전체* 스코프 → 1타가 플레이어를 `AlreadyHitActors`에 추가 → 2·3타 데미지 없음 → 패링 게이트 도달 못 함.
2. **Rush 오버슈트**: 워프 타겟을 *발동 순간* 플레이어 위치로 1회 고정 → 실제 워프는 윈드업 뒤라, 그 사이 플레이어 접근하면 고정점이 플레이어 너머 → 앞으로 달렸다 되돌아옴.
3. **세로 찍기 패링 실패**: 캡슐은 무기축(자루 전체)을 따라 길쭉 → 세로 찍기에서 **날 끝이 머리 높이 들어오는 순간(도끼 아직 위) 이미 히트** → 게이트가 시각보다 *먼저* 발화 → 플레이어가 "몸에 닿을 때" 누르면 늦음. **반지름=폭(WHERE), 패링 난이도=타이밍(WHEN). 폭으론 타이밍 못 고침.**

**해결 (3계층)**
1. `GA_EnemyWeaponTraceBase` 생성자에서 `bOncePerActor=false` → 적 근접 전부 "윈도우당 1히트". 비대칭을 분기가 아니라 per-faction CDO 디폴트로 표현.
2. `Event.Rush.Warp` 태그 + `AN_SendGameplayEvent`를 돌진 윈도우 시작에 배치 → *돌진 시작 순간* 현재 플레이어 위치로 재조준. 노티 미배치 시 옛 동작으로 graceful degrade.
3. 윈도우별 트레이스 오버라이드(`ANS_WeaponTrace`에 소켓/반지름/모드 얹음) → 찍기 윈도우만 도끼 머리 소켓으로, 히트 시점=시각 일치.
+ 설계 원칙: 히트 정확도(적) ⊥ 패링 관대함(플레이어 윈도우) 분리.

**배운 점** — 모호한 "가끔 안 됨" 버그를 GAS 데미지 파이프라인의 데이터 흐름까지 추적해 진짜 게이팅 구조를 규명. 타이밍 실패를 "판정이 좁다"가 아니라 "히트가 이르다"로 정확히 진단(기하학적 원인까지). → 상세 위키 [[weapontrace]]

---

## 2. 사망 시 몸이 이상하게 꺾임 / 처형 후 공중 정지

**증상** — 랙돌이 시체를 이상하게 꺾음. 비물리 사망을 시도하니 처형 후 캐릭터가 공중에서 멈춤. 죽음 몽타주 끝에 시체가 반쯤 기립했다 무너짐(포즈 팝).

**근본 원인**
- 꺾임의 진범은 **PhysicsAsset** — 코드는 표준 `SetCollisionProfileName("Ragdoll")` + `SetSimulatePhysics(true)`만 함. 비균일 스케일 탓도 아니었음.
- 공중 정지: `bPauseAnims`(프레임 동결)는 물리가 없어 공중 프레임이 박제됨.
- 포즈 팝: 랙돌 인계를 몽타주 End(블렌드아웃 *완료* 후)에 걸면 ABP가 idle로 되돌리는 블렌드가 끼어듦.
- 죽는 타격에 움찔: `AS_Combat`가 Health 차감 *전에* HitReact 이벤트를 보내 죽음 몽타주와 같은 프레임 충돌.

**해결**
- PhysicsAsset 교체(에디터) → 꺾임 해소. 코드는 무죄로 확정.
- 랙돌 하이브리드 유지. 인계를 블렌드아웃 *시작*(`Montage_SetBlendingOutDelegate`)으로 → 포즈 팝 제거.
- 치명타 선판정 후 lethal이면 HitReact 이벤트 스킵.
- 단일 판정(`IsExecutionDeath()`) + `EnterRagdoll()` 멱등 헬퍼 + 백스톱 타이머.

**배운 점** — 같은 증상 재발 시 보는 곳: **랙돌이 꺾이면 코드가 아니라 PhysicsAsset/콜리전/제약**. 사망 타이밍은 로그로 "완전 동기"임을 증명(지연 사망 가설은 오진). 값진 실패(비물리 사망 시도 전부 되돌림)가 정답(물리 기반 랙돌)을 확정시킴. → [[ragdoll-death-montage-decision]]

---

## 3. 적이 안 쏘고 따라옴 / 카이팅 지옥 / 첫발만 쏨

**증상** — 활 잡몹이 사격을 안 하고 플레이어한테 붙어서 비빔. 또는 끝없이 도망(카이팅 지옥). 코드는 멀쩡.

**근본 원인** — 공유 BT + 데이터드리븐에서 거의 다 **거리밴드 갭 + 분기 데코 조건 미스**(데이터·BT 배선 문제). 5개 함정:
1. 토큰(동시공격 제한)을 원거리에 적용 → 토큰 못 받은 활이 reposition/chase로 빠짐.
2. chase 데코에 거리 조건 없음 → 사거리 안에서도(쿨다운 순간) chase로 빠짐.
3. 공격 엔트리 MinRange가 "공격 사각" 생성(250~1000 후보 0).
4. 백스텝을 BT 노드로 박으면 매 틱 갈팡질팡 → SelectAttack 거리밴드 엔트리로 옮겨야.
5. `AttackRange > 사격MaxRange`면 그 사이가 데드존(bCanAttack인데 후보 0).

**해결**
- 원거리는 토큰 면제(`StandoffRange>0`이면 `HasToken=true` 강제 + reposition 차단).
- chase 데코 = `target AND bCanAttack==false`.
- 거리밴드를 빈틈없이 덮고, `사격MaxRange ≥ AttackRange`로 정합.
- 백스텝/회피 = SelectAttack 거리밴드 엔트리(Min0/Max250)로 이동 + GA를 StartupAbilities에 부여.

**진단법 (재사용)** — `SelectAttack::ExecuteTask`에 `#if !UE_BUILD_SHIPPING` 임시 디버그 HUD(`GEngine->AddOnScreenDebugMessage`)로 진입 거리·엔트리별 탈락 사유(`OUTRANGE`/`NO SPEC`/`CD`)·픽 결과 출력. PIE 화면에서 "왜 공격 안 하는지" 즉시 특정(`d=134 entries=2 NO CANDIDATE`로 백스텝 엔트리 누락 특정한 실사례). .cpp만이라 Live Coding 반영.

**배운 점** — "AI가 멍청하다"의 대부분은 AI 코드가 아니라 데이터/배선 정합 문제. 화면 디버그 HUD가 BP 디버거보다 빠르다. → [[ai-eqs-crowd-eqs]]

---

## 4. 복제한 적 BP가 "피규어처럼" 움직임

**증상** — 활도적 BP를 만들었더니 PIE에서 캐릭터가 reference pose 고정인 채 위치만 이동. ABP 프리뷰는 정상(NativeUpdateAnimation 돎)인데 PIE만 안 됨. 코드·ABP·BlendSpace 다 정상.

**근본 원인** — UE5.6에서 **기존 적 BP를 Duplicate(복제)하면 메시 AnimInstance가 PIE에서 tick을 안 함**. 복제본 컨테이너 자체가 썩음. 3시간 디버깅(부모 클래스/Speed/BlendSpace 다 멀쩡) 끝에 새 BP를 처음부터 만드니 즉시 해결.

**해결** — 새 적 BP = `AKDEnemyBaseCharacter` 우클릭 → **Create Child Blueprint Class**(Duplicate 금지).

**배운 점** — "ABP 프리뷰 OK / PIE 피규어"라는 비대칭 단서가 곧 BP 컨테이너 문제. 코드가 멀쩡한데 런타임만 깨지면 에셋 컨테이너를 의심. 디버깅보다 새로 만들기가 빠른 경우가 있다. → [[duplicate-bp-breaks-anim]]

---

## 5. patrol 복귀 중 플레이어를 보며 뒷걸음

**증상** — 적이 멀어져 patrol(home)로 복귀할 때도 플레이어를 바라보며 뒷걸음질.

**근본 원인** — `BTService_FindPlayer`가 인지 중이면 **거리 무관 매 틱 `SetFocus`** → 멀리서도 플레이어를 계속 바라봄 → 이동방향(home)과 facing이 반대 = 뒷걸음.

**해결** — `KDEnemyAIController::UpdateCombatFacing` 신설. 교전거리(`max(AttackRange,StandoffRange)×1.3`) 밖이면 `ClearFocus`+이동방향 회전, 안이면 focus+yaw 추적. FindPlayer가 `SetFocus`를 `UpdateCombatFacing(Target,Dist)`로 교체. 튜닝 노브 = EngageDist ×1.3. PIE 검증 완료.

**배운 점** — "항상 타겟을 본다"는 교전 안에서만 옳다. focus 적용에 거리 게이트를 넣는 게 정석. (→ **후속: 사례 9** — 이 게이트가 다수 EQS 포위로 스케일됐을 때 충돌해 `EngagementRange`로 일원화.)

---

## 6. 처형 생존 복귀 후 stagger 모션이 노출됨

**증상** — 엘리트가 처형을 생존하고 복귀할 때, execution 직후 stagger start 모션이 잠깐 보임.

**근본 원인** — `OnStaggerRecovered`가 처형 생존 복귀 경로에서도 스태거 몽타주를 재생. 처형 복귀는 기상까지 처형 몽타주가 소유하므로 스태거 몽타주를 재생할 이유가 없음.

**해결** — 처형 경로(`Montage_IsPlaying`=false)는 `ResumeBrainFromStagger`만 호출 / 스태거 `End` 점프는 타임아웃 전용으로 분리. 내가 넣었던 `Montage_Play` 분기를 원복.

**배운 점** — 한 콜백이 두 경로(처형 복귀 / 스태거 타임아웃)를 섞으면 한쪽 연출이 새어나온다. 경로별로 책임을 갈라야.

---

## 7. 히트스탑 영구 정지 (콤보 치명적)

**증상** — 약공 콤보 중 0.12s 내 2타가 들어가면 캐릭터가 영구 정지. (게임 치명적 — 콤보로 쉽게 트리거)

**근본 원인** — 히트스탑이 `SetPlayRate(0)` + rate 캡처 방식. 정지 중 rate=0을 캡처해 재진입 시 영구 정지. 게다가 1.0 하드코딩 복원과 mismatch. attacker(플레이어)에게도 걸려 콤보로 victim보다 쉽게 트리거.

**해결** — `Montage_Pause`/`Resume`로 재구현. rate를 안 건드리니 영구정지 원천 제거 + 하드코딩 mismatch 동시 해소. (스텔라 블레이드 레퍼런스 실측 "일시정지"와 정렬.) 재진입 최악 = 정지 살짝 짧아짐(치명→무해).

**배운 점** — 상태(rate)를 캡처/복원하는 토글은 재진입 시 깨지기 쉽다. "값을 0으로 만들었다 되돌리기"보다 "Pause/Resume"처럼 상태를 안 건드리는 API가 안전. → [[hit-feedback-decoupled-from-ge]]

---

## 8. BT 런타임 로직 introspection이 막힘

**증상** — ue-mcp introspection으로 BT 그래프(RootNode/children)를 뜯으려는데 전부 막힘.

**근본 원인** — BT의 RootNode/children이 전부 protected → 외부에서 못 읽음. 코드 introspection으로 BT 런타임 분기를 추적하려던 접근 자체가 막다른 길.

**해결** — `'`키 **Gameplay Debugger**로 선회. 코드로 먼저 분기를 좁히고, 남은 분기만 PIE에서 Gameplay Debugger로 확인.

**배운 점** — 엔진이 막아둔 길을 뚫지 말고 의도된 디버그 채널을 쓴다. 도구 선택도 디버깅 실력. (`KDEnemyAINodeUtils` 헬퍼를 만들었다 YAGNI로 폐기한 판단도 함께.) → [[bp-bt-debug-gameplay-debugger]]

---

## 9. 포위전에서 토큰 없는 적이 플레이어한테 등을 돌림

**증상** — 적 3~4명 포위전(EQS_Melee 도넛)에서, 공격 토큰 없는 적이 뒷걸음(OK)한 뒤 **아예 플레이어를 안 보고 등을 돌린 채 슬롯으로 걸어갔다가** 다시 응시하며 접근하는 사이클. patrol 상태로 빠지는 것처럼 보임.

**근본 원인** — patrol 아님(타겟 valid → BT는 전투 분기 유지). 진범은 **사례 5에서 넣은 facing 게이트가 다수 포위와 안 맞음**. 거리 개념이 3개인데 아무도 정합 안 됨:
- facing 게이트 `EngageDist = max(AttackRange,Standoff)×1.3 ≈ **195**` (melee)
- EQS 포위 도넛 반경 **200~450**
- 토큰 교전거리 `EngagementRange` **700**

토큰 없는 적은 700 안에서 reposition→도넛 슬롯(>195)에 서는데, facing 게이트가 195 밖이면 `ClearFocus`+이동방향 회전으로 풀어버림 → 도넛으로 걸어가며 **등을 돌림**. 토큰 받아 195 안으로 들어오면 다시 응시 = 그 사이클.

**해결** — facing 게이트를 **토큰/포위가 쓰는 것과 같은 `EngagementRange`로 일원화** (`UpdateCombatFacing`의 `EngageDist = GetEngagementRange()`, 1줄). 교전거리 안이면 Engage(공격)·Reposition(포위 스트레이프) 모두 응시, 밖이면 Chase=이동방향. 사례 5의 단일 적 patrol-복귀는 타겟 null이라 `FindPlayer`가 먼저 early-return → 이 함수에 안 와서 회귀 없음. 빌드 green.

**검증한 것(안 한 결정)** — perception/EQS/토큰을 명시 `ECombatPosture` 상태머신으로 단일화하는 큰 리팩토링(B안)도 검토했으나 **잡몹3종+엘리트엔 과투자**로 판단: 5개 상태가 이미 암묵적으로 다 쓰여 새 capability가 아니고, 정리의 ROI는 상태가 느는 **보스 마일스톤**에 떨어짐. stagger/사망 민감 코드를 지금 건드리는 비용 회피. → 1줄로 막고 풀 리팩토링은 이연(YAGNI).

**배운 점** — **단일 케이스로 검증한 게이트가 다수로 스케일되니 정반대로 작동**. "facing 거리"와 "교전 거리"를 다른 숫자로 둔 게 화근 — 같은 envelope로 통일. 증상(patrol처럼 보임) ≠ 원인(거리 개념 불일치)이 또 반복. + 큰 리팩토링 유혹을 검증으로 거른 판단(과설계 회피)도 자산. → [[ai-eqs-crowd-eqs]]

---

## 10. 패링(가드) 홀드 중 이동이 전혀 안 됨

**증상** — 플레이어가 가드(패링)를 홀드하는 동안 이동 입력이 전혀 안 먹고 제자리 고정. "패링 GA가 이동을 막나?" 의심.

**근본 원인** — **코드 무죄**. 입력→이동→어빌리티 경로를 끝까지 따라가 게이트가 없음을 증명:
- `KDPlayerController::Handle_Move` — 패링 상태 체크 없이 무조건 `AddMovementInput`.
- `GA_Parry` — `DisableMovement`/`MaxWalkSpeed`/`StopMovement` 어디에도 호출 없음. GE 2개(Parrying·PerfectParryReady 태그) + 몽타주 재생만.
- `State.Combat.Parrying` 태그를 이동 정지에 쓰는 곳 없음.
- 진범은 **블록 몽타주가 Root Motion ON인데 상하체 분리 슬롯에 미지정** → 풀바디 루트모션으로 빠져 캡슐 이동을 루트모션이 지배(제자리 블록 루프 = 제자리 고정). `AddMovementInput`은 호출돼도 루트모션에 덮여 무시됨.

**해결** — 블록 몽타주를 상체(UpperBody) 슬롯에 지정(또는 루트모션 OFF). 슬롯 꽂으니 하체 로코모션이 살아나 가드 중 이동 정상.

**배운 점** — 몽타주 구동 GA에서 이동이 잠기면 **어빌리티 코드보다 Root Motion 모드 / 슬롯 지정을 먼저 의심**. `AddMovementInput`이 무시되는 건 대개 루트모션이 이동을 지배하기 때문. 코드 경로를 끝까지 따라 "코드 무죄"를 먼저 확정한 게 진범(애님 셋업)을 빠르게 분리시킴 — 사례 2·3·4와 같은 *증상≠원인* 패턴.

---

## 메타 — 이 사례집이 보여주는 것

1. **증상 ≠ 원인** — 패링(트레이스), 꺾임(PhysicsAsset), 멍청한 AI(데이터 배선), 피규어(BP 컨테이너) 전부 "보이는 곳"이 진범이 아니었다.
2. **데이터 흐름/콜스택 추적** — 추측 수정 대신 게이팅 구조·동기 콜스택을 끝까지 따라가 진범을 *분리*.
3. **진단 도구 자작** — 화면 디버그 HUD, 진단 로그 2줄, Gameplay Debugger 선택.
4. **값진 실패의 기록** — 비물리 사망, 헬퍼 클래스, GA 자동화 등 되돌린 시도까지 남겨 "왜 안 되는지"를 자산화.
