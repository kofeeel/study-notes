# Effective Modern C++ -- Ch.1~2 Type Deduction 마스터 퀴즈

> **범위**: Item 1~6 (Chapter 1: Deducing Types + Chapter 2: auto)
> **난이도**: ★ Easy --> ★★★ Hard 로 점진적 상승
> **사용법**: 각 문제를 풀고 접힌 답안을 확인하세요. 페이지 참조(📕)로 원서를 복습하세요.

---

## 📖 핵심 개념 요약

### Item 1: Template type deduction (📕 p.9-17)

> [!tip] 한 줄 핵심
> **ParamType의 형태**(by-value / `T&` / `T&&`)가 T 추론을 결정한다. expr이 아니라 ParamType이 주인공.

```cpp
template<typename T> void f(T  param);   // by-value: const/& 벗김
template<typename T> void g(T& param);   // by-ref:   & 벗김, const 보존

const std::vector<Mesh>& meshes = GetMeshes();
f(meshes);   // T = vector<Mesh>       → ❌ 전체 복사!
g(meshes);   // T = const vector<Mesh> → ✅ reference 유지
```

- **왜?** UE5 `TArray` / Godot `PackedStringArray`를 템플릿에 넘길 때 의도치 않은 복사·const 손실이 프레임 드랍의 숨은 원인.
- **트랩:** `T&` 파라미터에 C-array 넘기면 포인터 decay가 **안** 일어남 → `const char (&)[7]` 같은 타입이 잡힘 (Q1 함정 참조).

---

### Item 2: auto type deduction (📕 p.18-22)

> [!tip] 한 줄 핵심
> `auto` ≈ template deduction. **단, `{}` 하나가 다르다** — `auto`는 `initializer_list`로 추론, template은 실패.

```cpp
auto a = 27;       // int
auto b(27);        // int
auto c = {27};     // std::initializer_list<int>  ← 함정!
auto d{27};        // initializer_list<int> (C++11/14 기준)

template<typename T> void f(T x);
f({27});           // ❌ 컴파일 에러: T 추론 불가
```

- **버그 시나리오:** `auto config = {defaultValue};` — 개발자는 `int` 하나를 담으려 했지만 실제로는 `initializer_list<int>`. `int` 받는 함수에 넘기면 컴파일 에러.
- C++14 lambda `return {1,2,3};`도 template 규칙 적용되어 **추론 실패**.

---

### Item 3: decltype (📕 p.23-29)

> [!tip] 한 줄 핵심
> `decltype`은 표현식을 **있는 그대로** 반영. 이름 → 선언 타입. 이름 아닌 lvalue → **무조건 `T&`**. 괄호 하나가 생사를 가른다.

```cpp
int x = 0;
decltype(x)   a;   // int    ← 이름
decltype((x)) b;   // int&   ← (x)는 lvalue 표현식

decltype(auto) f1() { int x=0; return  x;  }  // → int   안전
decltype(auto) f2() { int x=0; return (x); }  // → int&  💥 dangling!
```

- **왜 중요?** perfect forwarding 래퍼·제네릭 getter에서 reference/value 보존이 핵심.
- **최악의 버그:** `(x)` 괄호 하나가 dangling reference 생성. Release 빌드에서만 터짐.

---

### Item 4: View deduced types (📕 p.30-35)

> [!tip] 한 줄 핵심
> **`typeid().name()`은 거짓말쟁이.** const/reference 벗긴 결과를 보여줌. 정확한 타입은 IDE hover, 컴파일 에러 트릭, 또는 Boost.TypeIndex.

```cpp
const int& cr = x;
std::cout << typeid(cr).name();  // "i" (그냥 int) ← const&  날아감!

// ✅ 정확한 타입 확인 트릭 — 컴파일 에러 메시지에 타입 노출
template<typename T> class TD;   // 선언만, 정의 없음
TD<decltype(cr)> probe;          // error: 'TD<const int&>' incomplete
```

