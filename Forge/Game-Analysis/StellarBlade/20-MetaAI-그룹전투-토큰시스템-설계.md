# 20. MetaAI 그룹 전투 — 토큰 + 포메이션 시스템 설계

> ProjectKD 다수전 AI 설계. 스텔라블레이드 MetaAI 역엔지니어링 → UE5/GAS 이식.
> 선행 자료: [[04-AI-시스템]] · [[15-BT-심층분석]] · [[16-BT-트리구조-분석]] · [[13-UE5-GAS-설계안]]
> 작성: 2026-05-28 / status: **설계 완료 — 미구현 (코드 0줄)**. 검증 2회 반영. 착수 전 선결 §12 필수.
> ⚠ 이 문서는 *구현 지침*이다. 여기 적힌 시그니처/로직은 아직 코드에 없다 — §12 선결 체크리스트를 닫아야 착수 가능.

> ⚠ **UE5 고-스테이크 사실 (구현 전 UE5.6 기준 1회 재검증)**: ①`UWorldSubsystem`은 Tick 기본 비활성 → Timer 필수. ②BTDecorator `CalculateRawConditionValue`는 폴링 안 됨 → FlowAbort 필요. ③`MoveTo`의 FVector 키는 스냅샷 → 목표 갱신 자동추적 안 함. 세 가지 모두 본 설계의 전제.

---

## 0. 목적과 포트폴리오 스토리

**문제**: 잡몹 4명을 한 플레이어에게 붙이면 전부 동시에 달려들어 "뭉친다". 부자연스럽고, 다수전이 난투극이 됨 (2026-05-28 회의 지적사항).

**해결**: 적이 각자 판단하지 않고, **그룹 관리자(코디네이터)가 공격 권한과 위치를 배분**한다. 스텔라블레이드의 MetaAI를 우리 규모로 이식.

**포폴 가치**: "상용 AAA(스텔라블레이드) AI 구조를 분석 → UE5/GAS로 역이식" 스토리. 단순 토큰 땜질이 아니라 *분석 기반 설계*임을 문서로 증명.

---

## 1. 스텔라블레이드 MetaAI 원리 (분석 요약)

[[16-BT-트리구조-분석]] §4.2 기준.

- 각 몬스터 BT는 자율 판단하지 않는다. **MetaAI**라는 상위 관리자가 그룹 전체를 보고 명령을 내림.
- 명령 전달 = `LV_MetaAction` 이펙트 부여. "지금 이 몬스터에게 행동을 요청 중"이라는 신호. *(MetaAI의 정확한 내부 인과는 역엔지니어링 추정 — 단정 금지.)*
- 몬스터 BT가 묻는 두 데코레이터 (**둘 다 bool 판정**):
  - **`IsGroupAttacker`** (82회) — "내가 지금 공격 권한 그룹에 속하나?" (true/false)
  - **`IsGroupTarget`** (102회) — "내가 현재 그룹 타겟으로 지정됐나?" (true/false). ⚠ *위치 좌표를 반환하는 게 아님 — bool. `IsGroupTarget=false`일 때의 독립 가지도 존재.*
- 상태 관리는 블랙보드가 아니라 **이펙트/태그**로 한다 ([[16-BT-트리구조-분석]] §6.1, `CheckActorEffect` 1,873회 — 최다 데코레이터).

### ⚠ 우리 구현과의 근본 차이: push vs pull

| | 스텔라블레이드 | 우리 (ProjectKD) |
|---|---|---|
| 명령 방향 | **push** — MetaAI가 이펙트로 명령을 밀어넣음 | **pull/push 혼합** — 코디네이터가 BB에 토큰 상태 push, BT는 그걸 읽음 |
| MetaAI 레이어 | 전용 MetaAI 시스템 (복잡) | `UWorldSubsystem` 하나로 **단순화** |
| 포지셔닝 | 55×55 셀 그리드 + Theta* ([[13]] A-1) | 데모는 토큰만 (포메이션은 본편 확장) |

