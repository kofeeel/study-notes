---
tags: [sts2, godot, quiz, review, zombchelin]
---

# Godot Study 종합 문제집 (00~20)

[[00-INDEX]] | STS2 소스코드 기반 Godot 4 + C# 학습 점검

> 총 100문제. 객관식 / 단답형 / 서술형 / 코드 분석 / 좀슐랭 적용 문제로 구성.
> 각 섹션 끝에 정답이 있다.

---

## A. Godot 엔진 기초 (01~02)

### A1. 객관식

**Q01.** Godot에서 C# 노드 클래스에 반드시 붙여야 하는 키워드는?
- (a) `abstract`
- (b) `sealed`
- (c) `partial`
- (d) `static`

**Q02.** `project.godot`의 `[autoload]` 섹션에서 노드 이름 앞에 `*`가 붙으면 어떤 의미인가?
- (a) 디버그 빌드에서만 로드된다
- (b) 게임 시작 시 자동으로 씬 트리에 추가된다
- (c) 에디터에서만 로드된다
- (d) 수동으로 로드해야 한다

**Q03.** STS2가 물리 엔진을 `"Dummy"`로 설정한 이유는?
- (a) 성능 최적화를 위해 커스텀 물리를 사용하므로
- (b) 카드 게임이라 물리 시뮬레이션이 불필요하므로
- (c) 멀티플레이어 동기화 문제 때문에
- (d) Godot 기본 물리가 불안정해서

### A2. 단답형

**Q04.** Godot Node의 라이프사이클 메서드 4가지를 호출 순서대로 나열하시오.

**Q05.** STS2에서 `_Process()`를 거의 사용하지 않는 대신 어떤 패턴으로 게임 로직을 처리하는가?

**Q06.** STS2의 C# 코드에서 싱글톤에 접근하는 전형적인 패턴을 코드로 작성하시오. (예: `RunManager`)

### A3. 서술형

**Q07.** STS2가 GDScript 대신 C#을 선택한 기술적 이유를 3가지 이상 서술하시오.

**Q08.** `[Export]` 어트리뷰트의 역할과, STS2가 게임 데이터(카드, 유물)를 `.tres` 대신 C# 클래스 인스턴스로 관리하는 이유를 설명하시오.

---

### A. 정답

> [!faq]- A 정답 펼치기
> **Q01.** (c) `partial` — Godot 소스 제너레이터가 나머지 코드를 자동 생성하기 때문
> **Q02.** (b) 게임 시작 시 자동으로 씬 트리에 추가된다 (없으면 수동 로드)
> **Q03.** (b) 카드 게임이라 물리 시뮬레이션이 불필요하므로
> **Q04.** `_EnterTree()` → `_Ready()` → `_Process(delta)` → `_ExitTree()`
> **Q05.** `async/await` 기반 GameAction 큐 + `Cmd.Wait(seconds)` 타이밍 유틸리티
> **Q06.** `RunManager.Instance.IsInProgress` / `CombatManager.Instance.CheckWinCondition()` 등 `static Instance` 프로퍼티 패턴
> **Q07.** ① 정적 타입 → 컴파일 타임 에러 감지 ② async/await 완전 지원 (Hook 시스템 핵심) ③ IDE 지원 (Rider/VS) ④ 소스 제너레이터 활용 (`[GenerateSubtypes]`) ⑤ 수만 줄 코드 관리 가능 ⑥ .NET 리플렉션 활용
> **Q08.** `[Export]`는 Inspector에 값을 노출하고 씬 파일에 직렬화하는 역할. STS2는 카드/유물 데이터를 C# `AbstractModel` 서브클래스로 관리하는데, 이는 Hook 오버라이드, async 효과 구현, Canonical/Mutable 패턴 적용이 필요하기 때문. `.tres`로는 이러한 로직 캡슐화가 불가능하다.

---

## B. 아키텍처 & Model 시스템 (03~05)

### B1. 객관식

**Q09.** STS2의 3레이어 아키텍처에서 **Logic Layer**에 해당하는 것은?
- (a) Godot Nodes, 씬 파일, UI
- (b) GameAction, Hook, Cmd
- (c) AbstractModel, CombatState, RunState
- (d) SaveManager, Settings

**Q10.** `AbstractModel.MutableClone()`이 내부적으로 사용하는 .NET 메서드는?
- (a) `Activator.CreateInstance()`
- (b) `Object.Clone()`
- (c) `MemberwiseClone()`
- (d) `BinaryFormatter.Deserialize()`

**Q11.** `ModelId`의 형식으로 올바른 것은?
- (a) `CardModel.StrikeIronclad`
- (b) `card.strike-ironclad`
- (c) `Card/StrikeIronclad`
- (d) `CARD_STRIKE_IRONCLAD`

### B2. 단답형

**Q12.** Canonical 모델을 직접 수정하려 하면 어떤 예외가 발생하는가?

**Q13.** `ModelDb.Init()`에서 모든 `AbstractModel` 서브타입을 인스턴스화할 때 사용하는 .NET API를 작성하시오.

**Q14.** Hook 리스너의 순회 순서를 올바르게 나열하시오: 파워, 렐릭, 포션, 카드, 몬스터, Modifier

### B3. 코드 분석

**Q15.** 다음 코드에서 `ModifyDamageAdditive`의 기본 반환값이 `0m`이고, `ModifyDamageMultiplicative`의 기본 반환값이 `1m`인 이유를 설명하시오.

```csharp
public virtual decimal ModifyDamageAdditive(...) => 0m;
public virtual decimal ModifyDamageMultiplicative(...) => 1m;
```

**Q16.** `Should*` 메서드의 기본 반환값이 대부분 `true`인 이유와, `ShouldTakeExtraTurn`의 기본값이 `false`인 이유를 설명하시오.

### B4. 서술형

**Q17.** Canonical/Mutable 상태 분리 패턴의 장점을 안전성, 멀티플레이어, 메모리 효율 관점에서 설명하시오.

**Q18.** STS2에서 사용하는 디자인 패턴 6가지를 나열하고, 각각 어떤 클래스/시스템이 해당 패턴을 구현하는지 매핑하시오.

---

### B. 정답

