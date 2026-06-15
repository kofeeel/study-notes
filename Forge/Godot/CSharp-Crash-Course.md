---
tags: [csharp, godot, basics, crash-course, cpp, unreal]
---

# C# 핵심 속성 — C++/UE 개발자용

[[00-INDEX]] | STS2 코드를 읽고 좀슐랭을 만들기 위한 최소 C# 지식

> C++과 Unreal Engine 경험이 있는 개발자를 위한 크래시 코스.
> 이미 아는 개념은 UE 대응물을 짚고, C#에만 있는 것은 깊게 설명한다.

---

## 1. 타입 시스템 — C++과 거의 같지만 더 간결하다

### 기본 타입

```csharp
int hp = 100;            // int32와 동일 (항상 32비트)
float speed = 3.5f;      // float (f 접미사 필수! — C++도 같음)
double precise = 3.14;   // double
decimal gold = 99.9m;    // 128비트 고정밀 실수 (m 접미사) ← C++에 없음!
bool isDead = false;     // bool
string name = "Ironclad"; // std::string 대응, 하지만 불변(immutable)
char grade = 'A';        // char (유니코드 16비트, C++의 wchar_t에 가까움)
```

> [!note] STS2가 `decimal`을 쓰는 이유
> `float`는 `0.1 + 0.2 ≠ 0.3` 같은 부동소수점 오차가 있다.
> 카드 게임에서 데미지 1.5배 계산 시 오차가 쌓이면 버그가 되므로 `decimal`(정밀 연산)을 사용한다.
> C++에서는 보통 정수 연산으로 우회하지만, C#은 `decimal` 타입을 기본 제공한다.

### var — 타입 추론

```csharp
var hp = 100;              // 컴파일러가 int로 추론
var name = "Ironclad";     // string으로 추론
var list = new List<int>(); // List<int>로 추론

// C++의 auto와 동일한 개념!
// auto hp = 100;  ←→  var hp = 100;
// 타입은 컴파일 타임에 고정 (동적 타입 아님)
```

### 배열 & 컬렉션

```csharp
// 배열 — 크기 고정 (C++의 std::array / C 배열 대응)
int[] scores = new int[3];           // {0, 0, 0}
int[] scores2 = { 10, 20, 30 };     // 초기값 지정
string[] names = new[] { "A", "B" };

// List<T> — UE의 TArray<T> 대응 (가장 많이 사용!)
List<int> items = new List<int>();
items.Add(42);           // TArray::Add()
items.Remove(42);        // TArray::Remove()
items.Count;             // TArray::Num()  — Length 아님!

// Dictionary<K,V> — UE의 TMap<K,V> 대응
Dictionary<string, int> inventory = new Dictionary<string, int>();
inventory["sword"] = 1;
inventory.TryGetValue("sword", out int count); // TMap::Find()와 유사

// HashSet<T> — UE의 TSet<T> 대응
HashSet<string> visited = new HashSet<string>();
visited.Add("room1");
visited.Contains("room1"); // TSet::Contains()
```

> [!note] UE 컬렉션 → C# 매핑
> | UE (C++) | C# | 비고 |
> |----------|-----|------|
> | `TArray<T>` | `List<T>` | 가변 배열 |
> | `TMap<K,V>` | `Dictionary<K,V>` | 해시맵 |
> | `TSet<T>` | `HashSet<T>` | 고유 집합 |
> | `TOptional<T>` | `T?` (Nullable) | 있을 수도 없을 수도 |
> | `TSubclassOf<T>` | 제네릭 `where T : Base` | 타입 제약 |

### Nullable — null 안전성

```csharp
// C++의 nullptr 체크와 비슷하지만 컴파일러가 강제한다

// 값 타입은 기본적으로 null 불가
int hp = null;   // ❌ 컴파일 에러

// ?를 붙이면 null 허용 (C++의 std::optional<T>과 유사)
int? hp = null;  // ✅ Nullable<int>
string? name = null; // ✅

// null 관련 연산자 (C++에 없는 것들!)
string? name = GetName();
int length = name?.Length ?? 0;
// ?. = null이면 뒤를 실행 안 함 (null 전파) — UE의 IsValid() 체크를 연산자로
// ?? = 왼쪽이 null이면 오른쪽 값 사용

// STS2의 .csproj에 <Nullable>enable</Nullable> 설정
// → ? 없으면 null 불가, null 넣으면 경고/에러
// UE의 check(Ptr) / ensure(Ptr) 같은 역할을 컴파일 타임에 수행
```

---

## 2. 클래스 & 객체

### 기본 클래스

