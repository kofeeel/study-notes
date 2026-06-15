---
tags: [sts2, godot, godot-basics, csharp]
---

# STS2에서 배우는 Godot 엔진 기초

[[00-INDEX]] | 다음: [[02-Project-Structure]]

---

## 1. Godot의 Node/Scene 시스템

Godot의 핵심 철학은 **모든 것이 Node**라는 것이다. Node들이 트리 구조로 연결되고, 그 트리를 저장한 파일이 Scene(`.tscn`)이다.

### STS2의 최상위 씬 구조

`project.godot`에 정의된 메인 씬:
```ini
run/main_scene="res://scenes/game.tscn"
```

`game.tscn`이 루트 씬이고, 여기서 모든 것이 시작된다. STS2의 씬 계층은:

```
game.tscn (NGame : Control)
├── scene_container.tscn  ← 현재 화면 (메인메뉴, 런, 전투 등)
│   └── run.tscn (NRun)
│       └── combat_ui.tscn
│           ├── player_hand.tscn
│           ├── combat_piles_container.tscn
│           └── creature.tscn  (플레이어/몬스터)
└── [UI overlays, hover tips, etc.]
```

C# 코드에서 `NGame`은 `Control`을 상속한다:
```csharp
// src/Core/Nodes/NGame.cs
public partial class NGame : Control
{
    public static NGame? Instance { get; private set; }
    public NSceneContainer RootSceneContainer { get; private set; }
    public NRun? CurrentRunNode => RootSceneContainer.CurrentScene as NRun;
}
```

> [!note] `partial class` 키워드
> Godot C#에서 Node를 상속하는 클래스는 반드시 `partial`이어야 한다. Godot의 소스 제너레이터가 나머지 코드를 자동 생성하기 때문이다.

---

## 2. Autoload 싱글톤 패턴

`project.godot`의 `[autoload]` 섹션에 정의된 7개의 전역 싱글톤:

```ini
[autoload]

SentryInit="*res://addons/sentry/SentryInit.gd"
OneTimeInitialization="*res://scenes/one_time_initialization.tscn"
AssetLoader="*res://scenes/asset_loader.tscn"
DevConsole="*res://scenes/debug/dev_console.tscn"
CommandHistory="*res://scenes/debug/command_history.tscn"
MemoryMonitor="res://scenes/debug/memory_monitor.tscn"
FmodManager="*res://addons/fmod/FmodManager.gd"
```

앞에 `*`가 붙은 것은 게임 시작 시 **자동으로 씬 트리에 추가**되는 것이고, 없는 것(`MemoryMonitor`)은 수동 로드다.

| Autoload | 역할 |
|----------|------|
| `SentryInit` | 크래시 리포팅 (Sentry SDK 초기화) |
| `OneTimeInitialization` | 게임 최초 1회 초기화 (DB 로드, 모델 등록 등) |
| `AssetLoader` | 에셋 사전 로딩 관리 |
| `DevConsole` | 개발자 콘솔 (디버그 빌드) |
| `CommandHistory` | 디버그 명령 히스토리 |
| `MemoryMonitor` | 메모리 사용량 모니터링 |
| `FmodManager` | FMOD 오디오 엔진 |

### C#에서 Autoload 접근하기

GDScript에서는 노드 이름으로 바로 접근하지만, C#에서는 다르다:

```csharp
// GDScript 방식
# FmodManager.play_sound(...)

// C# 방식 — 싱글톤 패턴으로 직접 접근
SaveManager.Instance.PrefsSave.FastMode  // RunManager도 동일
RunManager.Instance.IsInProgress
CombatManager.Instance.IsEnding
```

STS2는 Autoload보다 C# 정적 싱글톤을 더 많이 활용한다. `RunManager`, `CombatManager`, `SaveManager` 등이 모두 `static Instance` 패턴이다.

---

## 3. Signal 시스템 기초

Godot의 Signal은 C#의 이벤트와 유사하지만, 엔진 레벨에서 지원하는 Observer 패턴이다.

### STS2에서의 Signal 사용

```csharp
// NGame.cs — Godot Signal 선언
public partial class NGame : Control
{
    [Signal]
    public delegate void WindowChangeEventHandler();
}

// CombatState.cs — 일반 C# 이벤트 (Signal 아님)
public event Action<CombatState>? CreaturesChanged;
```

> [!note] Signal vs C# Event
> STS2는 Godot Signal보다 **일반 C# event**를 훨씬 많이 사용한다. Godot Signal은 주로 GDScript와의 인터페이스나 씬 에디터에서 연결할 때 사용하고, 순수 C# 로직은 `event Action<T>`를 쓴다.

### GameAction의 이벤트 패턴

```csharp
// src/Core/GameActions/GameAction.cs
public abstract class GameAction
{
    public event Action<GameAction>? AfterFinished;
    public event Action<GameAction>? BeforeExecuted;
    public event Action<GameAction>? BeforeCancelled;
    public event Action<GameAction>? BeforePausedForPlayerChoice;
}
```

---

## 4. C# vs GDScript — STS2가 C#을 선택한 이유

`project.godot`에서 확인:
```ini
config/features=PackedStringArray("4.5", "C#", "Mobile")
```

### STS2 규모에서 GDScript의 한계

| 항목 | GDScript | C# (STS2 선택) |
|------|----------|----------------|
| 타입 안전성 | 동적 타입, 런타임 에러 | 정적 타입, 컴파일 타임 에러 |
| IDE 지원 | 제한적 | Rider/VS 완전 지원 |
| 성능 | 인터프리터 | JIT 컴파일 |
| 코드 규모 | 소규모 적합 | 수만 줄 이상 관리 가능 |
| `async/await` | 미지원 (signal await만) | 완전 지원 |
| 소스 생성기 | 불가 | `[GenerateSubtypes]` 등 활용 |
| 리플렉션 | 제한 | 완전한 .NET 리플렉션 |