> [!faq]- B 정답 펼치기
> **Q09.** (b) GameAction, Hook, Cmd
> **Q10.** (c) `MemberwiseClone()` — 얕은 복사 후 `DeepCloneFields()`로 깊은 복사 보완
> **Q11.** (b) `card.strike-ironclad` — Category(베이스 클래스 슬러그) + "." + Entry(구체 클래스 슬러그)
> **Q12.** `CanonicalModelException`
> **Q13.** `Activator.CreateInstance(type)` — 타입 정보로 인스턴스 동적 생성
> **Q14.** 크리처 파워 → (플레이어면) 렐릭 → 포션 → 오브 → 카드+Affliction+Enchantment → (몬스터면) MonsterModel → Modifier → MultiplayerScalingModel
> **Q15.** Additive는 **더하기** 연산이므로 기본값 0(영향 없음). Multiplicative는 **곱하기** 연산이므로 기본값 1(영향 없음). 이렇게 해야 오버라이드하지 않은 모델이 값을 변경하지 않는다.
> **Q16.** 기본 `true`는 "화이트리스트 방식" — 기본은 허용, 특수 케이스만 `false`로 막는다. `ShouldTakeExtraTurn`은 예외적 상황(추가 턴)이므로 기본 `false`가 올바르다. 기본값이 `true`면 모든 모델이 추가 턴을 제공하게 된다.
> **Q17.** ① **안전성**: Canonical 원본이 실수로 수정되는 것을 예외로 방지 ② **멀티플레이어**: 모든 플레이어가 같은 Canonical을 공유, 차이점(Mutable)만 동기화하면 됨 ③ **메모리**: 공통 정의 데이터는 Canonical 하나만 유지, Mutable은 런타임에 필요한 것만 복제
> **Q18.** ① Singleton: `RunManager.Instance` ② Command: `GameAction` ③ Observer: `Hook` + `AbstractModel` ④ State Machine: `GameActionState` 열거형 ⑤ Factory: `ModelDb` + `MutableClone()` ⑥ Template Method: `AbstractModel`의 virtual 메서드

---

## C. 초기화 & 전투 시스템 (04, 06)

### C1. 객관식

**Q19.** STS2의 2단계 초기화에서 **Essential 단계**에 포함되지 않는 것은?
- (a) `ModelDb.Init()`
- (b) `LocManager.Initialize()`
- (c) `PrewarmJit()`
- (d) `SaveManager.Instance.InitSettingsData()`

**Q20.** `CombatState`의 `RoundNumber`와 `CurrentSide`의 초기값은?
- (a) `RoundNumber = 0`, `CurrentSide = CombatSide.None`
- (b) `RoundNumber = 1`, `CurrentSide = CombatSide.Player`
- (c) `RoundNumber = 1`, `CurrentSide = CombatSide.Enemy`
- (d) `RoundNumber = 0`, `CurrentSide = CombatSide.Player`

**Q21.** `NAssetLoader`가 유휴 상태일 때 CPU 낭비를 방지하는 방법은?
- (a) `QueueFree()` 호출
- (b) `SetProcess(false)` 호출
- (c) `GD.Print("idle")` 출력
- (d) Thread.Sleep() 호출

### C2. 단답형

**Q22.** `PrewarmJit()`가 하는 일을 한 줄로 설명하시오.

**Q23.** `CombatManager`에서 전투 종료 조건(`IsEnding`)이 `true`가 되는 두 가지 조건을 서술하시오.

**Q24.** `CombatHistory`에 기록되는 엔트리 타입 5가지를 나열하시오.

### C3. 흐름 분석

**Q25.** 카드 플레이의 전체 흐름을 12단계 중 핵심 6단계로 요약하시오. (UI → 최종 상태 업데이트까지)

**Q26.** 전투 턴 루프에서 다음 순서를 올바르게 정렬하시오:
`BeforePlayPhaseStart`, `AfterBlockCleared`, `BeforeSideTurnStart`, `SetupPlayerTurn`, `AfterSideTurnStart`

---

### C. 정답

> [!faq]- C 정답 펼치기
> **Q19.** (c) `PrewarmJit()` — Deferred 단계에서 실행됨
> **Q20.** (b) `RoundNumber = 1`, `CurrentSide = CombatSide.Player` — 플레이어 선공
> **Q21.** (b) `SetProcess(false)` — 큐가 비면 `_Process` 비활성화
> **Q22.** `RuntimeHelpers.PrepareMethod()`로 `IPacketSerializable`의 모든 Serialize/Deserialize 메서드를 강제 JIT 컴파일하여 첫 프레임 히치를 방지한다.
> **Q23.** ① 살아있는 `IsPrimaryEnemy`가 없을 때 ② `ShouldStopCombatFromEnding()` 훅이 `false`를 반환하는 모델이 없을 때
> **Q24.** `CardPlayStartedEntry`, `DamageReceivedEntry`, `BlockGainedEntry`, `CardDrawnEntry`, `MonsterPerformedMoveEntry` (외 `EnergySpentEntry`, `PowerReceivedEntry`, `PotionUsedEntry` 등)
> **Q25.** ① 플레이어가 카드 드래그 → ② `PlayCardAction` 생성 & 큐 추가 → ③ 유효성 검사 (핸드, 타겟) → ④ 에너지 소비 + `Hook.BeforeCardPlayed` → ⑤ 카드 효과 실행 (데미지/블록 + ModifyDamage 훅) → ⑥ `Hook.AfterCardPlayed` + UI 업데이트
> **Q26.** `BeforeSideTurnStart` → `AfterBlockCleared` → `SetupPlayerTurn` → `AfterSideTurnStart` → `BeforePlayPhaseStart`

---

## D. 카드 & 엔티티 시스템 (07~08)

### D1. 객관식

**Q27.** `CardModel`에서 업그레이드 가능 여부를 판단하는 조건은?
- (a) `CurrentUpgradeLevel < MaxUpgradeLevel`
- (b) `IsUpgraded == false`
- (c) `Rarity != CardRarity.Basic`
- (d) `Type == CardType.Power`

**Q28.** `Creature` 래퍼에서 블록 상한값은?
- (a) 100
- (b) 255
- (c) 999
- (d) 무제한

**Q29.** `Player` 클래스의 생성자 접근 제한자는?
- (a) `public`
- (b) `protected`
- (c) `internal`
- (d) `private` (팩토리 메서드 사용)

### D2. 단답형

**Q30.** `DynamicVar` 시스템에서 `DamageVar(6m)`을 선언하고 업그레이드 시 +3하는 코드를 작성하시오.

**Q31.** 카드 파일(Pile) 4종류와 그 사이의 카드 이동 흐름을 화살표로 표현하시오.

**Q32.** `PlayerCombatState`에서 "Stars"(별)의 역할을 설명하시오.

