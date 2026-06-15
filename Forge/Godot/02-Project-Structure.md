---
tags: [sts2, godot, architecture, project-structure]
---

# STS2 프로젝트 구조 분석

[[01-Godot-Engine-Basics]] | [[00-INDEX]] | 다음: [[03-Architecture-Overview]]

---

## 전체 디렉토리 트리

```
F:/Projects/godot_study/extracted/
├── project.godot          ← Godot 프로젝트 설정
├── sts2.csproj            ← C# 프로젝트 파일
├── sts2.sln               ← Visual Studio 솔루션
├── global.json            ← .NET SDK 버전 고정
├── packages.lock.json     ← NuGet 패키지 잠금
├── release_info.json      ← 빌드/릴리즈 메타데이터
├── default_bus_layout.tres ← Godot 오디오 버스 설정
│
├── src/                   ← C# 게임 로직 (핵심!)
│   ├── Core/              ← 엔진 코어 (50개+ 하위 폴더)
│   ├── GameInfo/          ← 게임 메트릭 업로더
│   ├── SourceGeneration/  ← 소스 제너레이터
│   └── gdscript/          ← GDScript 헬퍼 (최소)
│
├── scenes/                ← Godot 씬 파일 (.tscn)
├── images/                ← 텍스처/스프라이트
├── animations/            ← 스파인 애니메이션
├── fonts/                 ← 폰트 리소스
├── shaders/               ← GLSL 셰이더
├── materials/             ← 셰이더 머티리얼
├── banks/                 ← FMOD 사운드 뱅크
├── localization/          ← 다국어 번역 파일
├── themes/                ← Godot UI 테마
├── models/                ← 3D 모델 (배경용)
├── addons/                ← Godot 플러그인
├── steam/                 ← Steam SDK 파일
└── debug_audio/           ← 오디오 디버그 에셋
```

---

## src/Core/ 구조 — 50개 하위 폴더 역할

`src/Core/`는 게임의 모든 로직이 담긴 핵심 폴더다.

### 데이터/상태 (Data & State)

| 폴더 | 역할 |
|------|------|
| `Models/` | 모든 게임 오브젝트의 데이터 모델 (CardModel, RelicModel, PowerModel 등) |
| `Combat/` | 전투 상태 (CombatState, CombatManager) |
| `Runs/` | 런 진행 상태 (RunManager, IRunState) |
| `Saves/` | 세이브/로드 시스템 |
| `Context/` | LocalContext — 현재 플레이어 컨텍스트 (멀티플레이어 지원) |

### 게임 로직 (Game Logic)

| 폴더 | 역할 |
|------|------|
| `Hooks/` | `Hook.cs` — 전역 이벤트 디스패처 (Observer 패턴) |
| `GameActions/` | `GameAction` 추상 클래스 + 구체 액션들, `ActionExecutor` |
| `Commands/` | `Cmd.Wait()` 등 유틸리티 커맨드, 커맨드 빌더 |
| `Entities/` | 카드/크리처/플레이어/유물 등 Entity 로직 |
| `Factories/` | 카드/유물/포션 생성 팩토리 |
| `Map/` | ActMap, 맵 노드/경로 생성 |
| `Rooms/` | AbstractRoom, CombatRoom, MerchantRoom 등 |
| `Rewards/` | 보상 시스템 |
| `Odds/` | 확률 계산 |
| `Random/` | 결정론적 RNG (재현 가능한 시드) |

### 시스템 서비스 (System Services)

| 폴더 | 역할 |
|------|------|
| `Nodes/` | Godot Node를 상속하는 모든 C# 클래스 (NGame, NRun 등) |
| `Assets/` | 에셋 로딩/참조 |
| `Audio/` | 오디오 래퍼 |
| `Localization/` | 다국어 LocString 시스템 |
| `Settings/` | 게임 설정 (FastMode 등) |
| `Multiplayer/` | 멀티플레이어 네트워킹, 직렬화 |
| `Logging/` | 로깅 시스템 |
| `Platform/` | Steam, 플랫폼별 추상화 |