- **트랩:** "타입이 `const int&`인 줄 알았는데 `int`로 찍혀서" 잘못된 진단 위에 잘못된 수정을 쌓는 디버깅 지옥.

---

### Item 5: Prefer auto to explicit types (📕 p.37-42)

> [!tip] 한 줄 핵심
> `auto`는 **타입을 틀릴 수 없게** 만든다. 미초기화 차단 + 묵시적 변환 차단 + closure 최적화 무료.

```cpp
std::unordered_map<std::string, int> m;

// ❌ 실제 원소 타입은 pair<const std::string, int>
//    매 반복마다 임시 pair 복사 생성!
for (const std::pair<std::string, int>& p : m) { /*...*/ }

// ✅ auto가 정확한 타입을 잡아줌 — 복사 없음
for (const auto& p : m) { /*...*/ }
```

- **부수 효과:** `int x;` (UB) → `auto x;` (컴파일 에러). 미초기화 원천 차단.
- **closure:** `std::function`은 heap 할당 + 간접 호출. `auto`는 closure 실제 타입을 그대로 → 더 작고 더 빠름.

---

### Item 6: Explicitly typed initializer idiom (📕 p.43-48)

> [!tip] 한 줄 핵심
> `auto`가 **proxy 객체**를 잡으면 dangling 위험. `static_cast`로 진짜 타입을 강제하라.

```cpp
std::vector<bool> GetFeatures();   // proxy 반환 API

// ❌ vector<bool>::reference 가 잡힘
//    임시 벡터가 ; 끝에서 소멸 → flag는 dangling
auto flag = GetFeatures()[5];
if (flag) { /* 💥 UB — Debug에선 우연히 동작, Release에서 크래시 */ }

// ✅ explicitly typed initializer idiom
auto flag = static_cast<bool>(GetFeatures()[5]);
```

- **proxy 후보:** `vector<bool>::reference`, expression templates (Eigen, Blaze), `bitset::reference` 등.
- **판단법:** API가 "보이지 않는 임시 객체를 거쳐 값을 돌려주는가?" → YES면 `static_cast` 강제.

---

## 🧠 OX 퀴즈 (10문제) -- ★ Easy

> [!tip] 규칙
> O(맞다) 또는 X(틀리다)로 답한 뒤, 접힌 해설을 확인하세요.

### Q1.
> [!question] template type deduction에서 `T&` 파라미터에 `const int&` 타입의 인자를 전달하면, T는 `const int&`로 추론된다.

> [!note]- 정답 확인 (클릭)
> **X** — reference-ness는 무시된다. T는 `const int`로 추론되고, ParamType은 `const int&`가 된다. (📕 p.11)

### Q2.
> [!question] universal reference (`T&&`) 파라미터에 lvalue를 전달하면, T는 lvalue reference로 추론된다. 이것은 template type deduction에서 T가 reference로 추론되는 **유일한** 경우다.

> [!note]- 정답 확인 (클릭)
> **O** — T가 reference로 추론되는 유일한 상황이다. lvalue를 전달하면 T는 `int&` 같은 lvalue reference로 추론된다. (📕 p.13)

### Q3.
> [!question] by-value 파라미터에 `const int`를 전달하면, T는 `const int`로 추론된다.

> [!note]- 정답 확인 (클릭)
> **X** — by-value 전달은 복사본을 만든다. 원본의 `const`는 복사본과 무관하므로 T는 `int`로 추론된다. `volatile`도 마찬가지로 무시된다. (📕 p.14)

### Q4.
> [!question] `auto x = {27};`에서 x의 타입은 `int`이다.

> [!note]- 정답 확인 (클릭)
> **X** — braced initializer를 사용하면 `auto`는 `std::initializer_list<int>`로 추론한다. `auto x(27);`이면 `int`이다. 이것이 `auto`와 template type deduction의 **유일한 차이**. (📕 p.21)

### Q5.
> [!question] `decltype(x)`와 `decltype((x))`는 항상 같은 타입을 반환한다.