```csharp
public class Zombie
{
    // 필드 (내부 데이터) — C++ private 멤버 변수와 동일
    private int _hp;           // 관례: private 필드는 _camelCase (UE의 m_ 대신 _)
    private string _name;

    // ★ 프로퍼티 — C#의 핵심! C++에는 없는 개념
    public int Hp              // 겉보기에는 public 변수지만 실제로는 getter/setter
    {
        get => _hp;            // 읽기
        private set => _hp = value;  // 쓰기 (private = 내부에서만)
    }
    // C++에서는 GetHp() / SetHp()로 분리해야 했던 것을 하나로 통합

    public string Name => _name;  // 읽기 전용 (get만 있음)

    // 자동 프로퍼티 — 필드를 안 만들어도 됨 (가장 흔한 형태)
    public int Attack { get; set; }          // 읽기+쓰기
    public int Defense { get; private set; } // 읽기는 public, 쓰기는 private
    public bool IsDead => Hp <= 0;           // 계산 프로퍼티 (매번 계산)

    // 생성자 — C++과 동일 개념
    public Zombie(string name, int hp)
    {
        _name = name;
        _hp = hp;
    }

    // 메서드
    public void TakeDamage(int amount)
    {
        _hp = Math.Max(0, _hp - amount);  // FMath::Max() 대응
    }
}

// 사용
var zombie = new Zombie("Walker", 50);  // C++의 new와 다름! 스택/힙 구분 없음 (GC 관리)
zombie.TakeDamage(10);
Console.WriteLine(zombie.Hp);  // 40
```

> [!note] 프로퍼티 vs C++ Getter/Setter
> C++: `int32 GetHp() const { return Hp; }` + `void SetHp(int32 NewHp) { Hp = NewHp; }`
> C#: `public int Hp { get; private set; }` ← 한 줄로 끝!
> UE의 `UPROPERTY(BlueprintReadOnly)` = C#의 `{ get; private set; }`
> UE의 `UPROPERTY(BlueprintReadWrite)` = C#의 `{ get; set; }`

### 접근 제한자

```
public     — 어디서든 접근 가능
private    — 이 클래스 안에서만 (기본값)
protected  — 이 클래스 + 자식 클래스
internal   — 같은 프로젝트(어셈블리) 안에서만

// C++과 거의 동일! internal만 C++ 전용 없음
// UE의 UPROPERTY(meta=(AllowPrivateAccess)) 같은 트릭이 필요 없음
```

### static — 인스턴스 없이 사용

```csharp
// C++의 static 멤버와 동일 개념
public static class MathHelper  // static class = 인스턴스 생성 불가 (UE의 UBlueprintFunctionLibrary 대응)
{
    public static int Clamp(int value, int min, int max)
        => Math.Max(min, Math.Min(max, value));
    // FMath::Clamp() 대응
}

// 사용
int result = MathHelper.Clamp(150, 0, 100);  // 100

// 싱글톤 패턴 (STS2에서 매우 자주 사용)
// UE의 GetGameInstance<T>() / GetSubsystem<T>()와 유사한 역할
public class GameManager
{
    public static GameManager Instance { get; } = new GameManager();
    private GameManager() { }  // 외부 생성 금지
}
// 사용: GameManager.Instance.DoSomething();
```

---

## 3. 상속 & 다형성 — C++과 거의 같다

### 상속

```csharp
// 부모 클래스
public abstract class AbstractModel  // abstract = 직접 인스턴스화 불가 (= C++ 순수 가상 클래스)
{
    public string Id { get; }
    public bool IsMutable { get; private set; }

    // abstract = C++의 pure virtual (= 0)
    public abstract bool ShouldReceiveCombatHooks { get; }

    // virtual = C++과 완전 동일! 자식이 선택적 오버라이드
    public virtual Task AfterAttack() => Task.CompletedTask;
    public virtual decimal ModifyDamage() => 0m;
    public virtual bool ShouldDie() => true;
}

// 자식 클래스 — C++의 : public Base 대신 그냥 : Base
public class CardModel : AbstractModel
{
    public override bool ShouldReceiveCombatHooks => true;  // 구현 필수

    // virtual 메서드 오버라이드 (선택)
    public override async Task AfterAttack()
    {
        // 카드별 공격 후 효과
    }
}

// 손자 클래스
public sealed class StrikeIronclad : CardModel  // sealed = C++의 final
{
    protected override async Task OnPlay(...)
    {
        await DamageCmd.Attack(6).FromCard(this).Execute(ctx);
    }
}
```

### 핵심 키워드 — C++ 대응

```
C#                    C++                         비고
──────────────────   ──────────────────────────   ─────────────────
abstract class       순수 가상 함수 있는 클래스       인스턴스화 불가
abstract method      virtual void Foo() = 0;      자식이 반드시 구현
virtual method       virtual void Foo() { }       자식이 선택적 override
override             override (C++11)             동일!
sealed               final (C++11)                더 이상 상속 불가
base                 Super:: 또는 부모클래스명::     부모 호출
```

```csharp
// 예: 생성자에서 부모 호출
public StrikeIronclad()
    : base(1, CardType.Attack, CardRarity.Basic, TargetType.AnyEnemy)
//    ^^^^^ C++의 : CardModel(1, ...) 이니셜라이저 리스트와 동일
{ }
```

### 인터페이스