### 개발 도구 (Dev Tools)

| 폴더 | 역할 |
|------|------|
| `Debug/` | 디버그 유틸리티 |
| `DevConsole/` | 개발자 콘솔 명령 |
| `TestSupport/` | 단위 테스트 지원 (`TestMode.IsOn` 체크 등) |
| `AutoSlay/` | 자동 플레이 AI (테스트용) |

### 특수 시스템

| 폴더 | 역할 |
|------|------|
| `CardSelection/` | 카드 선택 UI 로직 |
| `HoverTips/` | 호버 툴팁 시스템 |
| `Modding/` | 모딩 지원 인프라 |
| `Achievements/` | 업적 시스템 |
| `Leaderboard/` | 리더보드 연동 |
| `Timeline/` | 타임라인/에포크 시스템 (STS2 신기능) |
| `Extensions/` | C# 확장 메서드 |
| `Helpers/` | 범용 헬퍼 유틸리티 |
| `Exceptions/` | 커스텀 예외 타입 |
| `ValueProps/` | 값 프로퍼티 래퍼 (데미지/블록 수식) |

---

## scenes/ 구조

```
scenes/
├── game.tscn                  ← 루트 씬 (NGame)
├── run.tscn                   ← 런 컨테이너 (NRun)
├── scene_container.tscn       ← 씬 전환 컨테이너
├── asset_loader.tscn          ← Autoload: 에셋 로더
├── one_time_initialization.tscn ← Autoload: 초기화
│
├── combat/                    ← 전투 UI 씬
│   ├── combat_ui.tscn
│   ├── player_hand.tscn
│   ├── creature.tscn
│   ├── health_bar.tscn
│   ├── draw_pile.tscn
│   ├── discard_pile.tscn
│   ├── exhaust_pile.tscn
│   ├── end_turn_button.tscn
│   ├── intent.tscn
│   └── energy_counters/
│
├── cards/                     ← 카드 비주얼 씬
├── screens/                   ← 게임 화면들 (메인메뉴, 설정 등)
├── rooms/                     ← 방 타입별 씬
├── rewards/                   ← 보상 화면
├── ui/                        ← 공통 UI 컴포넌트
├── vfx/                       ← 비주얼 이펙트
│
├── relics/                    ← 유물 비주얼
├── potions/                   ← 포션 비주얼
├── orbs/                      ← 오브 비주얼
├── events/                    ← 이벤트 화면
├── merchant/                  ← 상인 화면
├── rest_site/                 ← 휴식 화면
├── map/                       ← 맵 화면 (scenes/rooms/에 포함)
├── creature_visuals/          ← 몬스터 스파인 애니메이션 씬
├── pause_menu/                ← 일시정지 메뉴
├── backgrounds/               ← 배경 씬
├── encounters/                ← 인카운터 설정
├── timeline_screen/           ← 타임라인 화면
├── ftue/                      ← 튜토리얼 (First Time User Experience)
│
└── debug/                     ← 디버그 씬
    ├── dev_console.tscn       ← Autoload: 개발자 콘솔
    ├── command_history.tscn   ← Autoload: 커맨드 히스토리
    └── memory_monitor.tscn    ← Autoload: 메모리 모니터
```

---

## 에셋 관리

### images/

```
images/
├── packed/          ← 텍스처 아틀라스 (atlas_generator 플러그인으로 생성)
│   ├── common_ui/   ← 공통 UI 스프라이트
│   ├── cards/       ← 카드 아트
│   ├── relics/      ← 유물 아이콘
│   └── ...
├── icon_1024.png    ← 앱 아이콘
└── icon.ico         ← Windows 아이콘
```

> [!note] 텍스처 아틀라스
> `addons/atlas_generator` 플러그인으로 개별 이미지를 하나의 큰 아틀라스 텍스처로 합친다. 드로우콜 최소화를 위한 최적화 기법이다.

### animations/

스파인(Spine) 2D 골격 애니메이션 데이터. `addons/megacontentcreator` (MegaSpine)가 이를 처리한다. 카드 게임임에도 몬스터/플레이어 캐릭터에 풍부한 애니메이션을 사용한다.