> [!note]- 정답 확인 (클릭)
> **X** — `int x = 0;`일 때 `decltype(x)`는 `int`이지만, `decltype((x))`는 `int&`이다. `(x)`는 이름이 아닌 lvalue 표현식이므로 decltype은 `T&`를 반환한다. (📕 p.28-29)

### Q6.
> [!question] `typeid(x).name()`은 const와 reference를 포함한 정확한 타입 정보를 반환한다.

> [!note]- 정답 확인 (클릭)
> **X** — `std::type_info::name`은 타입을 by-value 파라미터처럼 취급하므로 reference와 const가 제거된다. 정확한 타입에는 Boost.TypeIndex를 사용해야 한다. (📕 p.33)

### Q7.
> [!question] `auto` 변수는 반드시 초기화해야 하므로, 미초기화 버그를 원천 방지할 수 있다.

> [!note]- 정답 확인 (클릭)
> **O** — `auto x;`는 컴파일 에러. 이것이 `auto`를 선호해야 하는 이유 중 하나다. (📕 p.38)

### Q8.
> [!question] closure를 `auto`로 저장하는 것과 `std::function`으로 저장하는 것은 메모리 사용량과 성능이 동일하다.

> [!note]- 정답 확인 (클릭)
> **X** — `std::function`은 고정 크기 + heap 할당 가능성 + 간접 호출 오버헤드가 있다. `auto`는 closure의 실제 타입을 사용하므로 더 작고 빠르다. (📕 p.39)

### Q9.
> [!question] `std::vector<bool>::operator[]`는 `bool&`를 반환한다.

> [!note]- 정답 확인 (클릭)
> **X** — `std::vector<bool>`은 비트 단위 저장을 사용하고, C++은 비트에 대한 reference를 허용하지 않는다. 따라서 `std::vector<bool>::reference`라는 proxy 객체를 반환한다. (📕 p.43)

### Q10.
> [!question] C++14에서 함수 return type에 사용된 `auto`는 auto type deduction 규칙을 따른다.

> [!note]- 정답 확인 (클릭)
> **X** — C++14의 함수 return type `auto`와 lambda parameter `auto`는 **template type deduction** 규칙을 따른다. 따라서 braced initializer를 반환하면 컴파일 에러가 발생한다. (📕 p.22)

---

## 💻 코드 분석 퀴즈 (5문제) -- ★★ Medium

> [!tip] 규칙
> 각 코드에서 추론되는 타입을 맞혀보세요.

### Q1. Template Type Deduction -- Case 1 & 3

```cpp
template<typename T>
void f(T param);        // by-value

template<typename T>
void g(T& param);       // by-reference

int x = 42;
const int cx = x;
const int& rx = cx;

f(rx);   // (A) T = ?   param의 타입 = ?
g(rx);   // (B) T = ?   param의 타입 = ?
```

> [!note]- 정답 확인 (클릭)
> **(A)** `f(rx)`: by-value이므로 reference 무시, const도 무시 --> **T = `int`**, param = `int` (📕 p.14)
> **(B)** `g(rx)`: reference 무시 후 패턴 매칭 --> **T = `const int`**, param = `const int&` (📕 p.11)

### Q2. Universal Reference

```cpp
template<typename T>
void f(T&& param);

int x = 27;
const int cx = x;

f(x);    // (A) T = ?   param의 타입 = ?
f(27);   // (B) T = ?   param의 타입 = ?
f(cx);   // (C) T = ?   param의 타입 = ?
```

> [!note]- 정답 확인 (클릭)
> **(A)** x는 lvalue --> **T = `int&`**, param = `int&` (reference collapsing: `int& &&` --> `int&`)
> **(B)** 27은 rvalue --> Case 1 적용: **T = `int`**, param = `int&&`
> **(C)** cx는 lvalue --> **T = `const int&`**, param = `const int&` (📕 p.13)

### Q3. auto와 braced initializer