> 우리는 스블을 1:1 복제하지 않는다. "토큰 = 동시 공격자 제한"이라는 *핵심 효과*만 가져오고, MetaAI의 무거운 레이어는 서브시스템 하나로 압축한다. `IsGroupAttacker` ≈ 우리 토큰 보유 여부. `IsGroupTarget`은 데모에서 미사용 (본편 포메이션에서 재해석).

### UE5 대응 (이미 정리된 매핑)

| SB (UE4.26) | UE5/GAS | 출처 |
|---|---|---|
| `MetaAI` 태스크 | 커스텀 AIFormation (서브시스템/컨트롤러 확장) | [[16]] §7 |
| `IsGroupAttacker` | 토큰 보유 쿼리 | 본 문서 |
| `IsGroupTarget` | 포메이션 슬롯 배정 | 본 문서 |
| `CheckActorEffect` | `HasMatchingGameplayTag` | [[16]] §7 |
| `Enemy.Role.*` | 역할 태그 (이미 정의됨) | [[13]] B-9 |
| `Enemy.Formation.Surround/Line` | 포메이션 타입 (이미 정의됨) | [[13]] L170 |

> 태그 네이밍은 이미 [[13-UE5-GAS-설계안]]에 예약돼 있음 (`Enemy.Role.Attacker/Defender/Ranged/Supporter`, `Enemy.Formation.Surround/Line`). 본 설계는 그 위에 구현을 채운다.

---

## 2. 우리 현재 코드 현황 (이식 출발점)

실제 코드 (`Source/Project_KD/Enemy/`) 확인 결과:

| 파일 | 현재 역할 | MetaAI 접점 |
|---|---|---|
| `KDEnemyAIController` | `OnPossess`에서 BT 실행만. **비어있음** | 그룹 등록/해제 로직 얹을 자리 ✅ |
| `BTService_FindPlayer` | 거리 재서 `TargetActor` + `bCanAttack` 플립. 0.3s 간격. *"one BT/service serves every melee type"* 주석 | 공격 권한·포메이션 위치 산출 추가 지점 ✅ |
| `BTTask_ActivateAbilityByTag` | 태그로 GA 발동, 끝까지 InProgress | 토큰 점유/반납 훅 지점 ✅ |
| `KDEnemyBaseCharacter` | ASC pawn-direct, Stagger/Execution 컴포넌트 분리, Poise 시스템, per-pawn Sight/AttackRange | 역할 태그·전투상태 노출 지점 ✅ |

**핵심**: 구조가 이미 "공유 BT + per-pawn 데이터" 패턴이라 스블 BotCommon 방식과 동일. 토큰/포메이션을 *얹기* 좋은 상태. 기존 코드 갈아엎을 필요 없음.

---

## 3. 아키텍처 개요

```
┌──────────────────────────────────────────────┐
│ UCombatCoordinatorSubsystem (UWorldSubsystem) │  ← MetaAI 본체
│  - 전투원 등록부 (TArray<FCombatant>)         │
│  - 공격 토큰 풀 (Melee N개)                    │
│  - 포메이션 슬롯 (플레이어 중심 원형)          │
└──────────────────────────────────────────────┘
      ▲ 등록/해제          │ 토큰·슬롯 배정
      │                     ▼
┌─────────────┐     ┌──────────────────────────────┐
│AIController │     │ BT (공유 1개)                 │
│ 전투진입시  │     │  Decorator_Blackboard:        │
│ 등록 +반납  │     │   bHasAttackToken (FlowAbort) │ → true 공격 / false 대기
│ 허브(C1)    │     │  (포메이션 서비스 = 본편)      │
└─────────────┘     └──────────────────────────────┘
```