### D3. 코드 분석

**Q33.** 다음 `StrikeIronclad`의 `OnPlay` 메서드에서 `ArgumentNullException.ThrowIfNull`을 호출하는 이유는?

```csharp
protected override async Task OnPlay(PlayerChoiceContext ctx, CardPlay cardPlay)
{
    ArgumentNullException.ThrowIfNull(cardPlay.Target, "cardPlay.Target");
    await DamageCmd.Attack(DynamicVars.Damage.BaseValue)
        .FromCard(this).Targeting(cardPlay.Target).Execute(ctx);
}
```

**Q34.** `Creature`가 `Player`와 `Monster`를 모두 감싸는 래퍼 패턴을 사용하는 이점을 설명하시오.

---

### D. 정답

> [!faq]- D 정답 펼치기
> **Q27.** (a) `CurrentUpgradeLevel < MaxUpgradeLevel` — `IsUpgradable` 프로퍼티가 이 조건을 확인
> **Q28.** (c) 999 — `GainBlockInternal`에서 `Math.Min(Block + amount, 999)` 제한
> **Q29.** (d) `private` — `CreateForNewRun()` 등 팩토리 메서드로만 생성 가능
> **Q30.** 선언: `new DamageVar(6m, ValueProp.Move)` / 업그레이드: `DynamicVars.Damage.UpgradeValueBy(3m);`
> **Q31.** `DrawPile` →(드로우)→ `Hand` →(플레이/버리기)→ `DiscardPile` →(섞기)→ `DrawPile` / `Hand` →(소멸)→ `ExhaustPile`
> **Q32.** Stars는 STS2 신규 보조 자원으로, 일부 카드가 에너지 대신 또는 추가로 소비한다. 에너지 부족분을 Stars×2로 대체할 수도 있다 (`ShouldPayExcessEnergyCostWithStars` 훅).
> **Q33.** `StrikeIronclad`는 `TargetType.AnyEnemy`(적 단일 타겟)이므로 타겟이 반드시 존재해야 한다. `null`이면 플레이 로직의 버그이므로 빠르게 실패(fail-fast)시키기 위해 검증한다.
> **Q34.** 전투 로직이 "Player인지 Monster인지" 분기하지 않고 `Creature` 단일 인터페이스로 데미지, 블록, HP, 파워 등을 동일하게 처리할 수 있다. 코드 중복 제거와 확장성(Pet, Secondary Enemy 등) 확보에 유리하다.

---

## E. 커맨드 & 훅 시스템 (09~10)

### E1. 객관식

**Q35.** `Cmd.CustomScaledWait`에서 `FastModeType.Instant`일 때의 동작은?
- (a) 0.01초 대기
- (b) `fastSeconds` 값 사용
- (c) 즉시 반환, 대기 없음
- (d) `standardSeconds`의 절반 사용

**Q36.** `DamageCmd.Attack(damage)` 호출 시 반환되는 객체의 타입은?
- (a) `Task<DamageResult>`
- (b) `AttackCommand` (빌더 패턴)
- (c) `GameAction`
- (d) `decimal`

**Q37.** Hook의 `Modify*` 패턴에서 실행 순서는?
- (a) Multiplicative → Additive
- (b) Additive → Multiplicative
- (c) 랜덤
- (d) 등록 순서대로

### E2. 단답형

**Q38.** `AttackCommand`의 플루언트 빌더 체인을 사용하여 "데미지 10, 카드에서 발사, 적 단일 타겟, 슬래시 VFX"인 공격 코드를 작성하시오.

**Q39.** `Hook.ShouldDie()`가 `false`를 반환하면 어떤 일이 일어나는지 설명하시오. (`preventer` 매개변수 포함)

**Q40.** `InvokeExecutionFinished()`가 매 훅 호출 후 반드시 실행되는 이유를 설명하시오.

### E3. 코드 분석

**Q41.** `CreatureCmd.Damage`의 전체 파이프라인에서 다음 단계의 올바른 순서를 정렬하시오:
`AfterDamageReceived`, `ModifyDamage`, `BeforeDamageReceived`, `LoseHpInternal`, `DamageBlockInternal`, `Kill`

**Q42.** `AttackContext`가 `IAsyncDisposable`을 구현하여 `await using` 구문을 사용하는 이유를 설명하시오.

### E4. 서술형

**Q43.** Hook의 3가지 유형(Before/After, Modify, Should)의 차이점을 반환 타입, 용도, 예시와 함께 설명하시오.

**Q44.** Late / Early / VeryEarly 변형 훅이 존재하는 이유와, `BeforeTurnEnd`의 3단계 실행 순서를 서술하시오.

---

### E. 정답

> [!faq]- E 정답 펼치기
> **Q35.** (c) 즉시 반환, 대기 없음 — `case FastModeType.Instant: break;`
> **Q36.** (b) `AttackCommand` — 빌더 패턴으로 `.FromCard()`, `.Targeting()`, `.Execute()` 등을 체이닝
> **Q37.** (b) Additive → Multiplicative — 먼저 더하고 나중에 곱한다
> **Q38.**
> ```csharp
> await DamageCmd.Attack(10)
>     .FromCard(this)
>     .Targeting(target)
>     .WithHitFx("vfx/vfx_attack_slash")
>     .Execute(choiceContext);
> ```
> **Q39.** 사망이 방지되고 `preventer`(out 매개변수)에 사망을 막은 모델이 담긴다. 이후 `Hook.AfterDeath(..., wasRemovalPrevented: true)`와 `Hook.AfterPreventingDeath(preventer, creature)`가 호출된다. 만약 그 후에도 HP가 0이면 재귀적으로 Kill을 재시도한다.
> **Q40.** 멀티플레이어 동기화 시스템(`CombatManager.StateTracker`)에 "이 모델의 실행이 끝났음"을 알리는 신호. 단일 플레이어에서도 동일하게 호출하여 코드 경로의 일관성을 유지한다.
> **Q41.** `ModifyDamage` → `BeforeDamageReceived` → `DamageBlockInternal` → `LoseHpInternal` → `AfterDamageReceived` → `Kill`
> **Q42.** 하나의 카드가 여러 개별 공격을 묶을 때, `CreateAsync`에서 `BeforeAttack` 훅을 1번 호출하고, `DisposeAsync`에서 `AfterAttack` 훅을 1번 호출한다. `await using` 구문으로 블록 끝에서 자동으로 정리되어 훅 호출 누락을 방지한다.
> **Q43.** ① **Before/After**: `async Task` 반환, 이벤트 전/후 부작용 처리 (애니메이션, 상태 변경). 예: `AfterCardPlayed` ② **Modify**: `decimal` 반환, 값 수정 후 반환. Additive(0m 기본), Multiplicative(1m 기본). 예: `ModifyDamageAdditive` ③ **Should**: `bool` 반환, 동작 허가/차단. AND 논리(하나라도 false면 차단). 예: `ShouldDie`
> **Q44.** 같은 이벤트라도 효과 적용 타이밍이 다른 경우가 있기 때문. 예: "턴 종료 시 독 데미지(Early) → 카드 버리기(기본) → 블록 초기화(Late)". `BeforeTurnEnd` 3단계: `BeforeTurnEndVeryEarly` → `BeforeTurnEndEarly` → `BeforeTurnEnd`