```cpp
auto a = 27;          // (A) 타입?
auto b(27);           // (B) 타입?
auto c = {27};        // (C) 타입?
auto d{27};           // (D) 타입?
auto e = {1, 2, 3.0}; // (E) 결과?
```

> [!note]- 정답 확인 (클릭)
> **(A)** `int` -- Case 3, 평범한 추론 (📕 p.21)
> **(B)** `int` -- 괄호 초기화, 동일
> **(C)** `std::initializer_list<int>` -- braced initializer 특수 규칙!
> **(D)** `std::initializer_list<int>` -- 책 기준 (C++17부터는 `int`으로 변경됨, 원서는 C++11/14 기준)
> **(E)** **컴파일 에러** -- `int`와 `double`이 혼재하여 `std::initializer_list<T>`의 T를 추론 불가 (📕 p.21)

### Q4. decltype vs auto

```cpp
Widget w;
const Widget& cw = w;

auto myWidget1 = cw;            // (A) 타입?
decltype(auto) myWidget2 = cw;  // (B) 타입?
```

> [!note]- 정답 확인 (클릭)
> **(A)** `Widget` -- `auto`는 template type deduction 규칙을 따르므로 reference 무시, const 무시 (by-value Case 3) (📕 p.26)
> **(B)** `const Widget&` -- `decltype`은 표현식의 타입을 그대로 보존 (📕 p.26)

### Q5. Proxy Class Trap

```cpp
std::vector<bool> features(const Widget& w);
Widget w;

auto highPriority = features(w)[5];    // (A) 타입? 안전한가?
bool highPriority2 = features(w)[5];   // (B) 타입? 안전한가?
```

> [!note]- 정답 확인 (클릭)
> **(A)** `std::vector<bool>::reference` -- **Undefined Behavior!** `features(w)`가 반환한 임시 `vector<bool>`이 문장 끝에서 소멸하면 dangling pointer 발생 (📕 p.44)
> **(B)** `bool` -- 안전. proxy 객체가 `bool`로 implicit conversion되면서 임시 벡터가 아직 살아있는 동안 값을 추출한다 (📕 p.44)

---

## 🔥 함정 문제 (3문제) -- ★★★ Hard

> [!tip] 규칙
> 흔히 혼동하는 edge case들입니다. 신중하게 생각하세요!

### Q1. 배열과 template -- 숨겨진 차이

```cpp
template<typename T> void f(T param);    // by-value
template<typename T> void g(T& param);   // by-reference

const char name[] = "Briggs";  // 타입: const char[7]

f(name);   // (A) T = ?
g(name);   // (B) T = ?
```

> [!note]- 정답 확인 (클릭)
> **(A)** **T = `const char*`** -- by-value 파라미터에 배열을 전달하면 array-to-pointer decay 발생 (📕 p.15-16)
> **(B)** **T = `const char [7]`** -- by-reference 파라미터에 배열을 전달하면 **배열 타입이 보존**된다! param의 타입은 `const char (&)[7]` (📕 p.16)
>
> 이 차이를 활용하면 배열 크기를 컴파일 타임에 알아낼 수 있다:
> ```cpp
> template<typename T, std::size_t N>
> constexpr std::size_t arraySize(T (&)[N]) noexcept { return N; }
> ```

### Q2. decltype(auto)의 괄호 함정

```cpp
decltype(auto) f1() {
    int x = 0;
    return x;      // (A) 반환 타입?
}

decltype(auto) f2() {
    int x = 0;
    return (x);    // (B) 반환 타입?
}
```

> [!note]- 정답 확인 (클릭)
> **(A)** **`int`** -- `x`는 이름이므로 `decltype(x)` = `int` (📕 p.29)
> **(B)** **`int&`** -- `(x)`는 이름이 아닌 lvalue 표현식이므로 `decltype((x))` = `int&`. **지역 변수에 대한 reference를 반환하므로 undefined behavior!** (📕 p.29)
>
> 괄호 하나가 프로그램을 망칠 수 있다. `decltype(auto)`를 사용할 때는 return 문에 **절대로** 불필요한 괄호를 넣지 마라.