**싱글플레이어·단일레벨 전제**: 타겟은 항상 길동 1명. 코디네이터는 월드에 **하나**(WorldSubsystem). 멀티 타겟·여러 그룹·멀티레벨은 본편 확장 시 `GroupID` 파라미터 + `GameInstanceSubsystem` 승격 (YAGNI — 단 공개 API에 `GroupID=NAME_None`만 미리 넣어둠).

---

## 4. 핵심 컴포넌트 설계

### 4-0. 선행 조건 (이거 안 하면 전체 묵음 실패)

| # | 선행 | 이유 |
|---|---|---|
| P1 | **`AIControllerClass` 주석 해제** (`KDEnemyBaseCharacter.cpp:46`) 또는 BP 명시 지정 | 지금 엔진 기본 `AAIController` 스폰 → 캐스팅 nullptr → 등록/해제 전부 묵음 실패 (H6) |
| P2 | **`Enemy.Role.Attacker/Ranged` GameplayTag 선언** (`KDGameplayTags.h`에 `UE_DECLARE`/`UE_DEFINE`) | 미선언 태그는 `IsValid()=false`로 묵음 실패 (H9) |
| P3 | 토큰/등록 태그는 `AI.Combat.*` 네임스페이스로 분리 (기존 `State.Combat.*`와 의미 계층 다름 → 오매칭 방지) | H9 |

### 4-1. `UCombatCoordinatorSubsystem` (신규, UWorldSubsystem)

MetaAI 본체. 전투원 등록부와 토큰 풀을 관리. **모든 토큰 변경은 이 클래스 단일 경로로만** (BT는 읽기만).

```cpp
USTRUCT()
struct FCombatant
{
    TWeakObjectPtr<AKDEnemyBaseCharacter> Enemy;   // raw 금지 — dangling 방지 (C3)
    FGameplayTagContainer Roles;                   // Enemy.Role.* (복합역할 확장 무비용)
    bool   bHasToken      = false;
    float  TokenAcquiredTime = 0.f;                // 강제 반납 타이머 기준 (C4)
};

UCLASS()
class UCombatCoordinatorSubsystem : public UWorldSubsystem
{
    // 게임 월드에만 생성 (에디터 프리뷰/썸네일 제외) — H3
    // ⚠ ShouldCreateSubsystem + IsGameWorld()는 CDO 역참조 위험 + GamePreview 오포함.
    //    UE5.6 정석 = DoesSupportWorldType 오버라이드:
    virtual bool DoesSupportWorldType(const EWorldType::Type Type) const override; // Game || PIE
    virtual void Initialize(FSubsystemCollectionBase&) override;   // SetTimer(EvalHandle, EvaluateGroup, EvalInterval, looping)
    virtual void Deinitialize() override;                          // ClearTimer + Combatants.Reset() (C3)

    // 등록/해제 — AIController가 호출. Contains 가드로 중복 방어 (H1)
    void RegisterCombatant(AKDEnemyBaseCharacter* Enemy);          // 등록 즉시 슬롯 여유 시 인라인 토큰 부여 (1:1 첫공격 0지연, H7/H8)
    void UnregisterCombatant(AKDEnemyBaseCharacter* Enemy);        // 무조건 토큰 반납 포함

    // 토큰 — 멱등(idempotent). Acquire는 EvaluateGroup 단일 경로로만 (H10)
    void ReleaseAttackToken(AKDEnemyBaseCharacter* Enemy);         // 이미 false면 no-op (C1/H10 이중반납 방어)
    bool HasAttackToken(const AKDEnemyBaseCharacter* Enemy) const;

    // 매 EvalInterval(=0.2~0.3s) Timer 콜백 (C2/H4)
    void EvaluateGroup();

private:
    static constexpr int32 MaxMeleeTokens = 2;     // 동시 근접 공격자 (D5 — 플레이 검증값)
    static constexpr float MaxTokenHoldDuration = 4.f;  // 강제 반납 상한 → 기아 방지 (C4)
    FTimerHandle EvalHandle;
    TArray<FCombatant> Combatants;
};
```