### shaders/

```
shaders/
├── combat/        ← 전투 화면 셰이더
├── cards/         ← 카드 비주얼 효과
└── ui/            ← UI 셰이더
```

주목할 파일: `scenes/combat/doom_bar.gdshader` — 전투 화면의 둠 바 효과.

### localization/

다국어 번역 파일. STS2는 여러 언어를 지원하며 `LocString` 시스템으로 텍스트를 참조한다.

---

## addons/ — Godot 플러그인

| 플러그인 | 역할 |
|----------|------|
| `atlas_generator/` | 텍스처 아틀라스 자동 생성 에디터 플러그인 |
| `dev_tools/` | MegaCrit 내부 개발 도구 |
| `fmod/` | FMOD Studio 오디오 엔진 연동 |
| `mega_text/` | 커스텀 리치 텍스트 렌더러 (카드 설명 등) |
| `megacontentcreator/` | 스파인 애니메이션 + 콘텐츠 파이프라인 |
| `sentry/` | Sentry 에러 트래킹 SDK |

### FMOD 구조

```
banks/
└── desktop/       ← FMOD 사운드 뱅크 파일 (.bank)
    ├── Master.bank
    ├── Master.strings.bank
    └── ...
```

`project.godot`에서:
```ini
[Fmod]
General/banks_path="res://banks/desktop"
```

---

## 설정 파일들

### sts2.csproj

```xml
<!-- 핵심 내용 -->
<Project Sdk="Godot.NET.Sdk/4.5.0">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
  </PropertyGroup>
</Project>
```

- **Nullable 참조 타입 활성화**: `?` 없으면 null 불가 → 크래시 방지
- **Godot.NET.Sdk**: Godot 전용 .NET SDK

### global.json

```json
{
  "sdk": {
    "version": "8.0.0"
  }
}
```

.NET 8 LTS를 사용. Godot 4.x의 현재 권장 버전.

### release_info.json

빌드 버전, 출시일 등 메타데이터.

---

## src/gdscript/ — GDScript의 역할 (최소)

STS2는 C# 프로젝트이지만 `src/gdscript/` 폴더가 존재한다. 역할:

- Godot 에디터 플러그인용 GDScript (C#에서 플러그인 작성 불가)
- 일부 Autoload 씬의 GDScript 스크립트 (`SentryInit.gd`, `FmodManager.gd`)
- 에디터 도구 스크립트

게임 로직은 **100% C#**, GDScript는 엔진 인터페이스 레이어에만 사용한다.

---

## 좀슐랭에 적용한다면

### 권장 폴더 구조

```
zomblang/
├── project.godot
├── zomblang.csproj
│
├── src/
│   └── Core/
│       ├── Models/       ← ZombieModel, RecipeModel, WeaponModel
│       ├── Combat/       ← CombatState (STS2와 유사)
│       ├── Hooks/        ← Hook.cs (STS2 패턴 그대로 차용 가능)
│       ├── GameActions/  ← PlayCardAction → AttackAction, CookAction
│       ├── Runs/         ← RunManager (층수/생존 관리)
│       ├── Nodes/        ← NGame, NCombat 등
│       └── Saves/        ← 세이브 시스템
│
├── scenes/
│   ├── game.tscn
│   ├── combat/
│   ├── map/              ← 생존 맵
│   ├── kitchen/          ← 요리 인터페이스
│   └── ui/
│
├── images/
│   └── packed/           ← atlas_generator 활용
├── animations/           ← 좀비 스파인 애니메이션 (선택)
└── addons/
    └── fmod/             ← 오디오 (선택, 내장 오디오도 충분)
```

> [!note] STS2의 `src/Core/` 구조는 잘 설계된 템플릿이다. Hooks, GameActions, Models 패턴을 그대로 가져오면 좀슐랭의 복잡한 시너지 시스템을 깔끔하게 구현할 수 있다.

---

*관련 문서: [[01-Godot-Engine-Basics]] | [[03-Architecture-Overview]] | [[04-Model-System]]*
