---
tags: [sts2, godot, ui, scene, csharp]
---

# 18. UI/씬 구조 (UI & Scene Structure)

#sts2 #godot #ui #scene

STS2의 UI는 Godot Control 노드 계층 + C# 스크립트 패턴으로 구성된다. 씬 전환, 모달, 호버팁, 커스텀 리치텍스트까지 모두 코드에서 직접 관리한다.

---

## 최상위 씬 계층 구조

```
game.tscn (NGame - Control)
├── FmodBankLoader          ← FMOD 오디오 뱅크 로드
├── AudioManager (NAudioManager)
│   ├── Proxy (GDScript 브릿지)
│   └── FmodListener2D
├── DebugAudioManager
├── CursorManager (NCursorManager)
├── ControllerManager (NControllerManager)
├── HitStop (NHitStop)
├── ScreenShake (NScreenShake)
├── Transition (NTransition)     ← 화면 전환
├── ModalContainer (NModalContainer)  ← 팝업/모달
├── HotkeyManager (NHotkeyManager)
├── InputManager (NInputManager)
├── SceneContainer (scene_container.tscn)  ← 실제 콘텐츠
├── ReactionContainer (NReactionContainer)
├── RemoteMouseCursorContainer
├── MultiplayerTimeoutOverlay
└── FeedbackScreenOpener
```

> [!note] Godot에서 `unique_name_in_owner = true` 설정된 노드는 `%NodeName`으로 어디서든 접근 가능

---

## 씬 디렉터리 구조

```
scenes/
├── game.tscn             ← 루트 씬 (항상 존재)
├── run.tscn              ← 런 중 씬
├── scene_container.tscn  ← 화면 교체 컨테이너
├── screens/              ← 풀스크린 화면들
│   ├── main_menu/
│   ├── char_select/
│   ├── map/
│   ├── settings_screen/
│   ├── game_over_screen/
│   ├── rewards_screen.tscn
│   └── deck_view_screen/
├── ui/                   ← 공통 UI 컴포넌트
│   ├── hover_tip.tscn
│   ├── hover_tip_set.tscn
│   ├── top_bar.tscn
│   ├── reaction_wheel.tscn
│   └── multiplayer/
├── cards/
├── combat/
├── vfx/                  ← 비주얼 이펙트
└── backgrounds/
```

---

## NTransition — 화면 전환 시스템

```csharp
// F:/Projects/godot_study/extracted/src/Core/Nodes/NTransition.cs
public partial class NTransition : ColorRect
{
    // 4가지 전환 메서드
    public async Task FadeOut(float time = 0.8f, string transitionPath = "..fade_transition_mat.tres") { ... }
    public async Task FadeIn(float time = 0.8f, ...) { ... }
    public async Task RoomFadeOut() { ... }  // 방(전투→맵) 전환
    public async Task RoomFadeIn(bool showTransition = true) { ... }

    public bool InTransition { get; private set; }
}
```

**ShaderMaterial 기반 전환:**
```csharp
// threshold 파라미터를 0→1로 트위닝하여 전환 효과
transitionMaterial.SetShaderParameter(_threshold, 0);
while (t < time)
{
    transitionMaterial.SetShaderParameter(_threshold, 1.0 - (time - t));
    t += GetProcessDeltaTime();
    await ToSignal(GetTree(), SceneTree.SignalName.ProcessFrame);
}
```

**패스트 모드 지원:**
```csharp
if (SaveManager.Instance.PrefsSave.FastMode == FastModeType.Instant)
{
    InTransition = true;
    Visible = false;
    return; // 즉시 전환
}
```

---

## NModalContainer — 모달 시스템

```csharp
// F:/Projects/godot_study/extracted/src/Core/Nodes/CommonUi/NModalContainer.cs
public partial class NModalContainer : Control
{
    public static NModalContainer? Instance { get; private set; }  // 싱글턴
    public IScreenContext? OpenModal { get; private set; }

    public void Add(Node modalToCreate, bool showBackstop = true)
    {
        if (OpenModal != null) { Log.Warn("Another modal already open."); return; }
        OpenModal = (IScreenContext)modalToCreate;
        AddChildSafely(modalToCreate);
        ActiveScreenContext.Instance.Update();
        if (showBackstop) ShowBackstop();
    }

    public void Clear()
    {
        // 모달 노드 제거 + 백스톱(어두운 배경) 숨기기
    }

    // 백스톱: alpha 0→0.85로 0.3초 트위닝
    public void ShowBackstop() { ... }
    public void HideBackstop() { ... }
}
```

**사용 패턴:**
```csharp
// 팝업 열기
var popup = PackedScene.Instantiate<MyPopup>();
NModalContainer.Instance.Add(popup);

// 팝업 닫기
NModalContainer.Instance.Clear();
```

---

## NHoverTipSet — 호버팁 시스템

```csharp
// F:/Projects/godot_study/extracted/src/Core/Nodes/HoverTips/NHoverTipSet.cs
public static NHoverTipSet CreateAndShow(Control owner, IHoverTip hoverTip,
    HoverTipAlignment alignment = HoverTipAlignment.None)
{
    // PreloadManager 캐시에서 씬 인스턴스화
    NHoverTipSet set = PreloadManager.Cache
        .GetScene("res://scenes/ui/hover_tip_set.tscn")
        .Instantiate<NHoverTipSet>();

    // HoverTipsContainer (game.tscn 전역)에 추가
    HoverTipsContainer.AddChildSafely(set);
    _activeHoverTips.Add(owner, set);
    set.Init(owner, hoverTips);

    // owner가 트리에서 제거되면 자동으로 팁도 제거
    owner.Connect(Node.SignalName.TreeExiting, Callable.From(() => Remove(owner)));
    return set;
}
```