**`EvaluateGroup()` 매 틱 로직** (검증이 요구한 방어 전부 포함):
```
1. Combatants.RemoveAll(!Enemy.IsValid())                    // dangling 청소 (C3)
2. 후보 = Attacker 중 [유효 && !경직 && !처형중 && !dead]      // 부적격 제외 (H13)
3. 보유자 중 (Now - TokenAcquiredTime > MaxHold) → 강제 반납   // 기아 방지 (C4)
   단, 공격 어빌리티 실행 중인 보유자는 잠금 — 재배정 금지 (H5)
4. 보유 토큰 < MaxMeleeTokens 이면:
   - 우선순위: AttackRange*2 내 적 중 FIFO, 없으면 최근접 (D4 확정)
   - 선정자 BB.bHasAttackToken = true + TokenAcquiredTime = Now
5. 비보유자 BB.bHasAttackToken = false (push)                 // C2
```

**왜 WorldSubsystem인가**: 액터 아니라 스폰/배치 부담 없음, `GetWorld()->GetSubsystem<>()` 접근. ⚠ **단, 단일 레벨 데모 전제** — 멀티레벨 지속이 필요하면 `GameInstanceSubsystem`으로 승격 (Medium). 공개 API에 `FName GroupID=NAME_None` 기본 파라미터를 지금 넣어두면 본편 멀티그룹 확장 시 API 재작성 회피.

### 4-2. 토큰 → BT 전달 = **블랙보드 푸시** (D1 확정)

> 검증 C2: 데코레이터 직접 쿼리(`CalculateRawConditionValue`)는 폴링이 안 돼서, 토큰을 줘도 실행 중인 BT 가지가 안 바뀐다. **직접 쿼리 폐기.**

- `EvaluateGroup`이 각 적 BB에 `bHasAttackToken`(Bool) **push** (위 로직 4·5단계).
- BT 공격 가지 진입 조건 = **기본 `BTDecorator_Blackboard`** (`bHasAttackToken == true`) + **`FlowAbortMode = LowerPriority`** (Both는 하위 가지까지 abort 부작용 → 공격 Sequence엔 LowerPriority 권장).
- ⚠ **FlowAbort만으론 부족**: BT 가지가 abort돼도 GA는 ASC에서 계속 실행(몽타주 끝까지). `BTTask_ActivateAbilityByTag`에 **`AbortTask` 오버라이드** 추가 → `ASC->CancelAbilitiesWithTags(ActivationTag)` 호출. 그래야 **FlowAbort → AbortTask → GA Cancel → EndAbility → 반납** 체인이 닫힘. TickTask의 IsActive 폴링은 독립 유지.
- 직접 쿼리 방식은 본편 최적화로 보류.

### 4-3. 공격 시퀀스 게이트 구조 (H2 — 토큰과 사거리 분리)

> bCanAttack과 토큰을 단순 AND/OR로 묶으면 오동작. 역할을 분리한다.

```
[BTDecorator_Blackboard: bHasAttackToken == true]   ← 토큰 = "교전 진입 자격"
   └ Sequence
       ├ MoveTo(TargetActor)                         ← 접근
       ├ [Decorator: bCanAttack == true]             ← 사거리 = "실행 직전 체크"
       └ BTTask_ActivateAbilityByTag(공격)
```
토큰 = 진입 자격(코디네이터 관할), `bCanAttack` = 사거리 게이트(FindPlayer 관할). 둘은 다른 질문.

### 4-4. `AKDEnemyAIController` — 토큰 반납 단일 진입점 (C1 핵심)

> 검증 C1: 반납 경로가 코드 어디에도 안 붙어있음(13패스 지적). AIController를 **반납 허브**로 만든다.