```csharp
// 인터페이스 = "이런 기능을 가져야 한다"는 계약
// C++에서는 순수 가상 함수만 있는 추상 클래스로 구현 (UE의 IInterface)
public interface IRunState  // 관례: I 접두사 (UE와 동일!)
{
    IReadOnlyList<Player> Players { get; }
    bool IsGameOver { get; }
    int TotalFloor { get; }
}

// 클래스가 인터페이스를 구현 — 다중 상속 가능!
public class RunState : IRunState, ICardScope  // C++은 다중 상속, C#은 인터페이스로만
{
    public IReadOnlyList<Player> Players => _players;
    public bool IsGameOver => Players.All(p => p.IsDead);
    public int TotalFloor => _totalFloor;
}

// 인터페이스 vs 추상 클래스
// 인터페이스: 구현 없음, 다중 상속 가능, "무엇을 할 수 있는가" (UE의 UInterface)
// 추상 클래스: 구현 포함 가능, 단일 상속, "무엇인가" (UE의 AActor, UObject)
```

---

## 4. 제네릭 — C++ 템플릿의 안전한 버전

```csharp
// C++의 template<typename T>와 같은 개념이지만 컴파일 타임이 아닌 런타임 지원
// UE에서 TArray<T>, TMap<K,V> 쓰던 것과 동일한 <T> 문법

// 제네릭 클래스
List<int>           // int를 담는 리스트 = TArray<int32>
List<CardModel>     // CardModel을 담는 리스트 = TArray<UCardModel*>
Dictionary<string, int>  // = TMap<FString, int32>

// 제네릭 메서드
public static T Get<T>() where T : AbstractModel
{
    return (T)_contentById[GetId(typeof(T))];
}

// 사용
CardModel strike = ModelDb.Get<StrikeIronclad>();
RelicModel relic = ModelDb.Get<BurningBlood>();

// where T : 제약 조건 — C++ concepts(C++20)과 유사하지만 더 간단
// where T : class          — T는 참조 타입이어야 함
// where T : struct         — T는 값 타입이어야 함
// where T : AbstractModel  — T는 AbstractModel이거나 자식 (= TSubclassOf<T>)
// where T : new()          — T는 기본 생성자가 있어야 함
```

> [!note] C++ template vs C# Generic
> - C++ 템플릿: 컴파일 타임에 코드 복사 생성 → 사용한 타입마다 별도 코드
> - C# 제네릭: 런타임에 하나의 코드 공유 (JIT이 최적화)
> - C# 제네릭이 컴파일 에러 메시지가 훨씬 읽기 쉽다!
> - `where T :` 제약 = UE의 `TSubclassOf<T>` 역할 (잘못된 타입 방지)

---

## 5. async/await — 비동기 프로그래밍

### STS2에서 가장 중요한 C# 기능

```csharp
// UE에서 Latent Action, FTimerDelegate, Delay 노드로 했던 것을
// C#에서는 async/await 키워드로 깔끔하게 처리한다

// async = "이 함수는 중간에 멈출 수 있음"
// await = "이 작업이 끝날 때까지 기다림"
// Task = "나중에 완료될 작업" (UE의 FLatentActionInfo 대응)

public async Task PlayCard(CardModel card)
{
    // 1. 애니메이션 재생하고 기다림 (UE: PlayMontageAndWait)
    await CreatureCmd.TriggerAnim(creature, "Attack", 0.5f);

    // 2. 데미지 적용하고 기다림
    await DamageCmd.Attack(10).FromCard(card).Execute(ctx);

    // 3. 0.3초 대기 (UE: FTimerHandle + Delay)
    await Cmd.Wait(0.3f);

    // 4. 후속 효과
    await Hook.AfterCardPlayed(combatState, cardPlay);
}

// 반환값이 있는 경우
public async Task<int> CalculateDamage()  // Task<T> = 비동기 + 반환값
{
    await SomeAsyncWork();
    return 42;
}

// 호출
int damage = await CalculateDamage();
```

### Task vs void

```csharp
// ✅ 올바른 패턴
public async Task DoWork() { ... }        // 기다릴 수 있음
public async Task<int> GetValue() { ... } // 값도 반환

// ⚠️ 특수 경우
public async void OnButtonClick() { ... } // 이벤트 핸들러에서만 사용
// async void는 예외 처리가 안 되므로 일반 코드에서 쓰지 말 것
// UE에서 fire-and-forget Delegate 바인딩과 비슷한 주의사항

// 아무것도 안 하는 async 메서드 (STS2 Hook 기본 구현)
public virtual Task AfterAttack() => Task.CompletedTask;
// CompletedTask = "이미 완료된 빈 Task" (await해도 즉시 통과)
```

### UE 비동기 패턴 비교

```
UE (C++)                                 C#
──────────────────────────────────────   ──────────────────────────
FTimerHandle + SetTimer(0.5f, ...)       await Cmd.Wait(0.5f)
Latent Action / Delay Node              await SomeAsyncMethod()
FOnMontageEnded Delegate bind            await PlayAnimation()
AsyncTask(ENamedThreads::GameThread)     Task.Run(() => ...)
TFuture<T> / TPromise<T>                Task<T> / TaskCompletionSource<T>
```

---

## 6. 이벤트 & 델리게이트 — UE Delegate의 C# 버전

### event — C#의 Observer 패턴