---

## F. 방/게임 흐름 & 런 상태 (11~12)

### F1. 객관식

**Q45.** `AbstractRoom`의 세 가지 수명주기 메서드는?
- (a) `Init`, `Update`, `Destroy`
- (b) `Enter`, `Exit`, `Resume`
- (c) `Start`, `Process`, `End`
- (d) `Load`, `Run`, `Unload`

**Q46.** 이벤트 방에서 전투가 발생할 때 사용하는 자료구조는?
- (a) Queue
- (b) Stack (방 스택)
- (c) LinkedList
- (d) Dictionary

**Q47.** `NullRunState`는 어떤 디자인 패턴의 구현인가?
- (a) Strategy 패턴
- (b) Null Object 패턴
- (c) Decorator 패턴
- (d) Adapter 패턴

### F2. 단답형

**Q48.** `RoomType` 열거형의 8가지 값을 나열하시오.

**Q49.** `RunState.CurrentActIndex`를 변경하면 자동으로 발생하는 두 가지 부작용은?

**Q50.** 세이브/로드 시 `CombatRoom`의 방 스택 복원이 지원되지 않는 이유를 설명하시오.

### F3. 서술형

**Q51.** 이벤트 방(EventRoom) → 전투(CombatRoom) → 이벤트 복귀의 전체 흐름을 방 스택 push/pop 관점에서 설명하시오.

**Q52.** `RunManager`가 `RunState`를 `private`으로 보유하고 외부에 직접 노출하지 않는 아키텍처적 이유를 설명하시오.

---

### F. 정답

> [!faq]- F 정답 펼치기
> **Q45.** (b) `Enter`, `Exit`, `Resume`
> **Q46.** (b) Stack — 전투룸이 push되고, 종료 후 pop되어 이벤트룸으로 복귀
> **Q47.** (b) Null Object 패턴 — `IRunState`를 완전히 구현하지만 조회는 안전한 기본값, 쓰기는 예외
> **Q48.** `Unassigned`, `Monster`, `Elite`, `Boss`, `Treasure`, `Shop`, `Event`, `RestSite`, `Map` (참고: 코드에는 Map이 별도로 정의됨)
> **Q49.** ① `_visitedMapCoords.Clear()` — 방문 기록 삭제 ② `ActFloor = 0` — 층 카운터 리셋
> **Q50.** 전투는 진행 중 상태가 매우 복잡(카드 파일, 파워, 크리처 상태 등)하여 중간 상태를 직렬화하지 않는다. 대신 전투가 끝난 후(보상 단계)에만 저장한다. 세이브 로드 시에는 전투를 처음부터 다시 시작한다.
> **Q51.** ① EventRoom이 방 스택 하단에 있음 → ② CombatRoom을 push (ParentEventId 포함) → ③ CombatRoom.Enter() 실행 → ④ 전투 진행 → ⑤ 전투 종료, CombatRoom pop (`ShouldResumeParentEvent = true`) → ⑥ EventRoom.Resume(exitedCombatRoom) 호출 → ⑦ EventSynchronizer.ResumeEvents()로 이벤트 계속
> **Q52.** RunState 자체는 순수 데이터이고, RunManager가 네트워크/저장/이벤트를 처리하는 오케스트레이터 역할을 한다. 직접 노출하면 외부에서 무분별하게 상태를 변경할 수 있어 캡슐화와 불변식 보장이 깨진다. `IRunState` 인터페이스를 통한 제한된 접근만 허용한다.

---

## G. 저장 시스템 & 맵 생성 (13~14)

### G1. 객관식

**Q53.** STS2의 `SaveManager`가 관리하는 하위 매니저의 개수는?
- (a) 3개
- (b) 4개
- (c) 6개
- (d) 8개

**Q54.** 멀티플레이어에서 런 저장 파일을 생성하는 주체는?
- (a) 모든 플레이어
- (b) 호스트(Host)만
- (c) 클라이언트(Client)만
- (d) 서버 사이드

**Q55.** `StandardActMap`의 기본 열(column) 개수는?
- (a) 5
- (b) 7
- (c) 9
- (d) 맵마다 다름

### G2. 단답형

**Q56.** 저장 파일 손상 시 복구에 사용하는 파일 확장자를 쓰시오.

**Q57.** `CloudSaveStore`가 사용하는 디자인 패턴의 이름과, 이 패턴의 장점을 한 줄로 설명하시오.

**Q58.** 맵 생성 시 두 경로가 X자로 교차하는 것을 방지하는 메서드의 이름은?

### G3. 서술형

**Q59.** 맵 생성의 전체 4단계(경로 생성 → 타입 배정 → 가지치기 → 후처리)를 각각 2~3줄로 설명하시오.

**Q60.** RNG Counter 직렬화 방식이 전체 난수 시퀀스 저장보다 나은 이유를 설명하시오.

---

### G. 정답