```cpp
virtual void OnPossess(APawn* InPawn) override;
virtual void OnUnPossess() override;

// OnPossess에서 구독 (반납 트리거 3종):
//   Enemy->OnDeath.AddDynamic(this, &::HandleReturnToken)            // 사망 (E1)
//   Enemy->StaggerComp->OnStaggerBegin.AddDynamic(.., HandleReturn)  // 경직 (E5)
//   Enemy->ExecutionComp->OnExecutionBegin.AddDynamic(.., HandleReturn) // 처형 (E4)
// 핸들러 → Coordinator->ReleaseAttackToken(Pawn)  (멱등이라 중복 안전)
// OnUnPossess → RemoveDynamic 전부 + 무조건 ReleaseAttackToken + Unregister
```

- **등록 타이밍**: `OnPossess` 즉시가 아니라 **전투 진입**(플레이어 발각) 시 — §6 H1 참조. 스폰만 되고 미발각인 적은 그룹 제외.
- **어빌리티 종료 반납**은 BTTask 폴링이 아니라 **`GA::EndAbility` 오버라이드 단일 지점**에서 (C1). BTTask는 토큰을 모름.

### 4-5. `AKDEnemyBaseCharacter` 확장

```cpp
UPROPERTY(EditAnywhere, Category="Enemy|AI")
FGameplayTagContainer CombatRoles;   // Enemy.Role.Attacker / Ranged (BP child 지정, 복합 확장 대비 Container)
```

- 역할로 코디네이터가 멜리/원거리 풀 구분. 장검·도끼 = `Attacker`, 활잡이 = `Ranged`.
- ⚠ StaggerComp/ExecutionComp는 protected → 서브시스템 직접 접근 불가. **public BlueprintPure getter 신설 필수**: `bool IsStaggered() const`, `bool IsBeingExecuted() const` (IsDead 패턴). 없으면 후보 필터 컴파일 에러 (H13).

### 4-6. 포메이션 — **데모 제외, 본편 확장** (C5 권고 채택)

> 검증 C5: 원형 슬롯 수식·반지름·NavMesh 투영·MoveTo 스냅샷 문제가 전부 미정의. 그리고 **토큰만으로 "뭉치지 않음"은 충분히 시연된다.**

- 데모: 토큰 미보유 적은 **현 위치 부근에서 대기/서성**(기존 Idle 가지 재사용). 포위 안 함.
- 본편: `BTService_FormationMove` + 원형 슬롯(`θ_i=2π·i/N`, `R=max(R_min, AttackRange)`) + `ProjectPointToNavigation` + sticky-slot 재할당 + FVector 스냅샷 회피(커스텀 MoveTo). `IsGroupTarget` 재해석.

---

## 5. GameplayTag 정의

```
Enemy.Role.Attacker        # 근접 (장검·도끼) — 멜리 토큰 풀.  ⚠ KDGameplayTags.h에 선언 필수 (P2)
Enemy.Role.Ranged          # 원거리 (활잡이) — 토큰 풀 분리.   ⚠ 선언 필수 (P2)
AI.Combat.Engaged          # 전투 진입 (등록됨) — State.Combat.*과 분리 (P3/H9)
AI.Combat.HasToken         # 토큰 보유 (디버그/GameplayCue용, 선택)
# Enemy.Formation.*        # 본편 확장으로 이동 (§4-6) — 데모 미사용
```

[[13]] 예약 `Enemy.Role.*` 재사용(단 **선언 추가 필요**) + 전투 상태는 `AI.Combat.*` 신규 네임스페이스. ⚠ `State.Combat.*`에 섞지 말 것 — 기존 전투 쿼리에 오매칭(H9).

---

## 6. 데이터 흐름 (시퀀스)