### Q3. const pointer의 by-value 전달

```cpp
template<typename T>
void f(T param);

const char* const ptr = "Hello";

f(ptr);   // T = ?   param의 타입 = ?
```

> [!note]- 정답 확인 (클릭)
> **T = `const char*`**, param = `const char*` (📕 p.15)
>
> 핵심: by-value 전달 시 **포인터 자체의 const**(`*` 오른쪽의 `const`)는 무시되지만, **포인터가 가리키는 대상의 const**(`*` 왼쪽의 `const`)는 보존된다.
>
> - `const char* const` --> 복사하면 포인터 자체의 불변성은 사라지지만, 가리키는 문자열의 불변성은 유지
> - 이는 `const int`를 by-value로 전달하면 `int`가 되는 것과 같은 원리 (top-level const 제거)

---

## 📝 빈칸 채우기 (5문제) -- ★★ Medium

### Q1.
> [!question] Template type deduction에서 ParamType이 reference나 pointer일 때, 먼저 expr의 \_\_\_\_\_\_을(를) 무시한 뒤 패턴 매칭한다.

> [!note]- 정답 확인 (클릭)
> **reference-ness (참조성)** (📕 p.11)

### Q2.
> [!question] `auto`와 template type deduction의 유일한 차이점은 \_\_\_\_\_\_ 처리 방식이다. `auto`는 이것을 `std::initializer_list`로 추론하지만, template은 추론에 실패한다.

> [!note]- 정답 확인 (클릭)
> **braced initializer (중괄호 초기화자)** (📕 p.21-22)

### Q3.
> [!question] `decltype`은 이름(name)에 적용하면 선언된 타입을 그대로 반환하지만, 이름이 아닌 lvalue 표현식에 적용하면 항상 \_\_\_\_\_\_ 타입을 반환한다.

> [!note]- 정답 확인 (클릭)
> **T& (lvalue reference)** (📕 p.28)

### Q4.
> [!question] `std::vector<bool>::operator[]`는 `bool&` 대신 \_\_\_\_\_\_라는 proxy 객체를 반환한다. 이것을 `auto`로 받으면 dangling pointer 위험이 있다.

> [!note]- 정답 확인 (클릭)
> **`std::vector<bool>::reference`** (📕 p.43)

### Q5.
> [!question] proxy class로 인해 `auto`가 원하지 않는 타입을 추론할 때, \_\_\_\_\_\_ idiom을 사용하여 `auto val = static_cast<TargetType>(expr);` 형태로 원하는 타입을 강제할 수 있다.

> [!note]- 정답 확인 (클릭)
> **explicitly typed initializer (명시적 타입 초기화자)** (📕 p.46-47)

---

## 🏆 점수 체크

| 섹션 | 만점 | 내 점수 |
|---|---|---|
| OX 퀴즈 (10문제) | 10 | /10 |
| 코드 분석 (5문제, 소문제 포함) | 15 | /15 |
| 함정 문제 (3문제) | 9 | /9 |
| 빈칸 채우기 (5문제) | 5 | /5 |
| **합계** | **39** | **/39** |

| 등급 | 점수 |
|---|---|
| S -- Type Deduction Master | 35-39 |
| A -- 실전 투입 가능 | 28-34 |
| B -- 한 번 더 읽자 | 20-27 |
| C -- Item 1부터 다시... | 0-19 |

---

> [!note] 다음 단계
> - Chapter 3 (Items 7-17): Moving to Modern C++ --> `QUIZ-EMC-Ch3-ModernCpp.md`
> - 틀린 문제의 📕 페이지를 다시 읽고, 직접 코드를 컴파일해서 확인하면 기억에 남는다.
> - `decltype(auto)`와 proxy class는 실무에서 반드시 마주치므로 완벽히 이해할 것.