```csharp
public class Creature
{
    // C#의 event = UE의 DECLARE_DYNAMIC_MULTICAST_DELEGATE
    public event Action<int, int>? BlockChanged;     // (oldValue, newValue)
    public event Action<Creature>? Died;             // (죽은 크리처)
    public event Action? SimpleEvent;                // 매개변수 없음

    private int _block;
    public int Block
    {
        get => _block;
        set
        {
            int old = _block;
            _block = value;
            BlockChanged?.Invoke(old, _block);  // UE의 Broadcast()와 동일
            // ?. = 바인딩이 없으면(null) 호출 안 함
        }
    }
}

// 구독 (연결)
creature.Died += OnCreatureDied;       // UE의 AddDynamic() 대응
creature.Died -= OnCreatureDied;       // UE의 RemoveDynamic() 대응

void OnCreatureDied(Creature c)
{
    Console.WriteLine($"{c.Name} died!");
}
```

> [!note] UE Delegate vs C# event
> ```
> UE: DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FOnDied, ACrea*, Creature)
> C#: event Action<Creature>? Died;
>
> UE: OnDied.Broadcast(this);    →  C#: Died?.Invoke(this);
> UE: OnDied.AddDynamic(cb);     →  C#: Died += callback;
> UE: OnDied.RemoveDynamic(cb);  →  C#: Died -= callback;
> ```
> C#이 훨씬 간결하다. UE의 매크로 지옥이 없다!

### Godot Signal (C# 버전)

```csharp
// Godot Signal 선언 — UE의 UPROPERTY(BlueprintAssignable)과 유사
public partial class NGame : Control
{
    [Signal]
    public delegate void WindowChangeEventHandler();
    // 명명 규칙: 이름 + EventHandler
}

// UE의 UPROPERTY(BlueprintAssignable) FOnWindowChange OnWindowChange;
// 와 같은 역할 (에디터에서 연결 가능)
```

---

## 7. LINQ — 컬렉션 조작의 핵심

```csharp
// LINQ = 리스트/배열을 파이프라인으로 처리
// C++의 <algorithm> + ranges (C++20)와 유사하지만 훨씬 간결
// UE의 Algo:: 함수들 + FilterByPredicate 대응

List<CardModel> allCards = GetAllCards();

// Where = 필터링 (UE: FilterByPredicate / C++: std::copy_if)
var attacks = allCards.Where(c => c.Type == CardType.Attack);

// Select = 변환 (C++: std::transform)
var names = allCards.Select(c => c.Name);

// FirstOrDefault = 첫 번째 또는 null (UE: FindByPredicate)
var firstRare = allCards.FirstOrDefault(c => c.Rarity == CardRarity.Rare);

// Any = 하나라도 있는지 (UE: ContainsByPredicate / C++: std::any_of)
bool hasPoison = powers.Any(p => p is PoisonPower);

// All = 모두 만족하는지 (C++: std::all_of)
bool allDead = enemies.All(e => e.IsDead);

// Count = 개수 (C++: std::count_if)
int rareCount = allCards.Count(c => c.Rarity == CardRarity.Rare);

// Sum = 합계 (C++: std::accumulate)
int totalDamage = results.Sum(r => r.Damage);

// OrderBy / OrderByDescending = 정렬 (UE: Sort / C++: std::sort)
var sorted = allCards.OrderBy(c => c.EnergyCost);

// 체이닝 — C++20 ranges의 | 파이프와 유사
List<CardModel> result = allCards
    .Where(c => c.Type == CardType.Attack)   // 공격 카드만
    .OrderBy(c => c.EnergyCost)               // 비용 순 정렬
    .ToList();                                 // 결과 확정

// OfType<T> = 특정 타입만 필터링 (UE에서 Cast<T> 후 유효한 것만 모으는 패턴)
var cardPlays = entries.OfType<CardPlayStartedEntry>();

// Except = 차집합
var available = allCards.Except(blacklist);
```

### 람다 표현식 `=>`

```csharp
// C++의 람다 [](int x){ return x > 0; } 와 동일 개념이지만 훨씬 간결

// C++ : [](int x) -> bool { return x > 0; }
// C#  : (int x) => x > 0
// 더 줄이면: x => x > 0  (타입 추론)

// 매개변수 => 표현식
Func<int, bool> isPositive = x => x > 0;

// 매개변수 => { 여러 줄 } (C++ 람다 본문과 동일)
Action<int> printDouble = x =>
{
    int doubled = x * 2;
    Console.WriteLine(doubled);
};

// 메서드에서 사용
allCards.Where(c => c.Rarity == CardRarity.Rare);
// C++: std::copy_if(begin, end, back, [](auto& c){ return c.Rarity == Rare; });
// C#이 훨씬 간결!

// => 는 프로퍼티/메서드 본문에도 사용 (expression-bodied member)
public bool IsDead => Hp <= 0;           // 프로퍼티
public int GetDamage() => Attack * 2;     // 메서드 (한 줄짜리)
```

---

## 8. 열거형 & 구조체 & 레코드

### enum — C++과 거의 동일