```
1. 적 스폰 → AIController가 BT 실행 (기존 그대로, 미등록 상태)
2. FindPlayer.TickNode에서 TargetActor가 null→valid 전환되는 엣지 (= 최초 발각):
   → AIController.RegisterCombatant(self)   [Contains 가드로 1회만, H1]
   → 등록 즉시 슬롯 여유 시 인라인 토큰 부여 → 1:1 첫 공격 0지연 (H7/H8)
3. Timer(EvalInterval)마다 Coordinator.EvaluateGroup() — §4-1 5단계 로직:
   유효성 청소 → 부적격 제외(경직/처형/dead) → 강제반납(MaxHold) → FIFO/최근접 부여 → BB push
4. 적 BT 분기 (BB.bHasAttackToken 읽음, FlowAbort=Both):
   a. true  → [접근 → bCanAttack 사거리체크 → 공격 어빌리티]  (§4-3)
   b. false → 현위치 대기/서성 (기존 Idle 가지, 포위 없음 §4-6)
   * 토큰이 중간에 빠지면 FlowAbort로 a→b 자동 전환
5. 토큰 반납 (멱등 ReleaseAttackToken, C1) — 트리거 4종:
   - 어빌리티 종료 → GA::EndAbility 오버라이드
   - 사망 → OnDeath 구독 (AIController 허브)
   - 경직 → OnStaggerBegin 구독
   - 처형 → OnExecutionBegin 구독
6. 적 사망/어그로상실/언포제스 → Unregister + 무조건 반납 (E1/E3)
```

---

## 7. 엣지케이스 (검증 집중 대상)

설계 견고함은 여기서 갈린다. 검증 에이전트가 집중 공격할 지점.

| # | 상황 | 처리 | 배선 위치 |
|---|---|---|---|
| E1 | **토큰 보유자 사망** | `OnDeath` 구독 → 즉시 반납 | §4-4 허브 |
| E2 | **토큰 기아** | `MaxTokenHoldDuration` 강제 반납(어빌리티 중 잠금) | §4-1 3단계 |
| E3 | **플레이어 도망** | 거리 초과 시 반납. BB ClearValue를 AIController가 관찰 → Unregister | §6-6 |
| E4 | **처형 중** | `OnExecutionBegin` 구독 → 반납. 타 적 공격 허용 = D3 보류 | §4-4 |
| E5 | **경직 중** | `OnStaggerBegin` 구독 → 반납 + EvaluateGroup 후보 제외 | §4-4 / §4-1 2단계 |
| E6 | **전 적 토큰 미보유** | 정상 — 전원 대기 = 의도된 소강 리듬 | — |
| E7 | **단일 적** | 등록 즉시 인라인 부여 → 기존 1:1 0지연 유지 | §4-1 Register |
| E8 | **(포메이션) 슬롯<적** | 데모 미적용 (포메이션 본편 이동 §4-6) | 본편 |
| E9 | **코디네이터 null** | fallback: 토큰 있다고 간주 → 기존 단순 추적. 기능 죽어도 게임 굴러감 | §4-2 데코 기본값 |
| E10 | **Subsystem 생명주기** | `Deinitialize`서 ClearTimer+Reset, `TWeakObjectPtr` **필수**, 진입 시 IsValid 청소 | §4-1 |
| E11 | **경직 복귀 직후** | `OnStaggerRecovered` → EvaluateGroup 즉시 트리거(재획득 창 닫기) | §4-1 |
| E12 | **TOCTOU/이중 반납** | Release 멱등 + Acquire는 EvaluateGroup 단일경로 + 타이머 acquire시작/release정리 | §4-1 (H10) |

---

## 8. 데모 범위 vs 본편 확장 (YAGNI)

| 항목 | 데모 | 본편 확장 |
|---|---|---|
| 코디네이터 | WorldSubsystem 1개 (타겟=길동 단일) | 그룹 ID 분리, 멀티 타겟 |
| 토큰 | 멜리 2 + 사격 1 (고정) | 난이도/구간별 가변, 역할별 세분 |
| 포메이션 | **없음** (토큰 미보유 = 현위치 대기) | Surround·Line·측면우회 |
| 슬롯 산출 | — | 원형 분할 + EQS 지형 ([[13]] A-1) |
| 협공 | 없음 (토큰만) | 동시 좌우 협공, 콤보 연계 |