> [!faq]- G 정답 펼치기
> **Q53.** (c) 6개 — Settings, Progress, Run, RunHistory, Prefs, Profile
> **Q54.** (b) 호스트(Host)만
> **Q55.** (b) 7
> **Q56.** `.backup` — 저장 전에 이전 파일을 `.backup`으로 보관
> **Q57.** Decorator 패턴. 기존 로컬 I/O(`GodotFileIo`) 위에 클라우드 저장 기능을 감싸서 코드 변경 없이 기능을 추가한다.
> **Q58.** `HasInvalidCrossover` — 교차 감지 시 해당 방향을 건너뜀
> **Q59.** ① **경로 생성**: 7개의 독립 경로를 row 1에서 시작해 마지막 행까지 랜덤 방향(-1, 0, +1)으로 뻗으며, 교차를 방지한다 ② **타입 배정**: 마지막 행=RestSite, 1행=Monster 등 고정 후, 나머지를 큐에서 꺼내 제약 규칙(인접 중복 금지 등) 검사 후 배정 ③ **가지치기**: 동일한 타입 시퀀스를 가진 중복 경로를 감지하여 제거, 맵 다양성 확보 ④ **후처리**: 빈 열 제거(Center), 인접 노드 간격 확보(Spread), 지그재그 직선화(Straighten)
> **Q60.** ① 파일 크기가 극소(시드 문자열 + 타입별 정수 카운터)로 유지됨 ② 시드와 카운터만 있으면 `FastForwardCounter`로 동일한 내부 상태를 완벽히 재현 가능 ③ 전체 시퀀스 저장은 수만 개의 난수를 기록해야 하므로 비효율적

---

## H. RNG & 모딩 시스템 (15~16)

### H1. 객관식

**Q61.** `Rng.Chaotic`의 용도로 적절한 것은?
- (a) 카드 셔플
- (b) 몬스터 AI 행동 결정
- (c) 파티클 효과, 사운드 피치 변화
- (d) 맵 생성

**Q62.** `RunRngSet`에서 각 RNG 타입의 시드가 생성되는 방식은?
- (a) 모두 동일한 시드 사용
- (b) 순번으로 시드 증가 (seed+1, seed+2, ...)
- (c) 기본 시드 + 타입 이름 해시
- (d) 완전히 독립된 랜덤 시드

**Q63.** STS2의 모딩 시스템에서 `ModInitializerAttribute`가 없을 때 자동으로 실행되는 것은?
- (a) `Main()` 메서드 호출
- (b) `Harmony.PatchAll(assembly)` 자동 실행
- (c) 모드 로딩 건너뜀
- (d) 에러 발생

### H2. 단답형

**Q64.** `RunRngSet`에서 용도별 RNG를 분리하는 이유를 한 문장으로 설명하시오.

**Q65.** 모드 의존성 해결에 사용하는 알고리즘의 이름은?

**Q66.** `WeightedNextItem<T>` 메서드의 동작 원리를 3줄 이내로 설명하시오.

### H3. 코드 분석

**Q67.** 다음 코드에서 `FastForwardCounter`가 `Counter > targetCount`일 때 예외를 던지는 이유를 설명하시오.

```csharp
public void FastForwardCounter(int targetCount)
{
    if (Counter > targetCount)
        throw new InvalidOperationException(...);
    while (Counter < targetCount)
    {
        Counter++;
        _random.Next();
    }
}
```

**Q68.** `ManifestJson`에서 `affects_gameplay` 필드가 `true`인 모드와 `false`인 모드의 차이를 게임 내에서 어떻게 처리하는지 설명하시오.

---

### H. 정답

> [!faq]- H 정답 펼치기
> **Q61.** (c) 파티클 효과, 사운드 피치 변화 — 게임 결과에 영향 없는 순수 비주얼 용도만
> **Q62.** (c) 기본 시드 + 타입 이름 해시 — `new Rng(Seed, "combat_targets")` 형태로 파생
> **Q63.** (b) `Harmony.PatchAll(assembly)` 자동 실행
> **Q64.** 한 시스템(전투 타겟)의 RNG 소비가 다른 시스템(몬스터 AI)의 결과에 영향을 주지 않도록 독립성을 보장하기 위해.
> **Q65.** Kahn's Algorithm (위상 정렬, in-degree 기반)
> **Q66.** ① 모든 항목의 가중치 합계를 구한다 ② 0~합계 사이 랜덤 값에서 각 항목의 가중치를 순서대로 빼간다 ③ 잔여값이 0 이하가 되는 시점의 항목을 반환한다
> **Q67.** `System.Random`의 내부 상태는 되돌릴 수 없다(단방향). 이미 더 많이 소비한 상태에서 과거로 돌아갈 방법이 없으므로 예외를 던져 프로그래밍 오류를 알린다. 복원하려면 새 Rng를 시드부터 다시 생성해야 한다.
> **Q68.** `affects_gameplay = true`인 모드는 런 메타데이터에 기록되어 리더보드 제외, 업적 비활성화 등에 사용된다. `false`인 모드(스킨, UI 변경 등)는 게임플레이에 영향을 주지 않아 리더보드와 업적에 영향 없이 사용 가능하다.

---

## I. 멀티플레이어 & UI (17~18)

### I1. 객관식

**Q69.** STS2의 멀티플레이어 모델은?
- (a) Dedicated Server
- (b) Host/Client P2P
- (c) Peer-to-Peer Mesh
- (d) Cloud Gaming

**Q70.** `LocalContext.IsMine(card)` 메서드의 역할은?
- (a) 카드가 내 덱에 있는지 확인
- (b) 카드의 소유자가 로컬 플레이어인지 확인
- (c) 카드가 플레이 가능한지 확인
- (d) 카드가 업그레이드됐는지 확인

**Q71.** `NTransition`의 화면 전환에서 `FastModeType.Instant`일 때의 동작은?
- (a) 빠른 페이드 (0.1초)
- (b) 전환 효과 없이 즉시 전환
- (c) 슬라이드 전환
- (d) 전환 스킵 불가

### I2. 단답형

**Q72.** `ActionQueueSynchronizer`에서 Client가 카드를 플레이할 때의 네트워크 흐름을 3단계로 설명하시오.

**Q73.** STS2에서 지원하는 `NetAction` 타입 5가지를 나열하시오.

**Q74.** `NModalContainer`에서 이미 모달이 열려있을 때 새 모달을 추가하면 어떻게 처리되는가?

### I3. 서술형

**Q75.** `CombatStateSynchronizer`에서 전투 시작 전 동기화하는 데이터와, 왜 Host만 RNG 시드를 브로드캐스트하는지 설명하시오.

**Q76.** STS2의 호버팁 시스템에서 `owner`가 씬 트리에서 제거될 때 자동으로 팁이 제거되는 메커니즘을 설명하시오.

---

### I. 정답