```csharp
// C++의 enum class와 유사 (기본적으로 scoped)
public enum CardType
{
    Attack,   // 0
    Skill,    // 1
    Power,    // 2
    Curse,    // 3
    Status,   // 4
}

// 사용 — C++ enum class와 동일
CardType type = CardType.Attack;  // C++: ECardType::Attack
if (type == CardType.Attack) { ... }

// 비트 플래그 — C++의 ENUM_CLASS_FLAGS() 매크로 대응
[Flags]  // UE의 ENUM_CLASS_FLAGS(EValueProp) 역할
public enum ValueProp
{
    None       = 0,
    Unblockable = 1,
    Unpowered   = 2,
    Move        = 4,
}
// 사용: props.HasFlag(ValueProp.Unblockable)
// C++: EnumHasAnyFlags(Props, EValueProp::Unblockable)
```

### struct — 값 타입

```csharp
// C#의 struct = 값으로 복사됨 (C++의 struct와 비슷하지만 중요한 차이!)
// C#: class = 힙(참조), struct = 스택(값) — 이 구분이 C++과 다르다
// C++에서는 class/struct 차이가 거의 없지만 C#에서는 근본적으로 다름

public struct MapCoord  // UE의 FIntPoint, FVector2D 같은 값 타입
{
    public int col;
    public int row;

    public MapCoord(int col, int row)
    {
        this.col = col;
        this.row = row;
    }
}

// 값 타입이므로 대입하면 복사됨 (C++의 기본 동작과 동일!)
MapCoord a = new MapCoord(1, 2);
MapCoord b = a;  // 복사! a와 b는 독립적
b.col = 5;       // a.col은 여전히 1
// class였다면 참조 복사 → b.col 변경 시 a.col도 변경됨
```

> [!note] C#의 class vs struct = C++의 포인터 vs 값
> C# `class` = 항상 힙, 참조로 전달 (C++의 `new`로 만든 포인터와 같음)
> C# `struct` = 스택, 값으로 복사 (C++의 일반 변수와 같음)
> UE의 `USTRUCT()` = C#의 `struct`와 목적이 같음 (가벼운 데이터 묶음)

### record — 불변 데이터 (C# 9+)

```csharp
// C++에 없는 개념! 값 비교, ToString, 복사를 자동 생성
public record ModelId(string Category, string Entry)
{
    public override string ToString() => $"{Category}.{Entry}";
}

// 사용
var id1 = new ModelId("card", "strike-ironclad");
var id2 = new ModelId("card", "strike-ironclad");
id1 == id2;  // true! (값으로 비교)
// C++ 일반 클래스: 포인터 비교 → false
// C++ operator==() 오버로딩 한 것과 동일 효과를 자동으로
```

---

## 9. 패턴 매칭 & switch

```csharp
// switch 표현식 (C# 8+) — STS2에서 자주 사용
// C++의 switch보다 훨씬 강력 (타입 매칭, 조건 분기)
string banner = rarity switch
{
    CardRarity.Uncommon => "uncommon_mat.tres",
    CardRarity.Rare     => "rare_mat.tres",
    CardRarity.Curse    => "curse_mat.tres",
    _                   => "common_mat.tres",  // default
};

// is 패턴 매칭 — UE의 Cast<T>()와 유사하지만 더 간결
if (model is CardModel card)
{
    // card를 CardModel로 바로 사용 가능
    // UE: if (auto* Card = Cast<ACardModel>(Model)) { ... }
    Console.WriteLine(card.EnergyCost);
}

// switch + 타입 패턴 — UE의 여러 Cast 체인 대체
switch (room)
{
    case CombatRoom combat:    // Cast<ACombatRoom> 성공 시
        StartCombat(combat);
        break;
    case EventRoom eventRoom:  // Cast<AEventRoom> 성공 시
        StartEvent(eventRoom);
        break;
    case MerchantRoom:         // 타입만 체크, 변수 불필요
        OpenShop();
        break;
}
// UE에서 if-else Cast 체인 쓰던 것이 이렇게 깔끔해진다
```

---

## 10. Godot C# 전용 문법

### partial class (필수!)

```csharp
// Godot C#에서 Node 상속 클래스는 반드시 partial
// UE의 GENERATED_BODY() 매크로와 같은 역할!
// Godot 소스 제너레이터가 나머지 코드를 자동 생성함

public partial class NGame : Control  // ✅ GENERATED_BODY() 대응
{
    public override void _Ready() { }       // UE의 BeginPlay()
    public override void _Process(double delta) { }  // UE의 Tick(float DeltaTime)
}

// partial이 없으면 소스 제너레이터가 작동하지 않아 에러
public class NGame : Control  // ❌ = GENERATED_BODY() 빠뜨린 것과 같음
```

### [Export] — Inspector 노출

```csharp
// UE의 UPROPERTY(EditAnywhere) 대응!
public partial class MyNode : Node2D  // UE의 AActor 대응
{
    [Export] private int _speed = 100;           // UPROPERTY(EditAnywhere) int32 Speed;
    [Export] private PackedScene _bulletScene;    // UPROPERTY(EditAnywhere) TSubclassOf<AActor>
    [Export] private NodePath _targetPath;        // UPROPERTY(EditAnywhere) TSoftObjectPtr
}

// UE                                    Godot C#
// UPROPERTY(EditAnywhere)              [Export]
// UPROPERTY(VisibleAnywhere)           [Export] ... { get; private set; }
// UPROPERTY(BlueprintReadOnly)         프로퍼티 + private set
// UPROPERTY(Category="Combat")         [ExportGroup("Combat")]
```