**데모 1차 목표**: E1·E2·E7·E9·E10·E12 — 토큰 누수 없고(반납 배선), 기아 없고, 1:1 안 깨지고, 코디네이터 죽어도 굴러가고, PIE 종료 크래시 없고, 이중반납 카운터 오염 없음. **포메이션은 빼고 "동시 2명 제한"만 확실히 시연.**

---

## 9. 구현 단계 (잡몹 BT 작업에 녹이기)

별도 작업이 아니라 잡몹 BT 3종 만드는 흐름 안에 포함.

```
0. 선행 (§4-0): AIControllerClass 주석 해제 + Enemy.Role.* 태그 선언
   verify: 스폰된 적의 컨트롤러가 AKDEnemyAIController로 캐스팅됨 (로그)
1. 장검(1번째):
   - UCombatCoordinatorSubsystem (ShouldCreateSubsystem/Init Timer/Deinit/EvaluateGroup)
   - 반납 허브 (AIController가 OnDeath/Stagger/Execution 구독) + GA::EndAbility 반납
   - 공유 BT에 [bHasAttackToken Blackboard 분기 + FlowAbort=Both]
   - verify: 장검 4명 → 동시 2명만 공격, 나머지 대기 / 보유자 죽여도 그룹 안 멈춤(C1) / 1:1은 0지연(E7)
2. 활잡이(2번째):
   - CombatRole=Ranged → 멜리 토큰 풀에서 제외 (포메이션 N에 미포함, H11)
   - 거리 유지는 별도 로직(EQS) — 멜리 토큰과 독립
   - verify: 활잡이는 멜리 2명 제한과 무관하게 후방 사격
3. 도끼(3번째):
   - Attacker 재사용 (BT·코디네이터 무수정, 프로파일만)
   - 토큰 미보유 시 "압박거리 유지" 분기 → "멈추지 않는" 성격 (Medium)
   - verify: 프로파일 교체만으로 동작 = 재사용 패턴 증명
4. 포메이션: 데모 제외 (§4-6). 본편 확장.
```

각 단계 끝 verify 게이트. 1번 verify 3종(C1·E7·다수제한)이 데모 1차 목표. [[13]] B-9 "공유 BT + DataAsset 교체" 철학과 일치.

---

## 10. 결정 확정 (검증 20패스 반영)

- **D1 ✅ 확정**: **블랙보드 푸시** (직접 쿼리 폐기 — C2). FlowAbortMode=Both로 재평가.
- **D2 ✅ 확정**: **Timer** (Tick 폐기 — UWorldSubsystem Tick 기본 비활성). `EvalInterval` 0.2~0.3s.
- **D3 🟡 보류**: E4 처형 중 타 적 공격 허용 — 게임필 결정. 플레이 테스트로.
- **D4 ✅ 확정**: **AttackRange*2 내 FIFO, 없으면 최근접**. + 어빌리티 실행 중 보유자 재배정 금지 잠금 (H5).
- **D5 🟡 플레이검증**: 멜리 토큰 2개 적정값 — 밸런싱 영역.
- **D6 🟡 신규**: 활잡이(Ranged) 토큰 점유 단위 — 1발 vs 1사이클. 사격 토큰 목적을 "위치 겹침 방지"로 재정의하거나 EQS 이관(본편, H11).

---

---

## 11. 검증 이력

- **v1 → v2 (2026-05-28)**: 검증 20패스(렌즈별 1패스 + 종합) 반영. critical 5 + high 13 수정.
  - C1 토큰 반납 배선(13패스 지적) → §4-4 AIController 반납 허브 + GA::EndAbility.
  - C2 BT 재평가 → §4-2 BB 푸시 + FlowAbort=Both (직접 쿼리 폐기).
  - C3 Subsystem 생명주기 → §4-1 Deinitialize/Timer/TWeakObjectPtr/IsValid 청소.
  - C4 강제 쿨다운 → §4-1 MaxTokenHoldDuration.
  - C5 포메이션 → §4-6 데모 제외, 본편 이동.
  - 선행조건 P1(AIControllerClass)·P2(태그선언)·P3(네임스페이스) 신설.