> [!faq]- I 정답 펼치기
> **Q69.** (b) Host/Client P2P — ENet 직접 연결과 Steam 릴레이 두 가지 트랜스포트 지원
> **Q70.** (b) 카드의 소유자가 로컬 플레이어인지 확인 — `IsMe(card?.Owner)` → `player?.NetId == NetId`
> **Q71.** (b) 전환 효과 없이 즉시 전환 — `InTransition = true; Visible = false; return;`
> **Q72.** ① Client가 `RequestEnqueueActionMessage`를 Host에 전송 ② Host가 유효성 검증 후 `ActionEnqueuedMessage`를 모든 플레이어에게 브로드캐스트 ③ 모든 클라이언트가 동일한 액션을 실행
> **Q73.** `NetPlayCardAction`, `NetEndPlayerTurnAction`, `NetMoveToMapCoordAction`, `NetUsePotionAction`, `NetPickRelicAction` (외 6개 더 존재)
> **Q74.** 경고 로그를 출력하고 새 모달 추가를 무시한다 — `if (OpenModal != null) { Log.Warn(...); return; }`
> **Q75.** 각 플레이어의 덱/유물/RNG 상태를 `SyncPlayerDataMessage`로 교환한다. Host만 RNG를 브로드캐스트하는 이유는 결정론적 보장 — 모든 클라이언트가 Host의 RNG 상태를 사용해야 같은 시드에서 동일한 결과가 나오기 때문이다.
> **Q76.** `owner.Connect(Node.SignalName.TreeExiting, Callable.From(() => Remove(owner)))` — owner 노드의 `TreeExiting` 시그널에 팁 제거 콜백을 연결하여 자동 정리한다.

---

## J. 오디오/VFX & 좀슐랭 적용 (19~20)

### J1. 객관식

**Q77.** STS2에서 오디오 볼륨을 설정할 때 `Mathf.Pow(volume, 2f)`를 적용하는 이유는?
- (a) 볼륨 범위를 0~100으로 확장
- (b) 선형 입력을 지각적으로 균일한 감쇠로 변환
- (c) FMOD API 요구사항
- (d) 오버플로우 방지

**Q78.** `NScreenShake`의 세 가지 흔들림 타입은?
- (a) Shake, Vibrate, Jolt
- (b) Punch, Rumble, Trauma
- (c) Light, Medium, Heavy
- (d) X, Y, XY

**Q79.** STS2 아키텍처에서 좀슐랭에 **지금 당장 빌려오면 안 되는** 시스템은?
- (a) Hook 시스템
- (b) Model-Command-Hook 패턴
- (c) Harmony 패치 시스템
- (d) 화면 전환 패턴

### J2. 단답형

**Q80.** `NHitStop`이 `Engine.SetTimeScale(0.1f)`를 사용할 때의 부작용은?

**Q81.** `CreatureAnimator`에서 루프 애니메이션의 시작 시간을 랜덤화하는 이유는?

**Q82.** 좀슐랭의 STS2 핵심 매핑에서 `CardModel` → ?, `RelicModel` → ?, `CombatState` → ? 를 채우시오.

### J3. 서술형

**Q83.** STS2의 C# → GDScript 변환 시 다음 패턴의 대응관계를 설명하시오:
- `static class` → ?
- `interface` → ?
- `async Task` → ?
- LINQ `.Where().Select()` → ?

**Q84.** 좀슐랭 개발에서 Phase 1(프로토타입)에 구현해야 할 핵심 시스템 4가지와, Phase 1 완료 체크리스트 4가지를 나열하시오.

---

### J. 정답

> [!faq]- J 정답 펼치기
> **Q77.** (b) 선형 입력을 지각적으로 균일한 감쇠로 변환 — 인간의 소리 인지는 로그 스케일
> **Q78.** (b) Punch, Rumble, Trauma
> **Q79.** (c) Harmony 패치 시스템 — 모딩 지원은 게임 완성 후에 추가해야 한다
> **Q80.** **전체 게임**이 슬로우모션이 된다 (Physics 포함). 특정 노드만 멈추는 것이 아니다.
> **Q81.** 같은 캐릭터 여러 마리가 완전히 동기화되어 움직이면 부자연스럽게 보이므로, 시작 시간과 속도(±10%)를 랜덤화하여 자연스러운 분산을 만든다.
> **Q82.** `CardModel` → `RecipeModel` (레시피), `RelicModel` → `EquipmentModel` (주방 도구), `CombatState` → `CookSession` (요리/사냥 세션)
> **Q83.** ① `static class` → **Autoload 싱글톤** (.gd 스크립트 + Project Settings 등록) ② `interface` → **duck typing 또는 class_name 상속** (공통 base class) ③ `async Task` → **await + Signal** (`await tween.finished`) ④ LINQ `.Where().Select()` → **`.filter().map()`** Array 메서드 체인
> **Q84.** **핵심 시스템**: ① BaseModel/RecipeModel/ZombieModel 데이터 구조 ② CookSession (전투 루프) ③ HookManager 기본 구현 ④ RunState (맵 위치, 재료, 장비) **완료 체크리스트**: ① 한 번의 요리 세션을 완주할 수 있다 ② 레시피 선택이 의미있는 결정이다 ③ 장비(유물)가 플레이 스타일을 변화시킨다 ④ 10분 이내에 런을 완주 또는 실패할 수 있다

---

## K. 종합 & 심화 문제

### K1. 아키텍처 설계

**Q85.** STS2의 전체 시스템 의존성 그래프에서, `UI` → `ActionExecutor` → `GameAction` → `Hook` → `AbstractModel` → `CombatState`의 흐름을 설명하고, 이 단방향 의존성이 아키텍처적으로 중요한 이유를 서술하시오.

**Q86.** `AbstractModel`이 100개 이상의 virtual Hook 메서드를 가지는 것의 장점과 단점을 각각 3가지씩 서술하시오.

### K2. 크로스 시스템 분석

**Q87.** 플레이어가 "카드를 플레이하여 적에게 데미지를 주고, 그 결과 적이 사망"하는 시나리오에서, 다음 시스템들이 관여하는 순서와 역할을 설명하시오:
`CardCmd`, `PlayCardAction`, `ActionExecutor`, `Hook`, `CreatureCmd`, `DamageCmd`, `CombatHistory`, `CombatManager`

**Q88.** 새로운 유물 "불꽃 검"을 만든다고 할 때 (효과: 모든 공격에 +3 데미지), 다음을 작성하시오:
1. 어떤 클래스를 상속하는가
2. 어떤 Hook 메서드를 오버라이드하는가
3. 실제 C# 구현 코드

### K3. 결정론적 시스템

**Q89.** STS2의 결정론적 리플레이가 가능하기 위한 3가지 전제 조건을 설명하시오. 만약 UI 파티클 효과에 `RunRngSet`의 RNG를 실수로 사용하면 어떤 문제가 발생하는가?