**호버팁 타입:**
- `HoverTip` — 텍스트 (Title + Description + Icon + IsDebuff)
- `CardHoverTip` — 카드 미리보기 포함
- `NMapPointHistoryHoverTip` — 맵 노드 이력

**정렬 자동 교정:**
```csharp
// 화면 밖으로 나가면 자동으로 위치 보정
private void CorrectVerticalOverflow() { ... }
private void CorrectHorizontalOverflow() { ... }
```

**팁 표시 시 자동 "본 항목" 마킹:**
```csharp
if (canonicalModel is CardModel card)
    SaveManager.Instance.MarkCardAsSeen(card);
else if (canonicalModel is RelicModel relic)
    SaveManager.Instance.MarkRelicAsSeen(relic);
```

---

## 테마 시스템

```
themes/
├── fonts/
│   ├── kreon_bold_*.tres        ← 주요 UI 폰트 (Bold)
│   ├── kreon_regular_*.tres     ← 본문 폰트
│   ├── spectral_bold_shared.tres ← 고대 이름 배너용
│   └── source_code_pro_*.tres   ← 코드/디버그용
├── main_menu_text_button.tres
├── top_bar_floor.tres            ← 층수 표시 스타일
├── top_bar_gold.tres             ← 골드 표시 스타일
└── top_bar_hp.tres               ← HP 표시 스타일
```

**FontVariation 사용 패턴:** `.tres` 리소스로 폰트 변형(자간, 행간)을 미리 정의해두고 재사용.

---

## MegaText — 커스텀 리치텍스트

```csharp
// addons/mega_text/
// MegaLabel.cs          ← 단순 텍스트 (자동 크기 조절 지원)
// MegaRichTextLabel.cs  ← 리치텍스트 (BBCode 기반 키워드 강조)
// MegaLabelHelper.cs    ← 공통 유틸
// ThemeConstants.cs     ← 테마 상수 정의
```

**사용 예:**
```csharp
// 호버팁 내 텍스트 설정
control.GetNode<MegaLabel>("%Title").SetTextAutoSize(hoverTip.Title);
control.GetNode<MegaRichTextLabel>("%Description").Text = hoverTip.Description;

// 자동 줄바꿈 모드 제어
label.AutowrapMode = hoverTip.ShouldOverrideTextOverflow
    ? TextServer.AutowrapMode.Disabled
    : TextServer.AutowrapMode.WordSmart;
```

게임 내 키워드(블록, 데미지, 상태이상 등)는 MegaRichTextLabel의 BBCode 파싱으로 색상/아이콘이 자동 삽입된다.

---

## screens/ 주요 화면 목록

| 씬 | 설명 |
|----|------|
| `main_menu.tscn` | 메인 메뉴 |
| `character_select_screen.tscn` | 캐릭터 선택 |
| `map/` | 맵 탐색 화면 |
| `deck_view_screen.tscn` | 덱 보기 |
| `rewards_screen.tscn` | 보상 선택 |
| `game_over_screen.tscn` | 게임 오버 |
| `settings_screen.tscn` | 설정 |
| `card_pile_screen.tscn` | 카드 더미 보기 |
| `inspect_card_screen.tscn` | 카드 상세 보기 |
| `modding/` | 모드 관리 화면 |

---

## NSceneContainer — 씬 전환 컨테이너

`scene_container.tscn`은 현재 활성 화면 노드를 담는 컨테이너. 화면 전환 시:
1. `NTransition.FadeOut()` 호출
2. 기존 씬 제거, 새 씬 추가
3. `NTransition.FadeIn()` 호출

---

## 좀슐랭에 적용한다면

> [!note] 좀슐랭 UI 구조 설계

**씬 계층 (game.tscn 패턴 적용):**
```
ZombchelinGame (Control)
├── AudioManager
├── Transition (NTransition 패턴)
├── ModalContainer
├── HoverTipContainer
└── SceneContainer
    ├── MainMenu
    ├── KitchenScreen     ← STS2 전투 화면 대응
    ├── MapScreen         ← STS2 맵 화면 대응
    └── RewardScreen      ← STS2 보상 화면 대응
```

**호버팁 패턴 차용:**
```gdscript
# 재료/레시피 호버팁
class RecipeHoverTip:
    var title: String
    var description: String
    var ingredients: Array  # 재료 목록
    var cooking_time: float

# 화면 밖 overflow 보정 로직은 NHoverTipSet에서 그대로 차용
```

**모달 패턴:**
- 요리 확인 팝업, 레시피 상세 화면 → `NModalContainer` 패턴
- 싱글턴 인스턴스 + `Add()`/`Clear()` API 그대로 사용 가능

**테마:**
- 좀비 느낌의 폰트 + 녹슨/피묻은 컬러 팔레트
- `.tres` 파일로 버튼/라벨 스타일 분리해두면 나중에 일괄 변경 쉬움