- **v2 → v2.1 (2026-05-28)**: 회귀 재검증 20패스. **핵심 판정 = v2는 문서만 갱신, 코드 0줄 → status 거짓 정정.** + 코드화 시 깨지는 신규결함 반영:
  - FlowAbort=Both만으론 GA 안 끊김 → `AbortTask`에서 `CancelAbilitiesWithTags` (§4-2).
  - `ShouldCreateSubsystem/IsGameWorld` → `DoesSupportWorldType` (§4-1).
  - `IsStaggered()/IsBeingExecuted()` protected 접근불가 → public getter (§4-5).
  - 나머지(잠금 교착·BB push 경로·FIFO 자료구조·인라인부여 모순·Register 깜빡임)는 §12 선결로 이관.

---

## 12. 구현 착수 전 선결 체크리스트 (회귀검증 도출 — 전부 닫아야 착수)

**기존 코드 선결 (설계 아님 — 실제 손봐야 할 것):**
- [ ] `KDEnemyBaseCharacter.cpp:13` include + `:46` AIControllerClass 주석 **동시 해제** + BP child 오버라이드 전수 확인 (BP값이 C++ CDO 이김). verify: 스폰 적 `GetController<AKDEnemyAIController>() != nullptr` (P1, 최우선)
- [ ] `KDGameplayTags.h`에 `Enemy.Role.Attacker/Ranged` + `AI.Combat.Engaged/HasToken` 선언/정의 (P2/P3)
- [ ] `KDEnemyBaseCharacter.h`에 `CombatRoles` UPROPERTY + `IsStaggered()/IsBeingExecuted()` public getter

**설계 확정 (코드 작성 전 결정):**
- [ ] D6 — Ranged(활잡이) 토큰 경로: 별도 풀 vs 토큰 우회 자율사격 (멜리 풀 제외만 하면 활잡이 공격분기 영구 false)
- [ ] 어빌리티 잠금 = `bAbilityInFlight` 플래그(단방향 푸시) + `MaxAbilityLockDuration` 상한 (EndAbility 미호출 시 영구교착 방지)
- [ ] 5단계 false push는 `bAbilityInFlight` 보유자 skip (실행 중 GA 중간중단 방지)
- [ ] FIFO = `EnqueueTime` 필드(Register 시 기록) + 타이브레이커 체인(사거리→EnqueueTime→거리)
- [ ] Register 인라인 부여 vs Acquire 단일경로 — 택1. 인라인 시 `TokenAcquiredTime=Now` 동시설정 예외 주석
- [ ] BB push 경로 = Register 시 `TWeakObjectPtr<UBlackboardComponent>` 캐시 (매틱 캐스팅 회피)
- [ ] TargetActor 경계 깜빡임 → 히스테리시스(이탈 후 1~2s 유예) + Unregister Contains 가드

**신규 생성:**
- [ ] `UCombatCoordinatorSubsystem.h/.cpp` (Register/Unregister/Release 멱등/EvaluateGroup/Deinitialize)
- [ ] `AKDEnemyAIController` OnUnPossess + OnPossess 3종 AddDynamic(`&AKDEnemyAIController::HandleReturnToken` — 클래스 스코프 명시) + RemoveDynamic
- [ ] 공격 GA 베이스 `EndAbility` 오버라이드에서 반납(ReleaseAttackToken 먼저 → Super 순서)

> status: **미구현.** 위 체크리스트 닫기 = 구현 1단계. 장검 verify 3종(동시≤2 / 보유자사망후 ≤0.3s 재배정 / 1:1 0지연 — 수치 측정) = 데모 게이트.
> ⚠ 검증은 여기서 종료. 두 번 돌렸고, 2회차 결론이 "더 검증 말고 코드 짜라"임. 다음은 구현.