**Q90.** 다음 JSON 저장 데이터에서 런을 정확히 복원하기 위해 필요한 정보의 의미를 각각 설명하시오:

```json
{
  "seed": "MEGACRIT",
  "counters": {
    "Shuffle": 23,
    "MonsterAi": 201,
    "CombatTargets": 34
  }
}
```

### K4. 좀슐랭 설계

**Q91.** 좀슐랭에서 "독 좀비를 요리하면 셰프가 중독되는" 시스템을 STS2의 Hook 패턴으로 설계하시오. 필요한 Hook 이름, 관여하는 Model, 효과 흐름을 서술하시오.

**Q92.** 좀슐랭의 `EquipmentModel` "화염 칼"(효과: 모든 요리 데미지 +5, 내구도 10, 사용할 때마다 -1)을 GDScript로 구현하시오.

### K5. 비교 분석

**Q93.** STS2의 `Signal` (Godot 기본) vs `C# event Action<T>` — 각각의 장단점과, STS2가 C# event를 더 많이 사용하는 이유를 설명하시오.

**Q94.** `GameAction`의 상태 머신에서 `GatheringPlayerChoice` 상태가 필요한 이유를 멀티플레이어 관점에서 설명하시오.

### K6. 시스템 설계 문제

**Q95.** 좀슐랭에 "날씨 시스템"을 추가한다고 하자. STS2의 `RunRngSet` 패턴을 참고하여 다음을 설계하시오:
1. 어떤 RNG 타입을 추가할 것인가
2. 날씨가 영향을 미칠 Hook은 무엇인가
3. 저장/로드 시 날씨 상태를 어떻게 복원할 것인가

**Q96.** STS2의 `StandardActMap` 생성 알고리즘을 좀슐랭의 "감염 구역 맵"에 적용할 때, 교체해야 할 `MapPointType`과 배치 제약 규칙을 설계하시오.

### K7. 최종 통합 문제

**Q97.** STS2에서 "카드 하나를 추가"하려면 최소한 어떤 것들을 작성/수정해야 하는지 나열하시오. (클래스 작성, 풀 등록, 리소스 등)

**Q98.** STS2의 아키텍처가 "새 카드/유물 추가 = 새 클래스 하나 작성, 기존 코드 수정 없음"을 달성하는 방법을 Open/Closed Principle 관점에서 설명하시오.

**Q99.** STS2에서 배울 수 있는 가장 중요한 아키텍처 패턴 하나를 선택하고, 왜 그것이 로그라이크 장르에서 핵심적인지 논증하시오.

**Q100.** 지금까지 학습한 내용을 바탕으로, 좀슐랭 개발 시 "절대로 먼저 구현하면 안 되는 시스템" 3가지와 "반드시 먼저 구현해야 하는 시스템" 3가지를 근거와 함께 서술하시오.

---

### K. 정답

