# 언리얼 엔진 UMG & CommonUI 완전 학습 가이드
### (인벤토리 & 크래프팅 테크데모 개발용 | GAS 숙지자 기준)

> **대상 독자**: GAS(Gameplay Ability System)를 이미 알고 있으며, 인벤토리 & 크래프팅 시스템 UI를 구축하려는 개발자  
> **엔진 버전**: Unreal Engine 5.3+  
> **언어**: C++ 중심, Blueprint 보조 설명 포함

---

## 목차

- [PART 1: Actor 라이프사이클 핵심 정리](#part-1)
- [PART 2: UMG 심화](#part-2)
- [PART 3: CommonUI 완전 정복](#part-3)
- [PART 4: 인벤토리 & 크래프팅 UI 설계 패턴](#part-4)
- [PART 5: 개발 체크리스트 & 자주 하는 실수](#part-5)

---

## PART 1: Actor 라이프사이클 핵심 정리

인벤토리 및 크래프팅 시스템에서 아이템은 다양한 형태로 존재합니다. 월드에 드랍된 `PickupActor`, 캐릭터가 장착한 `EquipActor`, 그리고 인벤토리 컴포넌트 내의 데이터 구조체 등입니다. 이러한 액터들이 월드에 생성되고 소멸되는 과정을 정확히 이해해야 메모리 관리, 데이터 동기화, 그리고 예기치 못한 크래시 문제를 완벽히 방지할 수 있습니다.

### 1.1 전체 라이프사이클 흐름 다이어그램

액터의 생명주기는 엔진 내부에서 매우 정교하게 설계되어 있습니다. 각 단계는 특정 목적을 가지고 있으며, 이를 정확한 시점에 활용하는 것이 중요합니다.

| 단계 | 함수명 | 주요 역할 및 활용 시나리오 |
| :--- | :--- | :--- |
| **생성** | `Constructor` | 클래스 기본 객체(CDO) 설정. 컴포넌트 생성 및 기본값 설정. |
| **데이터 로드** | `PostInitProperties` | 프로퍼티 로드 완료 후 호출. 기본값 기반의 추가 계산. |
| **에디터 배치** | `OnConstruction` | 블루프린트 Construction Script 대응. 에디터 상의 실시간 변화 처리. |
| **컴포넌트 초기화** | `PreInitializeComponents` | 컴포넌트 초기화 전 호출. 액터 수준의 사전 준비. |
| | `InitializeComponent` | 각 컴포넌트의 초기화 로직 (bWantsInitializeComponent=true 필요). |
| | `PostInitializeComponents` | 모든 컴포넌트 초기화 완료. 컴포넌트 간 참조 연결의 최적기. |
| **게임 시작** | `BeginPlay` | 액터가 게임에 본격적으로 투입되는 시점. GAS ASC 초기화 및 델리게이트 바인딩. |
| **업데이트** | `Tick` | 매 프레임 실행. 인벤토리 액터에서는 성능을 위해 가급적 비활성화. |
| **종료 시작** | `EndPlay` | 액터 제거 직전. 타이머 해제, 델리게이트 언바인딩, 메모리 정리. |
| **메모리 해제** | `BeginDestroy` | UObject 파괴 시작. 가비지 컬렉션(GC) 진입 전 단계. |
| | `FinishDestroy` | 메모리에서 완전히 제거되는 최종 단계. |

---

### 1.2 각 단계별 상세 설명 및 활용

#### 1. Constructor (생성자)
- **언제 호출되는가:** 클래스의 인스턴스가 메모리에 할당될 때 가장 먼저 호출됩니다. (CDO 생성 시 포함)
- **주요 용도:** `CreateDefaultSubobject`를 통한 컴포넌트 생성, 기본 변수 값 설정, 클래스 기본 속성 정의.
- **주의사항:** 월드(`GetWorld()`)에 접근하거나 다른 액터를 참조하는 로직은 금지됩니다. 아직 월드에 배치된 상태가 아니기 때문입니다.
- **인벤토리 활용:** 아이템 액터의 기본 충돌 설정(Collision Profile), 메쉬 컴포넌트의 기본 계층 구조를 정의합니다.

#### 2. PostInitializeComponents
- **언제 호출되는가:** 모든 컴포넌트가 생성되고 초기화된 직후입니다.
- **주요 용도:** **GAS 개발자에게 매우 중요한 지점**입니다. ASC가 액터에 부착되어 있고, 컴포넌트 간의 상호 참조를 설정하기 가장 좋은 시점입니다.
- **인벤토리 활용:** 인벤토리 컴포넌트와 ASC 간의 델리게이트 연결, 또는 아이템 데이터 에셋을 기반으로 한 초기 컴포넌트 설정을 수행합니다.

#### 3. BeginPlay
- **언제 호출되는가:** 액터가 월드에 스폰되거나 레벨이 시작될 때 호출됩니다.
- **주요 용도:** 게임 플레이 로직의 실질적인 시작점. 타이머 설정, 위젯 생성, 초기 데이터 요청 등.
- **인벤토리 활용:** `PickupActor`가 스폰되었을 때, 해당 아이템의 데이터 에셋을 기반으로 외형(Mesh)을 설정하거나, 서버로부터 아이템 정보를 받아오는 로직을 실행합니다.

#### 4. EndPlay
- **언제 호출되는가:** `Destroy()`가 호출되거나 레벨이 전환될 때 호출됩니다.
- **주요 용도:** 타이머 해제, 델리게이트 언바인딩, 동적 생성된 리소스 정리.
- **주의사항:** `EEndPlayReason`을 통해 왜 파괴되는지(LevelTransition, Destroyed 등)를 확인할 수 있습니다. 델리게이트 해제를 잊으면 메모리 누수나 댕글링 포인터 문제가 발생합니다.

---

### 1.3 C++ vs Blueprint 대응 관계

| C++ Function | Blueprint Event | 비고 |
| :--- | :--- | :--- |
| `PostInitializeComponents` | (없음) | C++ 전용 (컴포넌트 초기화 완료 시점) |
| `OnConstruction` | `Construction Script` | 에디터 상의 변화 대응 |
| `BeginPlay` | `Event BeginPlay` | 게임 시작 시점 |
| `Tick` | `Event Tick` | 매 프레임 업데이트 |
| `EndPlay` | `Event EndPlay` | 파괴/종료 시점 |

---

### 1.4 Spawn 흐름 vs Load 흐름 차이

- **Spawn (런타임 생성):** `GetWorld()->SpawnActor<T>()`를 통해 생성됩니다. 생성자 → `PostActorCreated` → `OnConstruction` → `PostInitializeComponents` → `BeginPlay` 순으로 흐릅니다. 아이템을 드랍하거나 제작하여 월드에 생성할 때 이 흐름을 따릅니다.
- **Load (레벨 배치):** 레벨에 이미 배치된 액터는 레벨 로드 시 생성자(에디터 타임) → `PostLoad` → `OnConstruction` → `PostInitializeComponents` → `BeginPlay` 순으로 흐릅니다. `PostLoad` 단계에서 세이브 데이터로부터 아이템의 상태를 복구하는 로직을 넣기에 적합합니다.

---

### 1.5 Replicated Actor 라이프사이클 차이점

멀티플레이어 환경에서 서버와 클라이언트의 라이프사이클은 다르게 동작할 수 있습니다.
- **서버(Authority):** 모든 라이프사이클 함수가 정상적으로 호출되며, 게임의 진실(Truth)을 관리합니다.
- **클라이언트(Proxy):** 서버에서 스폰된 액터가 네트워크를 통해 복제되어 생성됩니다. 클라이언트에서는 `OnRep_` 함수들이 호출된 후 `BeginPlay`가 실행될 수 있습니다.
- **GAS 연동:** `PossessedBy`(서버)와 `OnRep_PlayerState`(클라이언트)에서 ASC의 `InitAbilityActorInfo`를 호출하는 패턴이 대표적입니다. 인벤토리 UI는 클라이언트의 `BeginPlay` 이후 데이터 복제가 완료된 시점에 업데이트되어야 합니다.

---

### 1.6 인벤토리/크래프팅에서 주의할 포인트 (아이템 Actor, PickupActor 등)

1. **PickupActor의 효율적 관리:** 아이템을 획득할 때마다 액터를 파괴(`Destroy`)하고 버릴 때마다 생성(`Spawn`)하는 것은 비용이 큽니다. 대규모 인벤토리 시스템에서는 액터 풀링(Actor Pooling)을 고려하거나, 시각적 요소만 숨기고 비활성화하는 방식을 고민해야 합니다.
2. **TSoftObjectPtr 활용:** `Constructor`에서 `StaticLoadObject`로 데이터 에셋을 하드 레퍼런싱하는 것은 로딩 시간을 길게 만듭니다. `TSoftObjectPtr`를 사용하여 필요한 시점에 비동기 로드하는 것이 메모리 관리 측면에서 훨씬 유리합니다.
3. **초기화 순서 보장:** 인벤토리 UI가 생성되는 시점(`BeginPlay`)에 인벤토리 컴포넌트의 데이터가 아직 서버로부터 복제되지 않았을 수 있습니다. 데이터가 준비되었음을 알리는 델리게이트를 활용하여 UI를 갱신해야 합니다.

---

### 1.7 코드 예제 (C++)

```cpp
// AItemPickup.h
UCLASS()
class MYPROJECT_API AItemPickup : public AActor
{
    GENERATED_BODY()

public:
    AItemPickup();

protected:
    // 에디터 피드백 및 실시간 외형 변경
    virtual void OnConstruction(const FTransform& Transform) override;
    
    // 컴포넌트 간 참조 연결 최적기
    virtual void PostInitializeComponents() override;
    
    // 게임 로직 및 GAS 초기화
    virtual void BeginPlay() override;
    
    // 자원 해제 및 정리
    virtual void EndPlay(const EEndPlayReason::Type EndPlayReason) override;

    UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Item Data")
    TSoftObjectPtr<UItemDataAsset> ItemData;

    UPROPERTY(VisibleAnywhere, Category = "Components")
    UStaticMeshComponent* ItemMesh;

    UPROPERTY(VisibleAnywhere, Category = "Components")
    class USphereComponent* CollisionSphere;
};

// AItemPickup.cpp
AItemPickup::AItemPickup()
{
    PrimaryActorTick.bCanEverTick = false; // 성능 최적화: 틱 비활성화

    CollisionSphere = CreateDefaultSubobject<USphereComponent>(TEXT("CollisionSphere"));
    RootComponent = CollisionSphere;
    CollisionSphere->SetSphereRadius(100.f);
    CollisionSphere->SetCollisionProfileName(TEXT("Trigger"));

    ItemMesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("ItemMesh"));
    ItemMesh->SetupAttachment(RootComponent);
    ItemMesh->SetCollisionEnabled(ECollisionEnabled::NoCollision);
}

void AItemPickup::OnConstruction(const FTransform& Transform)
{
    Super::OnConstruction(Transform);
    
    // 에디터에서 아이템 데이터를 할당하면 즉시 메쉬를 업데이트하여 시각적 확인 가능
    if (!ItemData.IsNull())
    {
        // 에디터 타임에는 동기 로드 허용
        if (UItemDataAsset* LoadedData = ItemData.LoadSynchronous())
        {
            ItemMesh->SetStaticMesh(LoadedData->PickupMesh);
        }
    }
}

void AItemPickup::PostInitializeComponents()
{
    Super::PostInitializeComponents();
    // 컴포넌트 간의 초기 연결 로직 수행
}

void AItemPickup::BeginPlay()
{
    Super::BeginPlay();
    
    // 런타임에는 비동기 로드 권장 (여기서는 예시를 위해 단순화)
    if (!ItemData.IsNull())
    {
        // 실제 프로젝트에서는 FStreamableManager를 사용한 비동기 로드 수행
    }
}

void AItemPickup::EndPlay(const EEndPlayReason::Type EndPlayReason)
{
    // 델리게이트 해제 및 타이머 정리
    Super::EndPlay(EndPlayReason);
}
```

> **[핵심 요약] Actor 라이프사이클**
> 1. **Constructor**는 액터의 뼈대를 정의하고, **OnConstruction**은 에디터 상의 살을 붙인다.
> 2. **PostInitializeComponents**는 내부 컴포넌트 참조를 연결하기 가장 안전한 장소이다.
> 3. **BeginPlay**에서 실제 게임 로직과 네트워크 동기화(GAS 연동 등)를 시작한다.
> 4. 인벤토리 아이템처럼 빈번한 생성/파괴가 일어나는 액터는 **TSoftObjectPtr**로 메모리 관리를 최적화하자.

---

## PART 2: UMG (Unreal Motion Graphics) 심화

인벤토리 UI는 수많은 아이템 슬롯, 실시간 데이터 업데이트, 그리고 복잡한 드래그 앤 드롭 상호작용이 필요한 시스템입니다. UMG의 심화 기능을 정확히 활용하여 성능 최적화와 유지보수성을 모두 확보해야 합니다.

### 2.1 UUserWidget 라이프사이클

위젯의 생명주기를 GAS의 개념과 매칭하여 이해하면 훨씬 구조적인 UI 설계가 가능합니다.

1.  **Initialize**: 클래스 기본 설정 및 위젯 트리 생성 시점. C++에서 `NativeOnInitialized`를 오버라이드하여 초기 1회성 설정을 수행합니다.
2.  **NativePreConstruct**: 디자인 타임 데이터 적용 및 에디터 미리보기. 블루프린트의 `PreConstruct`에 대응하며, 에디터에서 위젯의 외형을 미리 확인하고 싶을 때 사용합니다.
3.  **NativeConstruct**: 위젯이 화면에 나타나는 시점(Viewport에 추가될 때). 여기서 ASC의 델리게이트를 구독하고 초기 데이터를 로드합니다. (GAS의 `OnAvatarSet` 시점과 유사)
4.  **NativeTick**: 매 프레임 업데이트. 성능을 위해 가급적 사용을 금지하고 이벤트 기반으로 전환해야 합니다.
5.  **NativeDestruct**: 위젯이 화면에서 제거되거나 부모 위젯이 파괴될 때 호출됩니다. 델리게이트 구독 해제 및 타이머 정리를 수행합니다.

---

### 2.2 Widget 생성 & 관리

#### CreateWidget<T>() 사용법 (C++)
C++에서 위젯을 생성할 때 `Outer` 인자를 누구로 설정하느냐에 따라 위젯의 생명주기와 소유권이 결정됩니다.
- **PlayerController**: 해당 플레이어에게 귀속된 UI(인벤토리, HUD 등)에 사용합니다. 플레이어가 로그아웃하거나 컨트롤러가 파괴되면 위젯도 함께 정리됩니다.
- **GameInstance**: 레벨이 바뀌어도 유지되어야 하는 UI(로딩 화면, 전역 알림 등)에 사용합니다.

#### AddToViewport() vs AddToPlayerScreen() 차이
- **AddToViewport**: 0번 플레이어의 화면 전체를 기준으로 배치합니다. 싱글 플레이어 게임에 적합합니다.
- **AddToPlayerScreen**: 로컬 멀티플레이어(분할 화면) 환경에서 특정 플레이어의 할당된 화면 영역 내에 배치합니다. 멀티플레이어 지원 게임이라면 이 함수를 사용하는 습관을 들이는 것이 좋습니다.

#### RemoveFromParent() 호출 시 주의점
- 위젯을 화면에서 제거할 때 사용합니다. 하지만 이 함수를 호출한다고 해서 위젯이 즉시 메모리에서 해제되는 것은 아닙니다. 가비지 컬렉터가 수거할 수 있도록 모든 하드 레퍼런스를 해제해야 합니다.

#### Z-Order / Layer 관리
- `AddToViewport` 호출 시 `ZOrder` 매개변수를 통해 위젯의 겹침 순서를 제어할 수 있습니다. 숫자가 클수록 화면의 앞쪽에 표시됩니다.

---

### 2.3 레이아웃 시스템

#### Anchor & Alignment 완전 정리
- **Anchor (앵커):** 부모 컨테이너 내에서 위젯의 기준점을 정의합니다. 해상도가 변해도 위젯이 상대적으로 어디에 위치할지 결정합니다.
- **Alignment (정렬):** 위젯 자체의 피벗 포인트를 정의합니다. (0,0)은 왼쪽 상단, (0.5, 0.5)는 중앙, (1,1)은 오른쪽 하단입니다.

#### 주요 Panel 위젯
- **CanvasPanel:** 자유로운 위치와 크기 조절이 가능하지만, 성능 비용이 가장 큽니다.
- **VerticalBox / HorizontalBox:** 위젯을 수직/수평으로 자동 정렬합니다. 인벤토리 리스트나 메뉴 버튼 배열에 적합합니다.
- **GridPanel:** 행과 열을 사용하여 정교한 그리드 레이아웃을 만듭니다. 인벤토리 슬롯 배치에 자주 사용됩니다.
- **Overlay:** 위젯을 겹쳐서 배치할 때 사용합니다. 아이템 아이콘 위에 개수 텍스트를 올릴 때 필수적입니다.
- **SizeBox / ScaleBox:** 위젯의 크기를 강제하거나 비율을 유지하며 스케일을 조절할 때 사용합니다.

#### Slot 개념
- UMG에서 모든 위젯은 부모 패널의 **Slot**에 담깁니다. 부모 패널의 타입에 따라 `CanvasPanelSlot`, `GridSlot` 등으로 캐스팅하여 속성을 변경할 수 있습니다.

---

### 2.4 데이터 바인딩 방식 비교

#### Binding (폴링 방식) - 왜 피해야 하는가
- 디자인 뷰에서 프로퍼티 옆의 `Bind` 버튼을 눌러 함수를 연결하는 방식은 **매 프레임** 해당 함수를 호출합니다.
- **문제점:** 인벤토리에 100개의 슬롯이 있다면 매 프레임 수백 번의 함수 호출이 발생하여 CPU 자원을 낭비합니다.

#### Event Dispatcher 방식 - 권장
- 데이터 소스(예: `InventoryComponent`)에 델리게이트를 선언하고, 데이터가 변경될 때만 위젯의 업데이트 함수를 호출합니다.
- **GAS 연동:** `OnAttributeChange`나 `OnGameplayTagChange` 콜백을 사용하여 UI를 갱신하는 것이 정석입니다.

#### TObjectPtr, TWeakObjectPtr 안전한 참조 관리
- 위젯 멤버 변수에는 반드시 `UPROPERTY()`와 함께 `TObjectPtr`를 사용하여 GC로부터 보호해야 합니다.
- 반대로 위젯이 액터를 참조할 때는 액터가 먼저 파괴될 수 있으므로 `TWeakObjectPtr`을 사용하여 안전하게 접근해야 합니다.

---

### 2.5 ListView / TileView (인벤토리 핵심!)

수백 개의 아이템 슬롯을 개별 위젯으로 생성하면 메모리와 렌더링 성능에 치명적입니다. `ListView`와 `TileView`는 **위젯 풀링(Widget Pooling)** 기술을 사용하여 이 문제를 해결합니다.

#### IUserObjectListEntry 인터페이스
- 리스트뷰의 슬롯 위젯은 반드시 이 인터페이스를 구현해야 합니다. `NativeOnListItemObjectSet` 함수를 통해 전달된 데이터 객체를 기반으로 UI를 업데이트합니다.

#### 구현 패턴
1.  **Data Object**: 슬롯에 담길 순수 데이터 클래스 (`UObject` 상속).
2.  **Entry Widget**: 슬롯 하나를 담당할 실제 위젯.
3.  **TileView**: 인벤토리 그리드 형태를 위해 `UTileView`를 사용하고, `EntryWidgetClass`를 설정합니다.

```cpp
// Entry Widget C++ 구현
void UItemSlotWidget::NativeOnListItemObjectSet_Implementation(UObject* ListItemObject)
{
    if (UItemDataObject* ItemData = Cast<UItemDataObject>(ListItemObject))
    {
        ItemIcon->SetBrushFromTexture(ItemData->IconTexture);
        CountText->SetText(FText::AsNumber(ItemData->CurrentStack));
    }
}
```

---

### 2.6 Input Mode 처리

인벤토리를 열고 닫을 때 마우스 커서와 게임 입력을 제어하는 것은 매우 중요합니다.

- **SetInputMode_UIOnlyEx:** 모든 입력을 UI로만 보냅니다. 게임 캐릭터 조작이 완전히 차단됩니다.
- **SetInputMode_GameAndUIEx:** 게임 조작과 UI 조작을 동시에 허용합니다. 마우스로 화면을 클릭하면서 캐릭터를 이동시킬 수 있는 환경(예: 디아블로 스타일)에 적합합니다.
- **SetInputMode_GameOnlyEx:** 모든 입력을 게임으로 보냅니다. UI 상호작용이 차단됩니다.

**인벤토리 패턴:**
1. 인벤토리 열기 → `SetInputMode_GameAndUIEx` → `bShowMouseCursor = true`
2. 인벤토리 닫기 → `SetInputMode_GameOnlyEx` → `bShowMouseCursor = false`

---

### 2.7 Widget Animation

- **UWidgetAnimation:** 타임라인 기반으로 위젯의 속성(Opacity, Translation, Scale 등)을 변경합니다.
- **PlayAnimation:** C++에서 `PlayAnimation(MyAnimation)`을 호출하여 실행합니다.
- **인벤토리 활용:** 창이 열릴 때 슬라이드 인 효과, 아이템 획득 시 슬롯이 반짝이는 효과 등을 구현합니다.

> **[핵심 요약] UMG 심화**
> 1. **NativeConstruct**에서 델리게이트를 바인딩하고, **NativeDestruct**에서 반드시 해제하라.
> 2. 매 프레임 호출되는 **Binding** 대신 **이벤트 기반 업데이트**를 사용하라.
> 3. 대규모 리스트는 **ListView/TileView**의 위젯 풀링을 활용하여 성능을 최적화하라.
> 4. **Input Mode** 전환을 통해 사용자 경험(UX)을 매끄럽게 관리하라.

---

## PART 3: CommonUI 완전 정복

CommonUI는 언리얼 엔진 5에서 대규모 프로젝트의 복잡한 UI 요구사항과 멀티 플랫폼 대응을 위해 도입된 강력한 프레임워크입니다. 특히 키보드/마우스와 컨트롤러 입력을 동시에 지원해야 하는 인벤토리 시스템에서 그 진가를 발휘합니다.

### 3.1 CommonUI란? (일반 UMG와의 차이)

기존 UMG 시스템은 다음과 같은 고질적인 문제점이 있었습니다.
1.  **입력 포커스 관리의 어려움**: 어떤 위젯이 현재 입력을 받고 있는지 제어하기가 매우 까다로웠습니다.
2.  **플랫폼별 대응 파편화**: PC와 콘솔의 입력 로직을 별도로 작성해야 하는 경우가 많았습니다.
3.  **뒤로 가기(Back) 처리**: ESC 키나 패드의 B 버튼 처리를 모든 위젯마다 개별적으로 구현해야 했습니다.

CommonUI는 **Input Routing**, **Action Tag**, **Activatable Widget** 개념을 통해 이 모든 문제를 우아하게 해결합니다.

---

### 3.2 핵심 클래스 계층

- **UCommonUserWidget:** 모든 CommonUI 위젯의 기본 클래스입니다.
- **UCommonActivatableWidget (★가장 중요):** 인벤토리 창, 설정 메뉴 등 하나의 독립적인 화면 단위를 만들 때 사용합니다. 활성화/비활성화 상태를 가지며 입력 포커스를 자동으로 관리합니다.
- **UCommonButtonBase:** 스타일 시스템과 연동되는 고급 버튼 클래스입니다.
- **UCommonTextBlock:** 텍스트 스타일 데이터 에셋을 사용하여 전역적으로 폰트와 크기를 관리합니다.
- **UCommonBorder:** 배경 스타일을 데이터 에셋으로 관리합니다.

---

### 3.3 UCommonActivatableWidget 완전 분석

#### Activation 개념 (활성화/비활성화)
- 위젯이 화면에 보인다고 해서 반드시 활성화된 것은 아닙니다. `ActivateWidget()`을 호출해야 비로소 입력을 받을 수 있는 상태가 됩니다.
- **NativeOnActivated() / NativeOnDeactivated():** 활성화/비활성화 시점에 호출되는 콜백입니다. 여기서 입력 바인딩이나 초기화 로직을 수행합니다.

#### bAutoActivate 설정
- 위젯이 생성되자마자 자동으로 활성화될지 결정합니다. 보통 최상위 HUD 위젯은 true로 설정합니다.

#### IsActivated() 체크
- 현재 위젯이 활성 상태인지 확인하여 중복 입력을 방지할 수 있습니다.

---

### 3.4 UCommonActivatableWidgetStack / ContainerBase

화면 레이어를 스택 구조로 관리하는 컨테이너입니다.

- **Widget Stack 개념:** 인벤토리 화면 위에 "아이템 버리기 확인" 팝업을 띄울 때 사용합니다.
- **PushWidget<T>():** 새로운 위젯을 스택의 최상단에 올립니다. 이때 아래에 있던 위젯은 자동으로 비활성화(Deactivate)되어 입력을 받지 않게 됩니다.
- **PopWidget():** 최상단 위젯을 제거하고 이전 위젯을 다시 활성화합니다.

**크래프팅 흐름 예시:**
1. `CraftingListWidget` (레시피 목록) 활성화
2. 레시피 클릭 → `PushWidget<RecipeDetailWidget>` (재료 확인)
3. 제작 클릭 → `PushWidget<ConfirmCraftWidget>` (최종 확인)
4. 확인 완료 → `PopWidget`을 통해 이전 단계로 복귀

---

### 3.5 CommonUI Input System (★인벤토리 핵심)

CommonUI는 특정 키를 직접 바인딩하는 대신 추상화된 **UI Action Tag**를 사용합니다.

#### Action Tag 시스템
- `UI.Action.Back`, `UI.Action.Confirm` 등의 태그를 정의하고, 각 플랫폼별(PC, PS5, Xbox)로 실제 키를 매핑합니다.

#### RegisterUIActionBinding()
- 위젯 코드 내에서 특정 태그가 입력되었을 때 실행할 함수를 등록합니다.

```cpp
// CommonActivatableWidget에서 입력 액션 바인딩
void UInventoryScreen::NativeOnActivated()
{
    Super::NativeOnActivated();

    // 'Back' 액션(ESC 등)이 들어오면 위젯을 닫는 함수 호출
    FBindUIActionArgs Args(BackActionTag, FOnClicked::CreateUObject(this, &UInventoryScreen::HandleBackAction));
    Args.bDisplayInActionBar = true; // 하단 액션바에 버튼 힌트 표시
    
    RegisterUIActionBinding(Args);
}
```

#### Input Routing: Focus 기반 Input 처리
- CommonUI는 현재 포커스가 있는 위젯부터 시작하여 부모 위젯으로 입력을 전달(Routing)합니다. 이를 통해 "뒤로 가기" 버튼 하나로 모든 팝업을 순차적으로 닫는 기능을 쉽게 구현할 수 있습니다.

---

### 3.6 UCommonButtonBase 완전 분석

`UButton`은 스타일을 수정하려면 모든 버튼 인스턴스를 일일이 건드려야 했지만, `UCommonButtonBase`는 **CommonButtonStyle** 데이터 에셋을 통해 전역적으로 관리합니다.

- **ButtonStyle:** Normal, Hovered, Pressed, Disabled 상태의 폰트, 배경 이미지, 사운드를 데이터 에셋 하나로 정의합니다.
- **GetSelected() / SetIsSelected():** 버튼의 선택 상태를 관리합니다. 인벤토리에서 현재 선택된 아이템 슬롯을 표시할 때 유용합니다.
- **OnButtonClicked():** 클릭 이벤트를 처리합니다.

---

### 3.7 CommonUI + ListView 통합

CommonUI는 전용 리스트뷰인 `UCommonListView`를 제공합니다.
- **포커스 자동화:** 패드 입력 시 리스트 아이템 간의 포커스 이동이 매우 자연스럽습니다.
- **인벤토리 그리드:** `UTileView`와 결합하여 패드 조작이 완벽하게 지원되는 인벤토리 그리드를 구현할 수 있습니다.

---

### 3.8 CommonUISubsystem

- **UCommonUISubsystem 접근법:** `GetWorld()->GetSubsystem<UCommonUISubsystem>()`을 통해 접근합니다.
- **InputData 설정:** 현재 사용 중인 입력 장치(키보드/마우스 vs 컨트롤러)를 감지하고 그에 맞는 아이콘을 UI에 표시할 수 있게 도와줍니다.

> **[핵심 요약] CommonUI 완전 정복**
> 1. **UCommonActivatableWidget**은 독립적인 화면 단위의 표준이다.
> 2. **Widget Stack**을 사용하여 팝업과 화면 전환을 계층적으로 관리하라.
> 3. **Action Tag**를 통해 플랫폼에 독립적인 입력 시스템을 구축하라.
> 4. **CommonButtonStyle**을 활용하여 UI 디자인의 일관성을 유지하라.