### Signal 연결

```csharp
// UE의 Delegate 바인딩과 같은 패턴
// UE: Button->OnClicked.AddDynamic(this, &AMyActor::OnButtonClicked);
// Godot C#:
button.Pressed += OnButtonPressed;

// 또는 Callable 사용 (동적 연결)
button.Connect("pressed", Callable.From(OnButtonPressed));

// 타이머 await (STS2의 Cmd.Wait 방식)
// UE: GetWorld()->GetTimerManager().SetTimer(Handle, Delegate, 1.5f, false);
// Godot C#:
SceneTree tree = (SceneTree)Engine.GetMainLoop();
SceneTreeTimer timer = tree.CreateTimer(1.5f);
await ToSignal(timer, SceneTreeTimer.SignalName.Timeout);
```

### 노드 접근

```csharp
// UE에서 컴포넌트 가져오는 것과 유사
// UE: GetComponentByClass<UHealthComponent>()
// Godot C#:
GetNode<Label>("ChildNode");      // 이름으로 자식 노드 접근

// UE: UPROPERTY(meta=(BindWidget)) 대응 — 고유 이름 접근
GetNode<Label>("%UniqueNode");    // % = 고유 이름 (씬 내 유일)

// null-safe 버전 (UE의 FindComponentByClass에 대응)
GetNodeOrNull<Label>("MaybeExists");
```

---

## 11. 자주 나오는 C# 문법 요약

### 문자열 보간

```csharp
string name = "Ironclad";
int hp = 80;
string msg = $"Player {name} has {hp} HP";  // $ 접두사 + {변수}
// 결과: "Player Ironclad has 80 HP"

// UE 대응: FString::Printf(TEXT("Player %s has %d HP"), *Name, Hp);
// C#이 훨씬 편하다!
```

### using — 자원 자동 정리

```csharp
// C++의 RAII와 동일 개념! (스마트 포인터의 소멸자 호출과 같음)
using (var file = new StreamWriter("save.json"))
{
    file.Write(json);
}  // 여기서 자동으로 file.Dispose() 호출 — C++의 소멸자와 같은 역할

// async 버전 (STS2의 AttackContext)
await using var ctx = await AttackContext.CreateAsync(state, card);
// 블록 끝에서 DisposeAsync() 자동 호출
```

### out 매개변수

```csharp
// C++의 참조 매개변수와 비슷 (int& outValue)
// 하지만 C#은 "반드시 값을 할당해야 함"을 컴파일러가 강제

public bool TryGetValue(string key, out int value)
{
    // 성공하면 true + value에 값 대입
    // 실패하면 false + value = default
}

// 사용 — UE의 TMap::Find()처럼 성공/실패를 bool로 반환
if (dict.TryGetValue("sword", out int count))
{
    Console.WriteLine($"보유: {count}개");
}

// STS2 예시
if (!card.CanPlay(out UnplayableReason reason, out _))
{
    ShowReason(reason);
}
// out _ = "값은 필요 없고 결과만 필요" (C++17 structured binding의 std::ignore와 유사)
```

### 삼항 연산자 & null 합체

```csharp
// 삼항 — C++과 동일
string status = hp > 0 ? "Alive" : "Dead";

// ?? (null이면 대체값) — C++에 없는 편의 기능
string name = playerName ?? "Unknown";

// ??= (null이면 대입)
_instance ??= new GameManager();
// C++: if (!Instance) Instance = new GameManager();

// ?. (null이면 뒤를 실행 안 함)
int? length = name?.Length;
// C++: int length = name ? name->Length() : std::nullopt;
// UE: IsValid(Obj) ? Obj->GetLength() : 0;
```

### IEnumerable & yield return

```csharp
// C++의 이터레이터(begin/end)를 극도로 간편하게 만든 것
// yield return = 값을 하나씩 생성하는 lazy 시퀀스 (C++20 coroutines co_yield와 유사)

public IEnumerable<AbstractModel> IterateHookListeners()
{
    foreach (var power in creature.Powers)
        yield return power;      // 하나씩 내보냄

    foreach (var relic in player.Relics)
        yield return relic;

    foreach (var card in allCards)
    {
        yield return card;
        if (card.Enchantment != null)
            yield return card.Enchantment;  // 조건부로도 내보냄
    }
}

// 호출 — foreach(range-based for)로 소비
foreach (AbstractModel model in combatState.IterateHookListeners())
{
    await model.AfterAttack(command);
}
// C++20: for (auto& model : IterateHookListeners()) { ... }
```

---

## 12. STS2 코드를 읽을 때 자주 만나는 패턴

### 패턴 1: 자동 프로퍼티 + init

```csharp
public CombatHistory History { get; }           // 읽기 전용 (생성자에서만 설정)
public RunRngSet Rng { get; init; }             // init = 초기화 시에만 설정 가능
public static CombatManager Instance { get; } = new CombatManager(); // 정적 초기화
// UE 대응: const / UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
```

### 패턴 2: required + init (C# 11)