STS2에서 `async/await`는 핵심 패턴이다. 카드 플레이 한 번에도 수십 개의 Hook이 순차적으로 `await`된다:

```csharp
// Hook.cs — 모든 Hook이 async/await
public static async Task AfterCardPlayed(IRunState runState, CombatState combatState, ...)
{
    foreach (AbstractModel model in combatState.IterateHookListeners())
    {
        await model.AfterCardPlayed(choiceContext, cardPlay);
        model.InvokeExecutionFinished();
    }
}
```

GDScript로는 이 패턴을 구현할 수 없다.

---

## 5. Export / Resource 시스템

### @export (GDScript) / [Export] (C#)

Godot에서 Inspector에 값을 노출하거나 씬 파일에 직렬화하려면 Export를 사용한다.

```csharp
// C# 방식
public partial class NGame : Control
{
    [Export] private Control _inspectionContainer;
    [Export] private NScreenShake _screenShake;
}
```

STS2는 `.tscn` 씬 파일에서 노드 참조를 연결하고, C# 코드에서 `[Export]`로 받는다.

### Resource (.tres, .res)

Godot Resource는 직렬화 가능한 데이터 컨테이너다. STS2는 `.tres` 파일을 주로 테마/오디오 버스 설정에 사용한다:

```
default_bus_layout.tres   ← 오디오 버스 레이아웃
themes/                   ← UI 테마 리소스
materials/                ← 셰이더 머티리얼
```

게임 데이터(카드, 유물 등)는 `.tres` 대신 **C# 클래스 인스턴스**로 관리한다. `AbstractModel` 서브클래스가 그 역할을 한다.

---

## 6. 라이프사이클 메서드

### Godot Node 라이프사이클

```
_EnterTree()   → 노드가 씬 트리에 추가될 때
_Ready()       → 노드와 모든 자식이 준비됐을 때  ← 가장 많이 사용
_Process(delta) → 매 프레임 (Update에 해당)
_ExitTree()    → 노드가 씬 트리에서 제거될 때
```

### STS2의 라이프사이클 활용

STS2는 C# `partial class`로 노드를 구현하므로 메서드 이름이 다르다:

```csharp
// C# 오버라이드 방식
public partial class NGame : Control
{
    public override void _Ready()
    {
        Instance = this;
        // 초기화 코드
    }

    public override void _Process(double delta)
    {
        // 매 프레임 업데이트
    }
}
```

> [!note] STS2의 특징
> STS2는 `_Process()`를 거의 사용하지 않는다. 대신 **async/await 기반 GameAction 큐**가 게임 로직을 처리한다. 애니메이션이 끝날 때까지 기다리는 것도 `await Cmd.Wait(seconds)`로 처리한다.

---

## 7. project.godot 설정 분석

### 주요 설정 해설

```ini
[application]
config/name="Slay the Spire 2"
run/main_scene="res://scenes/game.tscn"      # 진입점
config/use_custom_user_dir=true
config/custom_user_dir_name="SlayTheSpire2"  # 세이브 경로

[physics]
2d/physics_engine="Dummy"   # 물리 엔진 비활성화!
3d/physics_engine="Dummy"   # 카드게임이라 물리 불필요

[rendering]
rendering_device/driver.windows="d3d12"      # DirectX 12 사용
environment/defaults/default_clear_color=Color(0.0923, 0.1223, 0.1169, 1)  # 어두운 청록

[display]
window/size/viewport_width=1920
window/size/viewport_height=1080
window/stretch/mode="canvas_items"   # UI 스케일링 방식
window/stretch/aspect="expand"       # 화면 비율 확장
```

### 주목할 점: 물리 엔진 완전 비활성화

```ini
2d/physics_engine="Dummy"
3d/physics_engine="Dummy"
```

STS2는 카드 게임이라 물리 시뮬레이션이 전혀 필요 없다. `Dummy` 엔진을 사용하면 불필요한 CPU/메모리를 절약할 수 있다.

### FMOD 설정

```ini
[Fmod]
General/is_live_update_enabled=false
General/banks_path="res://banks/desktop"
```

오디오를 Godot 내장 오디오 대신 FMOD를 사용한다. 게임 오디오 전문 미들웨어로 복잡한 사운드 로직을 처리한다.

---

## 좀슐랭에 적용한다면

```ini
# 좀슐랭 project.godot 참고 설정
[application]
config/name="좀슐랭"
run/main_scene="res://scenes/game.tscn"
config/custom_user_dir_name="Zomblang"

[physics]
# 좀비 게임 → 움직임 있음 → 물리 엔진 필요할 수도
# CharacterBody2D 사용 시 기본 Godot Physics 유지

[display]
window/size/viewport_width=1920
window/size/viewport_height=1080
window/stretch/mode="canvas_items"
```

**Autoload 설계 제안:**

| Autoload | 역할 |
|----------|------|
| `GameManager` | 전체 게임 상태 (메뉴↔런↔전투) |
| `RunManager` | 현재 런 상태 (층수, 덱, 유물) |
| `AudioManager` | 사운드 재생 (FMOD 대신 Godot 내장) |
| `SaveManager` | 세이브/로드 |

**C# 채택 권장:** 좀슐랭도 Hook 시스템, async 카드 효과 등을 구현하려면 C#이 필수적이다. GDScript 프로토타입에서 C# 본 개발로 전환 시점을 초반에 정해두자.

---

*관련 문서: [[02-Project-Structure]] | [[03-Architecture-Overview]] | [[06-GameAction-System]]*