> [!faq]- K 정답 펼치기
> **Q85.** UI는 액션을 생성만 하고 실행하지 않는다 → ActionExecutor가 순서 보장 → GameAction이 Hook을 호출 → Hook이 AbstractModel 리스너를 순회 → Model이 CombatState를 읽기/수정. 이 단방향 의존성 덕분에 각 레이어를 독립적으로 테스트할 수 있고, 순환 의존이 없어 코드 이해와 디버깅이 용이하다.
> 
> **Q86.** **장점**: ① 새 유물/카드가 기존 코드 수정 없이 Hook 오버라이드만으로 효과 구현 가능 (OCP) ② Template Method 패턴으로 기본 동작 보장 ③ 모딩이 자연스럽게 지원됨. **단점**: ① 새 Hook 추가 시 AbstractModel, Hook.cs 모두 수정 필요 (보일러플레이트) ② 100개+ virtual 메서드로 인한 vtable 크기 ③ Hook 순서 의존성 디버깅이 어려움
> 
> **Q87.** ① UI에서 카드 드래그 → ② `PlayCardAction` 생성 → ③ `ActionExecutor` 큐에 추가, 순차 실행 → ④ `PlayCardAction.Execute()`에서 유효성 검사 → ⑤ `card.OnPlayWrapper()` → `Hook.BeforeCardPlayed` → ⑥ `DamageCmd.Attack()` → `Hook.BeforeAttack` → ⑦ `CreatureCmd.Damage()` → `Hook.ModifyDamage` → `LoseHpInternal` → `Hook.AfterDamageReceived` → ⑧ `CreatureCmd.Kill()` → `Hook.ShouldDie` → `Hook.AfterDeath` → ⑨ `CombatHistory.DamageReceived()` 기록 → ⑩ `CombatManager.CheckWinCondition()`
> 
> **Q88.**
> ```csharp
> public class FlameBlade : RelicModel
> {
>     public override decimal ModifyDamageAdditive(
>         Creature? target, decimal amount, ValueProp props,
>         Creature? dealer, CardModel? cardSource)
>         => 3m; // 모든 공격에 +3
> }
> ```
> ① `RelicModel` 상속 ② `ModifyDamageAdditive` 오버라이드 ③ 위 코드
> 
> **Q89.** ① 시드 고정 (`GetDeterministicHashCode`로 플랫폼 무관 해시) ② RNG 분리 (용도별 독립 RNG) ③ Counter 직렬화 (소비 횟수만 저장 후 `FastForwardCounter`로 복원). 파티클에 `RunRngSet` RNG를 사용하면 화면 해상도나 프레임 차이로 파티클 생성 횟수가 달라질 때 Counter가 어긋나서 이후 모든 게임 결과가 달라진다.
> 
> **Q90.** `"seed": "MEGACRIT"` = 런의 기본 시드 문자열, 이것으로 모든 RNG의 초기 상태를 재생성. `Shuffle: 23` = 카드/아이템 셔플 RNG가 23번 사용됨 → `FastForward(23)`으로 복원. `MonsterAi: 201` = 몬스터 AI 행동 결정에 201번 사용됨. `CombatTargets: 34` = 전투 타겟 결정에 34번 사용됨. 이 정보만으로 정확한 RNG 내부 상태를 재현할 수 있다.
> 
> **Q91.** ① `AfterDishCooked` 훅에서 좀비 타입 검사 → ② 독 좀비면 `StatusCmd.ApplyPoison(chef, toxicity)` 실행 → ③ `ModifyToxicity` Modify 훅으로 장비가 독성 경감 가능 → ④ `ShouldApplyPoison` Should 훅으로 특정 장비가 완전 면역 가능. 관여 Model: `ZombieModel`(독소 수치 보유), `EquipmentModel`(독 내성 장비), `StatusModel`(중독 상태)
> 
> **Q92.**
> ```gdscript
> class_name FlameKnife extends EquipmentModel
> 
> @export var bonus_damage: int = 5
> @export var max_durability: int = 10
> var current_durability: int = 10
> 
> func on_equipped(session) -> void:
>     HookManager.register(HookManager.Hook.BEFORE_COOK, _apply_flame_bonus)
> 
> func _apply_flame_bonus(ctx: Dictionary) -> void:
>     ctx["damage"] = ctx.get("damage", 0) + bonus_damage
>     current_durability -= 1
>     if current_durability <= 0:
>         _break_equipment()
> 
> func _break_equipment() -> void:
>     HookManager.unregister(HookManager.Hook.BEFORE_COOK, _apply_flame_bonus)
>     # 장비 파괴 효과
> ```
> 
> **Q93.** **Signal**: 장점 — 에디터에서 시각적 연결 가능, GDScript와 호환. 단점 — 타입 안전성 부족, 성능 오버헤드. **C# event**: 장점 — 컴파일 타임 타입 검사, 제네릭 지원 (`Action<T>`), 성능 우수. 단점 — 에디터 연결 불가, GDScript와 비호환. STS2는 순수 C# 로직이 대부분이므로 타입 안전성과 성능이 중요하여 C# event를 선호한다.
> 
> **Q94.** 멀티플레이어에서 한 플레이어의 GameAction이 다른 플레이어의 입력을 필요로 할 수 있다 (예: "상대방의 카드를 선택하세요"). `GatheringPlayerChoice` 상태에서 네트워크를 통해 해당 플레이어의 선택을 기다리고, 선택이 완료되면 `ReadyToResumeExecuting`으로 전환하여 실행을 재개한다. 이 상태가 없으면 원격 플레이어의 입력을 기다리는 동안 전체 게임이 블록된다.
> 
> **Q95.** ① `ZombieRngType.WeatherSystem` 추가 — 날씨 결정 전용 RNG ② `ModifyDamage`(폭풍 시 원거리 데미지 감소), `ModifyVisibility`(안개 시 시야 제한), `BeforeDayStart`(날씨 변경 발표) ③ `WeatherSystem` RNG의 Counter를 직렬화하고 현재 날씨 상태(enum)도 함께 저장. 복원 시 `FastForwardCounter`로 RNG 복원 후 날씨 enum으로 현재 상태 적용.
> 
> **Q96.** `MapPointType` 교체: `Monster` → `ZombieHorde`, `Elite` → `EliteHorde`, `Boss` → `InfectedGiant`, `RestSite` → `SafeHouse`, `Shop` → `Survivor(NPC)`, `Treasure` → `SupplyDrop`, `Unknown` → `Scavenging`. 배치 규칙: 초반 3행은 EliteHorde 금지, SafeHouse는 인접 행 중복 금지, 보스 직전 행은 반드시 SafeHouse, SupplyDrop은 같은 행에 2개 이상 금지.
> 
> **Q97.** ① `CardModel` 서브클래스 작성 (`.cs` 파일) — 타입, 비용, 키워드, DynamicVars, OnPlay, OnUpgrade 구현 ② 해당 캐릭터의 `CardPoolModel`에 자동 등록됨 (`[GenerateSubtypes]` 소스 생성기가 처리) ③ 포트레이트 이미지 추가 (`images/packed/cards/`) ④ 로컬라이제이션 텍스트 추가 (`localization/`) ⑤ (선택) 전용 VFX 씬 추가
> 
> **Q98.** `AbstractModel`의 virtual Hook 메서드 = 확장 포인트. 새 카드는 `CardModel`을 상속하고 필요한 Hook만 오버라이드한다. 기존 `Hook.cs`와 `ActionExecutor`는 `IterateHookListeners`로 모든 모델을 자동 순회하므로 수정 불필요. `[GenerateSubtypes]` 소스 생성기가 새 클래스를 자동 감지하여 `ModelDb`에 등록한다. 이것이 Open/Closed Principle — "확장에 열려있고(새 클래스 추가), 수정에 닫혀있다(기존 코드 변경 없음)".
> 
> **Q99.** **Hook 시스템(Observer 패턴)**이 로그라이크에서 핵심적인 이유: 로그라이크의 재미는 아이템/카드/유물 간의 **예측 불가능한 시너지**에서 나온다. Hook 시스템이 있으면 "유물 A는 공격 후에 +2 데미지, 파워 B는 3번째 공격마다 추가 타격"처럼 서로 모르는 시스템들이 독립적으로 반응하면서도 자연스럽게 조합된다. 이 패턴 없이는 모든 조합을 하드코딩해야 하므로 콘텐츠 확장이 불가능해진다.
> 
> **Q100.** **절대 먼저 구현하면 안 됨**: ① 멀티플레이어 동기화 (NetMessageBus 등) — 싱글플레이어 먼저 완성해야 함 ② 모딩 파이프라인 (Harmony, PCK) — 게임이 재미있어진 후에 ③ 클라우드 세이브/리더보드 — MVP 이후 추가. **반드시 먼저 구현**: ① Model-Command-Hook 삼위일체 — 모든 시스템의 기반, 나중에 바꾸기 매우 어려움 ② 시드 기반 RNG — 처음부터 결정론적으로 설계해야 나중에 세이브/리플레이 가능 ③ Canonical/Mutable 패턴 — 정의와 인스턴스 구분을 처음부터 해야 저장/모딩이 깔끔해짐

---

## 난이도 분포

| 난이도 | 문제 번호 | 비율 |
|--------|----------|------|
| 기초 (객관식/단답) | Q01~Q06, Q09~Q14, Q19~Q24, Q27~Q32, Q35~Q37, Q45~Q48, Q53~Q56, Q61~Q63, Q69~Q71, Q77~Q80 | 50% |
| 중급 (코드분석/서술) | Q07~Q08, Q15~Q18, Q25~Q26, Q33~Q34, Q38~Q44, Q49~Q52, Q57~Q60, Q64~Q68, Q72~Q76, Q81~Q84 | 35% |
| 심화 (설계/통합) | Q85~Q100 | 15% |

---

*이 문제집은 [[00-INDEX]]의 Godot Study 00~20번 문서를 기반으로 생성되었습니다.*
*관련: [[01-Godot-Engine-Basics]] ~ [[20-Apply-To-Zombchelin]]*