```csharp
public class Mod
{
    public required string path;  // 반드시 초기화해야 함
}
var mod = new Mod { path = "/mods/my_mod" };  // 지정 초기화 (C++20 designated init과 유사)
```

### 패턴 3: 컬렉션 읽기 전용 래핑

```csharp
private readonly List<Creature> _enemies = new();  // 내부: 수정 가능
public IReadOnlyList<Creature> Enemies => _enemies; // 외부: 읽기만
// 외부에서 Add/Remove 불가, 내부에서만 조작
// UE: private TArray + public const TArray& GetEnemies() const 패턴과 동일
```

### 패턴 4: 어트리뷰트(Attribute)

```csharp
[GenerateSubtypes]    // 소스 제너레이터에게 지시 ← UE의 UCLASS() 매크로 역할
[Export]              // Godot Inspector 노출 ← UPROPERTY(EditAnywhere)
[Signal]              // Godot Signal 선언 ← DECLARE_DYNAMIC_MULTICAST_DELEGATE
[JsonPropertyName("id")]  // JSON 직렬화 필드명
[Flags]               // 비트 플래그 enum ← ENUM_CLASS_FLAGS()

// 어트리뷰트 = "이 코드에 대한 메타데이터"
// UE의 UPROPERTY/UFUNCTION/UCLASS 매크로가 C#에서는 [] 어트리뷰트로 표현됨
// 런타임이나 빌드 도구가 읽어서 특별한 처리를 함 (= UHT와 비슷한 역할)
```

### 패턴 5: 확장 메서드

```csharp
// 기존 클래스에 메서드를 추가하는 것처럼 보이게 하는 기법
// C++에는 없는 개념! (가장 가까운 것: 자유 함수, 하지만 호출 문법이 다름)
public static class ListExtensions
{
    public static void StableShuffle<T>(this List<T> list, Rng rng)
    //                                  ^^^^ this 키워드 = 확장 대상
    {
        // Fisher-Yates 셔플
    }
}

// 사용 — 마치 List에 원래 있던 메서드처럼
myList.StableShuffle(rng);
// C++에서는: Algo::StableShuffle(MyArray, Rng); ← 자유 함수로 호출
// C#에서는: myList.StableShuffle(rng);          ← 멤버 함수처럼 호출
```

---

## 13. 빠른 대조표 — C++/UE ↔ C#/Godot

| C++ / Unreal Engine | C# / Godot | 비고 |
|---------------------|------------|------|
| `auto x = 5;` | `var x = 5;` | 타입 추론 |
| `void DoStuff() { }` | `void DoStuff() { }` | 동일! |
| `int32 Do() { return 0; }` | `int Do() { return 0; }` | 거의 동일 |
| `if (x > 0) { }` | `if (x > 0) { }` | 동일! |
| `for (int i=0; i<10; i++)` | `for (int i=0; i<10; i++)` | 동일! |
| `for (auto& item : list)` | `foreach (var item in list)` | range-based for |
| `UE_LOG(LogTemp, Log, TEXT("hi"))` | `GD.Print("hi");` | 로그 출력 |
| `UPROPERTY(EditAnywhere)` | `[Export]` | Inspector 노출 |
| `UCLASS()` + `GENERATED_BODY()` | `public partial class` | 코드 생성 필수 |
| `: public AActor` | `: Node` / `: Control` | 상속 (콜론) |
| `DECLARE_DYNAMIC_MULTICAST_DELEGATE` | `[Signal] delegate void ...EventHandler()` | 이벤트 |
| `OnDied.Broadcast(this)` | `EmitSignal(SignalName.Died)` | 이벤트 발화 |
| `OnDied.AddDynamic(this, &Cls::Cb)` | `Died += Callback;` | 이벤트 구독 |
| `virtual void Foo() override` | `public override void Foo()` | 오버라이드 |
| `virtual void Foo() = 0;` | `public abstract void Foo();` | 순수 가상 |
| `class final` | `sealed class` | 상속 금지 |
| `nullptr` | `null` | 널 |
| `Cast<AEnemy>(Actor)` | `actor is Enemy enemy` | 타입 캐스팅 |
| `TSubclassOf<AActor>` | `where T : Node` | 타입 제약 |
| `TArray<T>` | `List<T>` | 가변 배열 |
| `TMap<K,V>` | `Dictionary<K,V>` | 해시맵 |
| `TSet<T>` | `HashSet<T>` | 집합 |
| `FString` | `string` | 문자열 |
| `FName` | `StringName` | 경량 문자열 식별자 |
| `FText` | `string` (TR 시스템) | 로컬라이즈 문자열 |
| `TOptional<T>` | `T?` (Nullable) | 옵셔널 |
| `[](auto& x){ return x > 0; }` | `x => x > 0` | 람다 |
| `Algo::FilterByPredicate` | `.Where(x => ...)` | LINQ 필터 |
| `Algo::Transform` | `.Select(x => ...)` | LINQ 변환 |
| `FTimerHandle` / `SetTimer` | `await Task.Delay()` | 비동기 대기 |
| `BeginPlay()` | `_Ready()` | 초기화 |
| `Tick(float DeltaTime)` | `_Process(double delta)` | 매 프레임 |
| `UActorComponent` | 자식 `Node` | 컴포넌트 |
| `FMath::Clamp()` | `Mathf.Clamp()` | 수학 함수 |

---

## 14. 연습: STS2 코드 읽어보기

다음 코드를 한 줄씩 해석해보자 (UE 경험으로 대응하며 읽기):

```csharp
public sealed class StrikeIronclad : CardModel
// sealed(=final): 더 이상 상속 불가 / CardModel 상속

{
    protected override HashSet<CardTag> CanonicalTags
        => new HashSet<CardTag> { CardTag.Strike };
    // protected: 자식만 접근 / override: 부모 virtual 재정의
    // => : expression-bodied / HashSet = TSet<FGameplayTag>

    protected override IEnumerable<DynamicVar> CanonicalVars
        => new[] { new DamageVar(6m, ValueProp.Move) };
    // new[] : 배열 생성 / 6m : decimal 리터럴 (128비트 정밀 실수)
    // DamageVar(6m, ...) : 기본 데미지 6

    public StrikeIronclad()
        : base(1, CardType.Attack, CardRarity.Basic, TargetType.AnyEnemy)
    { }
    // base(...) : 부모 생성자 호출 — C++의 이니셜라이저 리스트 : CardModel(1, ...) 와 동일
    // 에너지 비용 1, 공격 카드, 기본 등급, 적 단일 타겟

    protected override async Task OnPlay(PlayerChoiceContext ctx, CardPlay cardPlay)
    {
        ArgumentNullException.ThrowIfNull(cardPlay.Target, "cardPlay.Target");
        // 타겟이 null이면 즉시 예외 — UE의 check(cardPlay.Target) 와 동일

        await DamageCmd.Attack(DynamicVars.Damage.BaseValue)
        // DamageCmd.Attack(6) → 빌더 패턴으로 커맨드 생성
            .FromCard(this)
            // 이 카드에서 발사 (공격자 = 카드 소유자)
            .Targeting(cardPlay.Target)
            // 타겟 지정
            .WithHitFx("vfx/vfx_attack_slash")
            // 히트 이펙트
            .Execute(ctx);
            // 실행 (async → await로 완료 대기)
    }

    protected override void OnUpgrade()
    {
        DynamicVars.Damage.UpgradeValueBy(3m);
        // 업그레이드: 데미지 6 → 9
    }
}
```

---

## 15. C++ → C# 마인드 전환 체크리스트

C++ 개발자가 C#으로 넘어올 때 꼭 기억할 것:

| 이것은 잊어라 | 이것을 기억하라 |
|--------------|---------------|
| `new`/`delete` 수동 메모리 관리 | GC가 알아서 해제 (Dispose 패턴만 필요시 사용) |
| 헤더/소스 분리 (.h/.cpp) | 한 파일에 다 쓴다 (.cs) |
| `#include` 인클루드 | `using` 네임스페이스 import |
| 포인터 `*`, 참조 `&` | 모든 class는 참조 타입 (자동 포인터) |
| `std::unique_ptr`, `std::shared_ptr` | 필요 없음 (GC가 관리) |
| `operator==()` 오버로딩 | `record`가 자동 생성, 또는 `IEquatable<T>` |
| 컴파일 시간 수십 초~분 | 거의 즉시 (핫 리로드 지원) |
| GENERATED_BODY() 매크로 | `partial` 키워드 |
| `UPROPERTY()` / `UFUNCTION()` 매크로 | `[Export]` / `[Signal]` 어트리뷰트 |
| UHT (Unreal Header Tool) | .NET Source Generator |
| `TArray`, `TMap`, `TSet` | `List`, `Dictionary`, `HashSet` |
| `FString::Printf()` | `$"문자열 보간 {var}"` |

---

## 다음 단계

이 정도면 STS2 코드를 **읽는** 데 충분하다. 직접 **작성**할 때는:

1. **Godot 4 C# 프로젝트 생성**해서 `partial class`, `[Export]`, `_Ready()` 연습
   - UE의 BeginPlay, UPROPERTY에 대응하므로 익숙할 것
2. **간단한 카드 모델** 만들어보기 (AbstractModel 상속, virtual 오버라이드)
   - UE에서 AActor 상속하고 virtual 오버라이드하던 것과 동일
3. **async/await**로 타이머 대기 연습
   - UE의 FTimerHandle 대신 `await ToSignal(timer, ...)` 사용

```csharp
// 첫 연습: 이 코드를 Godot에서 실행해보자
// UE에서 AActor + Tick 만들던 것과 동일!
public partial class MyFirstNode : Node2D  // AActor 대응
{
    [Export] private int _speed = 200;  // UPROPERTY(EditAnywhere)

    public override void _Ready()  // BeginPlay()
    {
        GD.Print($"Speed is {_speed}");  // UE_LOG
    }

    public override void _Process(double delta)  // Tick(float DeltaTime)
    {
        Position += new Vector2((float)(_speed * delta), 0);
        // SetActorLocation(GetActorLocation() + FVector(Speed * DeltaTime, 0, 0));
    }
}
```

---

*관련: [[01-Godot-Engine-Basics]] | [[03-Architecture-Overview]] | [[05-Model-System]]*
