# Effective Modern C++ - Chapter 5 & 6 Study Guide & Quiz

## Rvalue References, Move Semantics, Perfect Forwarding & Lambda Expressions (Items 23-34)

> **Move semantics는 Modern C++에서 가장 어려운 파트입니다.**
> 하지만 걱정 마세요 -- 이 가이드를 끝까지 풀면 "아, 이게 이거였구나!" 하는 순간이 옵니다.
> 포기하지 말고 한 문제씩! 💪

---

# 📖 핵심 개념 요약

## Chapter 5: Rvalue References, Move Semantics, and Perfect Forwarding

### Item 23: Understand `std::move` and `std::forward` (📕 p.158-163)

> [!question] 왜 알아야 하나?
> `std::move`는 실제로 아무것도 move하지 않고, `std::forward`도 forward하지 않는다. 둘 다 캐스팅일 뿐이다. 이름에 속으면 `const` 객체에 `std::move`를 걸어놓고 "왜 복사가 되지?" 하고 삽질한다. 이 구분 없이는 move semantics 전체가 마법처럼 느껴진다.

> [!danger] 모르면 이런 버그가 난다
> `const std::string`에 `std::move`를 적용하고 "이동했으니 빠르겠지" 하고 넘어간다. 실제로는 copy constructor가 호출되어 성능 최적화가 전혀 안 되는데, 프로파일러를 돌리기 전까지 아무도 모른다. 대규모 문자열 처리에서 예상 대비 2배 느린 원인이 이것.

> [!tip] 한 줄 핵심
> `std::move`는 "rvalue로 캐스팅해줘"이고, `std::forward`는 "원래 뭐였는지에 따라 캐스팅해줘"이다 -- 둘 다 런타임 코드를 1바이트도 생성하지 않는다.

### Item 24: Distinguish universal references from rvalue references (📕 p.164-167)

> [!question] 왜 알아야 하나?
> `T&&`가 항상 rvalue reference인 건 아니다. template이나 auto에서는 universal reference다. 이 구분을 못 하면 perfect forwarding 전체가 이해 안 된다. `std::vector<T>::push_back(T&&)`은 rvalue reference인데 `emplace_back(Args&&...)`은 universal reference인 이유를 모르면, 언제 `std::move`를 쓰고 언제 `std::forward`를 써야 하는지 판단이 안 된다.

> [!danger] 모르면 이런 버그가 난다
> Universal reference에 `std::move`를 써버린다. lvalue를 전달한 호출자의 원본 객체가 moved-from 상태가 되어 이후 코드에서 빈 문자열, 빈 벡터를 읽게 된다. 디버깅하면 "분명히 값을 넣었는데 왜 비어있지?" -- 원인을 찾는 데 반나절.

> [!tip] 한 줄 핵심
> `T&&` 형태 + type deduction 발생 = universal reference. 둘 중 하나라도 빠지면 그냥 rvalue reference.

### Item 25: Use `std::move` on rvalue references, `std::forward` on universal references (📕 p.168-176)

> [!question] 왜 알아야 하나?
> 잘못된 조합을 쓰면 두 가지 재앙이 온다: (1) universal reference에 `std::move` -> 호출자의 lvalue가 파괴됨, (2) return 문에서 `std::move` -> RVO가 깨져서 불필요한 move 발생. 올바른 규칙을 모르면 "최적화한다"면서 오히려 성능을 떨어뜨리거나 버그를 만든다.

> [!danger] 모르면 이런 버그가 난다
> `return std::move(localWidget);`으로 "최적화"했는데, 컴파일러의 RVO(copy elision)가 비활성화된다. move construction이 강제되어 오히려 느려진다. 게다가 RVO가 불가능할 때도 컴파일러가 자동으로 move를 시도하므로, 명시적 `std::move`는 어떤 경우에도 이득이 없다.

> [!tip] 한 줄 핵심
> rvalue reference -> `std::move`, universal reference -> `std::forward`, return 로컬 변수 -> 아무것도 하지 마라.

### Item 26: Avoid overloading on universal references (📕 p.177-182)

> [!question] 왜 알아야 하나?
> Universal reference를 받는 함수는 overload resolution의 블랙홀이다. 거의 모든 타입에 대해 exact match를 생성하므로, 옆에 있는 다른 overload를 전부 집어삼킨다. 이걸 모르고 "편의 overload 하나 추가해야지" 하면 기존에 잘 되던 코드가 깨진다.

> [!danger] 모르면 이런 버그가 난다
> `Person(T&& name)`과 `Person(int idx)`를 나란히 두었다. `short s = 3; Person p(s);`를 호출하면 `int` overload가 아니라 perfect forwarding constructor가 `short`에 exact match로 선택된다. `std::string(short)`은 없으므로 컴파일 에러. 더 무서운 건 `Person p2(p1);` -- non-const lvalue 복사가 copy constructor 대신 forwarding constructor를 호출하는 것.

> [!tip] 한 줄 핵심
> Universal reference overload는 overload resolution을 장악한다 -- 다른 overload와 공존이 거의 불가능하다.

### Item 27: Alternatives to overloading on universal references (📕 p.183-196)

> [!question] 왜 알아야 하나?
> Item 26에서 "하지 마라"고 했는데, 현실에서는 universal reference의 성능이 필요한 경우가 있다. 그때 대안 없이 포기하거나, 위험을 감수하고 overload하는 대신, tag dispatch/SFINAE/`enable_if` 같은 안전한 우회로를 알아야 한다.

> [!danger] 모르면 이런 버그가 난다
> 대안을 모르니까 그냥 universal reference overload를 쓴다. 처음엔 잘 되는데, 팀원이 derived class를 만들거나 implicit conversion이 있는 타입을 전달하는 순간 컴파일 에러 또는 잘못된 overload 호출이 터진다. 코드 리뷰에서 "이거 왜 이렇게 복잡해요?" -> 처음부터 `enable_if`로 constrain했으면 깔끔했을 것.

> [!tip] 한 줄 핵심
> Tag dispatch로 분기하거나, `std::enable_if`로 템플릿을 비활성화하거나, 포기하고 pass-by-value를 써라.

### Item 28: Understand reference collapsing (📕 p.197-202)

> [!question] 왜 알아야 하나?
> Universal reference가 어떻게 lvalue/rvalue를 모두 받을 수 있는지, `std::forward`가 어떻게 조건부 캐스팅을 수행하는지 -- 그 밑바닥에 reference collapsing이 있다. 이걸 모르면 universal reference와 `std::forward`가 마법으로 남고, 디버깅할 때 template instantiation 에러 메시지를 해독할 수 없다.

> [!danger] 모르면 이런 버그가 난다
> Template 코드에서 `T& &&`가 뭔지 몰라 컴파일 에러를 보고 멘붕. "왜 reference to reference가 나오지?" 하면서 삽질한다. Reference collapsing 규칙 ("하나라도 lvalue면 lvalue")을 알면 3초 만에 해석되는 에러인데, 모르면 30분 허비한다.

> [!tip] 한 줄 핵심
> 둘 중 하나라도 lvalue reference이면 결과는 `T&`. 둘 다 rvalue reference일 때만 `T&&`. "lvalue is sticky."

### Item 29: Assume that move operations are not present, not cheap, and not used (📕 p.203-206)

> [!question] 왜 알아야 하나?
> "move니까 빠르겠지"는 위험한 가정이다. `std::array`의 move는 O(N)이고, SSO 적용된 짧은 `std::string`의 move는 copy와 동일하며, `noexcept` 없는 move는 STL 컨테이너가 아예 사용하지 않는다. Generic code에서 move를 공짜로 가정하면 성능 예측이 완전히 빗나간다.

> [!danger] 모르면 이런 버그가 난다
> Move constructor에 `noexcept`를 빼먹는다. `std::vector::push_back` 재할당 시 move 대신 copy가 호출되어, 10만 개 객체 벡터의 재할당이 예상보다 10배 느려진다. 프로파일러를 돌려야 발견되는데, `noexcept` 하나면 해결되는 문제.

> [!tip] 한 줄 핵심
> Generic code에서는 move가 없고, 비싸고, 안 쓰인다고 가정하라. 구체적 타입을 알 때만 기대하라.

### Item 30: Familiarize yourself with perfect forwarding failure cases (📕 p.207-214)

> [!question] 왜 알아야 하나?
> Perfect forwarding은 만능이 아니다. 5가지 경우에서 실패한다. 이걸 모르면 "왜 직접 호출하면 되는데 forwarding하면 안 되지?" 하고 한참 헤맨다. 특히 braced initializer `{1,2,3}`과 0/NULL은 일상적으로 마주치는 함정이다.

> [!danger] 모르면 이런 버그가 난다
> `fwd({1, 2, 3})`이 컴파일 안 되는 이유를 모르고, "template이 broken이다"라며 overload를 추가하기 시작한다. 실제로는 `auto il = {1,2,3}; fwd(il);`로 한 줄이면 해결. 또는 `fwd(0)`으로 null pointer를 전달하려다 integral type으로 추론되어 엉뚱한 overload가 호출된다.

> [!tip] 한 줄 핵심
> Braced init, 0/NULL, static const 멤버, overloaded 함수명, bitfield -- 이 5가지는 perfect forwarding이 실패한다. 외워라.

---

## Chapter 6: Lambda Expressions

### Item 31: Avoid default capture modes (📕 p.216-223)

> [!question] 왜 알아야 하나?
> `[=]`가 안전해 보이지만, 멤버 함수 안에서 쓰면 `this`를 캡처한다. 객체가 먼저 소멸되면 dangling pointer. 게임에서 비동기 콜백에 람다 쓸 때 가장 흔한 크래시 원인이다. `[&]`는 말할 것도 없이 지역 변수 수명 문제. default capture는 "뭘 캡처했는지" 숨기므로 위험을 눈에 안 보이게 만든다.

> [!danger] 모르면 이런 버그가 난다
> 멤버 함수에서 `[=]`로 람다를 만들어 비동기 콜백에 등록한다. "값 복사니까 안전하지" 했는데, 실제로는 `this`가 캡처된다. 객체가 소멸된 후 콜백이 실행되면 `this->member` 접근에서 crash. Release 빌드에서만 간헐적으로 터져서 재현도 어렵다.

> [!tip] 한 줄 핵심
> `[=]`는 멤버 변수를 복사하지 않는다 -- `this` 포인터를 복사할 뿐이다. 항상 명시적으로 캡처하라.

### Item 32: Use init capture to move objects into closures (📕 p.224-228)

> [!question] 왜 알아야 하나?
> `std::unique_ptr`이나 대용량 `std::vector`를 lambda에 넣고 싶은데, C++11 캡처는 복사만 된다. Move-only 타입은 아예 캡처 불가. C++14 init capture를 모르면 `std::bind` workaround를 써야 하는데, 가독성이 처참하다.

> [!danger] 모르면 이런 버그가 난다
> 대용량 데이터를 lambda에서 쓰려고 `[data]`로 캡처한다. 수백 MB 벡터가 통째로 복사되어 메모리 사용량이 2배, 캡처 시점에 수 초 지연. `[data = std::move(data)]`로 바꾸면 포인터 교환 한 번으로 끝나는 것을, 복사 비용을 "어쩔 수 없다"고 넘긴다.

> [!tip] 한 줄 핵심
> C++14 init capture `[x = std::move(x)]`로 move 캡처. `=` 좌측은 closure 멤버, 우측은 바깥 scope.

### Item 33: Use `decltype` on `auto&&` parameters to `std::forward` them (📕 p.229-231)

> [!question] 왜 알아야 하나?
> Generic lambda의 `auto&&`는 universal reference인데, template parameter `T`가 없어서 `std::forward<T>`를 쓸 수 없다. `decltype(param)`이 그 역할을 대신한다는 걸 모르면, generic lambda에서 perfect forwarding을 포기하고 불필요한 복사를 감수하게 된다.

> [!danger] 모르면 이런 버그가 난다
> `[](auto x) { return f(x); }`로 wrapper lambda를 만든다. rvalue를 전달해도 `x`는 lvalue이므로 copy가 발생한다. move-only 타입이면 아예 컴파일 에러. `auto&&` + `std::forward<decltype(x)>(x)` 패턴 하나면 해결인데, 모르니까 `std::function`이나 함수 포인터로 우회한다.

> [!tip] 한 줄 핵심
> Generic lambda에서 perfect forwarding: `[](auto&& x) { return f(std::forward<decltype(x)>(x)); }`

### Item 34: Prefer lambdas to `std::bind` (📕 p.232-240)

> [!question] 왜 알아야 하나?
> `std::bind`는 인자 평가 시점, overload 해석, 중첩 bind 동작 등에서 직관과 다르게 동작한다. Lambda는 읽는 그대로 동작하고 inline 최적화도 된다. C++14 이후로 `std::bind`를 써야 할 이유가 문자 그대로 없다.

> [!danger] 모르면 이런 버그가 난다
> `std::bind(setAlarm, steady_clock::now() + 1h, _1, 30s)` -- 시간 계산이 bind 호출 시점에 평가되어 저장된다. 10분 후 호출하면 알람이 50분 후에 울린다. Lambda였다면 호출 시점에 평가되어 정확히 1시간 후. 이 차이를 모르면 타이밍 버그를 만들고, `std::bind`의 평가 시점 규칙을 문서에서 찾느라 시간 낭비.

> [!tip] 한 줄 핵심
> C++14 이후 `std::bind`를 쓸 이유는 전혀 없다. Lambda가 더 읽기 쉽고, 더 빠르고, 더 안전하다.

---

# 🧠 OX 퀴즈 (12문제)

> [!tip] **풀이 방법**
> 각 문장이 맞으면 O, 틀리면 X를 선택하세요. 답과 해설은 접힌 블록 안에 있습니다.

---

### Q1. `std::move`는 실제로 객체를 이동시키는 함수이다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- `std::move`는 런타임에 아무것도 하지 않는다. 단지 인자를 **무조건적으로 rvalue로 캐스팅**하는 함수 템플릿일 뿐이다. 실제 이동은 move constructor나 move assignment operator가 수행한다. (📕 p.158)

---

### Q2. `const` 객체에 `std::move`를 적용하면 move constructor가 호출된다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- `const std::string`에 `std::move`를 적용하면 `const std::string&&`가 되는데, move constructor는 `string&&` (non-const)를 받으므로 매칭되지 않는다. 대신 `const string&`를 받는 **copy constructor가 호출**된다. (📕 p.160)

---

### Q3. `auto&&`로 선언된 변수는 universal reference이다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **O** -- `auto&&`는 type deduction이 발생하고 `T&&` 형태를 가지므로 universal reference의 두 조건을 모두 만족한다. lvalue로 초기화하면 lvalue reference가 되고, rvalue로 초기화하면 rvalue reference가 된다. (📕 p.164-167)

---

### Q4. 함수 파라미터가 rvalue reference 타입이더라도, 그 파라미터 자체는 lvalue이다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **O** -- 모든 함수 파라미터는 이름이 있으므로 lvalue이다. `void f(Widget&& w)`에서 `w`의 **타입**은 rvalue reference이지만, `w`라는 표현식 자체는 **lvalue**이다. 이것이 `std::move`가 필요한 근본 이유이다. (📕 p.158)

---

### Q5. `std::vector<Widget>`의 move는 항상 O(1)이다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **O** -- `std::vector`는 heap에 데이터를 저장하고 내부적으로 포인터만 가지고 있으므로, move는 포인터 교환으로 **O(1)**에 수행된다. 반면 `std::array`는 데이터를 객체 내부에 저장하므로 O(N)이다. (📕 p.203-204)

---

### Q6. `std::array<Widget, 10000>`의 move는 포인터 교환이므로 O(1)이다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- `std::array`는 데이터를 **객체 내부에 직접 저장**하므로 (heap이 아님) move 시 각 원소를 개별적으로 move해야 한다. 따라서 **O(N) linear time**이 소요된다. (📕 p.204)

---

### Q7. Lambda의 default by-value capture `[=]`를 사용하면 dangling pointer 문제가 발생할 수 없다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- `[=]`는 `this` 포인터를 **값으로 복사**한다. 만약 lambda를 포함하는 closure가 해당 객체보다 오래 살면, `this`가 가리키는 객체가 파괴된 후에도 `this->member`에 접근하게 되어 **dangling pointer** 문제가 발생한다. (📕 p.219-221)

---

### Q8. Reference collapsing에서 `T& &&`는 `T&&`가 된다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- Reference collapsing 규칙: **둘 중 하나라도 lvalue reference이면 결과는 lvalue reference(`T&`)**이다. `T& &&`에서 첫 번째가 lvalue reference이므로 결과는 `T&`이다. `T&&`가 되려면 둘 다 rvalue reference(`T&& &&`)여야 한다. (📕 p.199)

---

### Q9. Perfect forwarding은 braced initializer `{1, 2, 3}`을 성공적으로 전달할 수 있다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- Braced initializer를 function template에 전달하면 "non-deduced context"가 되어 type deduction이 실패한다. **workaround**: `auto il = {1, 2, 3};`으로 먼저 `std::initializer_list<int>`로 받은 뒤 forwarding 함수에 전달한다. (📕 p.208)

---

### Q10. C++14 generic lambda에서 perfect forwarding을 하려면 `std::forward<auto>(param)`이라고 쓴다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- `auto`는 타입이 아니라 placeholder이므로 template argument로 사용할 수 없다. 올바른 방법은 `std::forward<decltype(param)>(param)`이다. `decltype(param)`이 lvalue/rvalue에 따라 적절한 reference 타입을 반환한다. (📕 p.229-231)

---

### Q11. `std::bind`에 전달된 인자는 bind object가 호출될 때 평가된다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- `std::bind`의 인자는 **`std::bind` 호출 시점에 평가**되어 bind object 내부에 저장된다. 예: `std::bind(setAlarm, steady_clock::now() + 1h, _1, 30s)`에서 시간 계산은 bind 호출 시 수행된다. Lambda에서는 호출 시 평가되므로 동작이 다르다. (📕 p.233-234)

---

### Q12. 로컬 변수를 return할 때 `std::move`를 적용하면 성능이 향상된다.

> [!question] O / X ?

> [!note]- 정답 확인 (클릭)
> **X** -- 오히려 **RVO(Return Value Optimization)를 방해**한다. `return w;`라고 쓰면 컴파일러가 copy elision을 수행할 수 있지만, `return std::move(w);`는 반환값이 로컬 변수가 아닌 reference가 되어 RVO 조건을 충족하지 못한다. 또한 RVO가 적용되지 않더라도 컴파일러는 자동으로 move를 시도한다. (📕 p.174-176)

---

# 💻 코드 분석 퀴즈 (8문제)

## CA-1: `std::move` vs `std::forward` -- 어떤 걸 써야 하나?

```cpp
class Widget {
public:
    template<typename T>
    void setName(T&& newName) {
        name_ = std::move(newName);  // (A)
    }
private:
    std::string name_;
};

std::string n = "Hello";
Widget w;
w.setName(n);
// n의 상태는?
```

> [!question] 이 코드의 문제점은 무엇이고, 어떻게 수정해야 하는가?

> [!note]- 정답 확인 (클릭)
> **문제**: `newName`은 **universal reference**인데 `std::move`를 사용했다. `n`(lvalue)이 전달되면 `std::move`에 의해 `n`의 값이 `name_`으로 **이동**되어 `n`은 **unspecified 상태**가 된다.
>
> **수정**: `std::move` 대신 `std::forward<T>`를 사용해야 한다.
> ```cpp
> name_ = std::forward<T>(newName);
> ```
> 이렇게 하면 lvalue가 전달되면 copy, rvalue가 전달되면 move가 수행된다. (📕 p.169)

---

## CA-2: 이 코드에서 어떤 constructor가 호출되는가?

```cpp
class Annotation {
public:
    explicit Annotation(const std::string text)
        : value(std::move(text)) {}
private:
    std::string value;
};
```

> [!question] `std::move(text)`는 move constructor를 호출하는가, copy constructor를 호출하는가?

> [!note]- 정답 확인 (클릭)
> **Copy constructor가 호출된다.** `text`가 `const std::string`이므로 `std::move(text)`의 결과는 `const std::string&&`이다. `std::string`의 move constructor는 `string&&` (non-const)를 받으므로 매칭되지 않는다. `const string&`를 받는 **copy constructor가 호출**된다. Move하고 싶다면 `const`를 제거해야 한다. (📕 p.159-160)

---

## CA-3: Universal reference인가, rvalue reference인가?

```cpp
// (1)
void f(Widget&& param);

// (2)
template<typename T>
void g(T&& param);

// (3)
template<typename T>
void h(std::vector<T>&& param);

// (4)
auto&& var = someExpression;

// (5)
template<typename T>
void k(const T&& param);
```

> [!question] 각각 universal reference인가, rvalue reference인가?

> [!note]- 정답 확인 (클릭)
> | # | 종류 | 이유 |
> |---|------|------|
> | (1) | **Rvalue reference** | 구체적 타입 `Widget&&`, type deduction 없음 |
> | (2) | **Universal reference** | `T&&` 형태 + type deduction 발생 |
> | (3) | **Rvalue reference** | `std::vector<T>&&`는 정확히 `T&&` 형태가 아님 |
> | (4) | **Universal reference** | `auto&&`는 type deduction + `T&&` 형태 |
> | (5) | **Rvalue reference** | `const T&&`는 `const`가 붙어 universal reference가 아님 |
>
> 핵심: **정확히 `T&&` 형태**이고 **type deduction이 발생**해야 universal reference이다. (📕 p.164-167)

---

## CA-4: Lambda의 캡처 문제

```cpp
class Widget {
public:
    void addFilter() const {
        filters.emplace_back(
            [=](int value) { return value % divisor == 0; }
        );
    }
private:
    int divisor;
};

void doSomeWork() {
    auto pw = std::make_unique<Widget>();
    pw->addFilter();
    // pw가 파괴된 후 filters의 lambda를 호출하면?
}
```

> [!question] 이 코드의 위험성은 무엇인가?

> [!note]- 정답 확인 (클릭)
> `[=]`가 `divisor`를 값으로 캡처하는 것처럼 보이지만, 실제로는 **`this` 포인터를 값으로 캡처**한다. Lambda 내부의 `divisor`는 `this->divisor`로 해석된다. `doSomeWork`에서 `pw`가 파괴되면 `this`가 가리키는 Widget 객체도 파괴되므로, lambda를 호출하면 **dangling pointer를 통한 접근** -- undefined behavior!
>
> **수정 (C++14)**:
> ```cpp
> filters.emplace_back(
>     [divisor = divisor](int value) { return value % divisor == 0; }
> );
> ```
> Init capture로 멤버 변수의 **값을 복사**하여 closure에 저장한다. (📕 p.219-222)

---

## CA-5: RVO를 방해하는 코드

```cpp
Widget makeWidget() {
    Widget w;
    // ... configure w ...
    return std::move(w);  // "최적화"?
}
```

> [!question] 이 코드가 `return w;`보다 느릴 수 있는 이유는?

> [!note]- 정답 확인 (클릭)
> `return w;`를 쓰면 컴파일러는 **RVO(Return Value Optimization)**를 적용하여 `w`를 반환값 위치에 직접 생성할 수 있다 (copy도 move도 없음). `return std::move(w);`는 반환값이 로컬 변수가 아닌 **reference (rvalue)**이므로 RVO 조건 (2)를 충족하지 못한다. 결과적으로 **move construction이 강제**되며, 이는 RVO보다 비용이 크다.
>
> 또한 RVO가 불가능한 경우에도 표준은 로컬 변수를 자동으로 rvalue로 취급하도록 요구하므로 `std::move`를 명시할 필요가 없다. (📕 p.173-176)

---

## CA-6: Overloading과 Universal Reference의 충돌

```cpp
class Person {
public:
    template<typename T>
    explicit Person(T&& n) : name(std::forward<T>(n)) {}

    explicit Person(int idx) : name(nameFromIdx(idx)) {}
private:
    std::string name;
};

Person p("Nancy");
auto cloneOfP(p);  // 어떤 일이 발생하는가?
```

> [!question] `cloneOfP`가 `p`의 복사본이 되는가?

> [!note]- 정답 확인 (클릭)
> **컴파일 에러!** `p`는 **non-const lvalue**이다. Copy constructor는 `const Person&`을 받지만, perfect forwarding constructor는 `Person&`로 instantiate되어 **exact match**가 된다. `const` 추가 없는 exact match가 promotion보다 우선하므로 perfect forwarding constructor가 호출된다. 이 constructor는 `Person` 객체로 `std::string`을 초기화하려 하지만 `std::string(Person&)` 같은 constructor는 없으므로 **컴파일 에러**.
>
> `const Person cp("Nancy"); auto clone(cp);`는 동작한다 -- `const Person&`이 copy constructor와 exact match이기 때문이다. (📕 p.180-182)

---

## CA-7: `std::bind`의 인자 평가 시점

```cpp
using namespace std::chrono;
using namespace std::literals;

// Lambda version
auto setSoundL = [](Sound s) {
    setAlarm(steady_clock::now() + 1h, s, 30s);
};

// bind version
auto setSoundB = std::bind(setAlarm,
    steady_clock::now() + 1h, _1, 30s);

// 10분 후에 호출
setSoundL(Sound::Siren);
setSoundB(Sound::Siren);
```

> [!question] 두 호출의 알람 시간이 다른 이유는?

> [!note]- 정답 확인 (클릭)
> - **Lambda**: `steady_clock::now() + 1h`는 `setSoundL`이 **호출될 때** 평가된다. 따라서 호출 시점으로부터 1시간 후에 알람이 울린다.
> - **`std::bind`**: `steady_clock::now() + 1h`는 `std::bind`가 **호출될 때** 평가되어 bind object에 저장된다. 10분 후에 `setSoundB`를 호출하면, 알람은 **bind 호출 시점으로부터 1시간 후** = 현재로부터 50분 후에 울린다.
>
> 이것이 lambda가 `std::bind`보다 직관적인 핵심 이유 중 하나이다. (📕 p.233-234)

---

## CA-8: Init Capture로 Move 캡처하기

```cpp
// C++11 -- 컴파일 에러!
auto pw = std::make_unique<Widget>();
auto func = [pw]() {  // unique_ptr은 copy 불가!
    return pw->isValidated();
};
```

> [!question] 이 코드를 C++14와 C++11에서 각각 어떻게 수정하는가?

> [!note]- 정답 확인 (클릭)
> **C++14 -- init capture 사용:**
> ```cpp
> auto pw = std::make_unique<Widget>();
> auto func = [pw = std::move(pw)]() {
>     return pw->isValidated();
> };
> ```
>
> **C++11 -- `std::bind` workaround:**
> ```cpp
> auto pw = std::make_unique<Widget>();
> auto func = std::bind(
>     [](const std::unique_ptr<Widget>& pw) {
>         return pw->isValidated();
>     },
>     std::move(pw)
> );
> ```
> `std::bind`에 rvalue를 전달하면 bind object 내부에 **move construct**된다. Lambda가 호출될 때 이 move-constructed 객체가 참조로 전달된다. (📕 p.224-228)

---

# 🔥 함정 문제 (5문제)

## T1: 교활한 `T&&`

```cpp
template<typename T>
class MyContainer {
public:
    void push_back(T&& elem);  // (A)

    template<typename... Args>
    void emplace_back(Args&&... args);  // (B)
};
```

> [!question] (A)와 (B) 중 universal reference는 어느 것인가?

> [!note]- 정답 확인 (클릭)
> **(B)만 universal reference**이다.
>
> **(A)**: `T`는 **class template의 파라미터**이므로 `push_back` 호출 시 이미 결정되어 있다. `MyContainer<Widget>`이면 `push_back(Widget&& elem)`이 되어 **rvalue reference**이다. Type deduction이 발생하지 않는다.
>
> **(B)**: `Args`는 **함수 template의 파라미터**이므로 `emplace_back` 호출마다 **type deduction이 발생**한다. `Args&&...` 형태이므로 **universal reference**이다.
>
> 이 차이는 `std::vector`의 `push_back`(rvalue reference overload)과 `emplace_back`(universal reference)의 관계와 동일하다. (📕 p.165-166)

---

## T2: Dangling Reference의 시한폭탄

```cpp
std::function<bool(int)> makeFilter() {
    int divisor = 5;
    return [&](int value) { return value % divisor == 0; };
}

auto f = makeFilter();
bool result = f(10);  // 안전한가?
```

> [!question] 이 코드는 정상 동작하는가?

> [!note]- 정답 확인 (클릭)
> **Undefined Behavior!** Default by-reference capture `[&]`로 `divisor`를 참조로 캡처했지만, `makeFilter`가 반환되면 `divisor`는 **파괴**된다. 반환된 closure는 이미 존재하지 않는 지역 변수에 대한 **dangling reference**를 가지고 있다.
>
> **수정**: 값으로 캡처하라.
> ```cpp
> return [divisor](int value) { return value % divisor == 0; };
> ```
> 또는 (더 안전한 습관):
> ```cpp
> return [=](int value) { return value % divisor == 0; };
> ```
> 이 경우 `divisor`는 지역 변수이므로 `[=]`가 안전하다 (멤버 변수가 아니므로 `this` 문제 없음). (📕 p.216-217)

---

## T3: Perfect Forwarding의 함정

```cpp
void processVec(const std::vector<int>& v) {
    // ...
}

template<typename T>
void fwd(T&& param) {
    processVec(std::forward<T>(param));
}

// 다음 중 컴파일되는 것은?
processVec({1, 2, 3});   // (A)
fwd({1, 2, 3});          // (B)
fwd(std::vector<int>{1, 2, 3});  // (C)
```

> [!question] (A), (B), (C) 중 어떤 것이 컴파일되고, 어떤 것이 실패하는가?

> [!note]- 정답 확인 (클릭)
> | 호출 | 결과 | 이유 |
> |------|------|------|
> | (A) | **컴파일 성공** | 직접 호출 -- `{1,2,3}`이 `std::vector<int>`로 implicit conversion |
> | (B) | **컴파일 실패** | Braced initializer는 template type deduction에서 non-deduced context |
> | (C) | **컴파일 성공** | 명시적으로 `std::vector<int>` 임시 객체를 생성했으므로 T 추론 가능 |
>
> **Workaround for (B)**:
> ```cpp
> auto il = {1, 2, 3};  // std::initializer_list<int>
> fwd(il);              // OK!
> ```
> (📕 p.207-209)

---

## T4: Static 변수와 `[=]`의 착각

```cpp
void addFilter() {
    static auto divisor = computeDivisor();

    filters.emplace_back(
        [=](int value) { return value % divisor == 0; }
    );

    ++divisor;  // static 변수 수정
}
```

> [!question] `[=]`를 사용했으므로 lambda는 `divisor`의 복사본을 가지고 있는가?

> [!note]- 정답 확인 (클릭)
> **아니다! 아무것도 캡처하지 않는다.** `[=]`는 non-static 지역 변수만 캡처한다. `divisor`는 **static 변수**이므로 **캡처 대상이 아니다**. Lambda 내부의 `divisor`는 static 변수를 **직접 참조**한다. `++divisor;` 이후에 생성된 lambda와 이전에 생성된 lambda 모두 **변경된 값**을 보게 된다.
>
> `[=]`가 "모든 것을 값으로 복사한다"는 착각을 주지만, 실제로는 static 변수는 **참조로 사용**되는 것과 같다. 이것이 default capture mode를 피해야 하는 또 다른 이유이다. (📕 p.222-223)

---

## T5: `noexcept`와 Move의 관계

```cpp
class BigData {
public:
    BigData(BigData&& other) {  // noexcept 없음!
        // ... move implementation ...
    }
};

std::vector<BigData> vec;
vec.push_back(BigData{});  // move 사용?
vec.reserve(100);          // 기존 원소를 move? copy?
```

> [!question] `reserve`로 재할당 시 기존 원소가 move되는가, copy되는가?

> [!note]- 정답 확인 (클릭)
> **Copy된다.** `std::vector`의 `reserve` (또는 `push_back` 등 재할당이 필요한 경우)는 **strong exception safety guarantee**를 제공해야 한다. Move constructor가 `noexcept`가 아니면 move 중 예외가 발생했을 때 원래 상태로 복원할 수 없으므로, 안전한 **copy**를 사용한다.
>
> **해결**: Move constructor에 `noexcept`를 선언하라.
> ```cpp
> BigData(BigData&& other) noexcept { ... }
> ```
> 이것이 Item 14 ("Declare functions `noexcept` if they won't emit exceptions")의 실전적 이유이다. (📕 p.205-206)

---

# 🎮 미니 챌린지 (3문제)

> [!tip] **도전!**
> 다음 코드를 Modern C++ lambda와 move semantics를 활용하여 리팩토링하세요.

---

## MC-1: `std::bind`를 Lambda로 변환

**Before (std::bind 사용):**
```cpp
using namespace std::placeholders;

auto betweenB =
    std::bind(std::logical_and<>(),
        std::bind(std::less_equal<>(), lowVal, _1),
        std::bind(std::less_equal<>(), _1, highVal));
```

> [!question] 이것을 C++14 lambda로 변환하라.

> [!note]- 정답 확인 (클릭)
> ```cpp
> auto betweenL = [lowVal, highVal](const auto& val) {
>     return lowVal <= val && val <= highVal;
> };
> ```
> Lambda 버전이 훨씬 읽기 쉽고, `auto` 파라미터로 인해 다형적이며, inline 최적화도 가능하다. (📕 p.236-237)

---

## MC-2: Move-only 객체를 Closure로 이동

**Before (컴파일 에러):**
```cpp
std::vector<double> data = getLargeDataSet();

// 이 vector를 lambda에서 사용하고 싶지만 복사 비용이 크다
auto processData = [data]() {  // 비싼 복사!
    return computeResult(data);
};
```

> [!question] 복사 대신 move를 사용하도록 C++14와 C++11 각각으로 리팩토링하라.

> [!note]- 정답 확인 (클릭)
> **C++14 (init capture):**
> ```cpp
> std::vector<double> data = getLargeDataSet();
> auto processData = [data = std::move(data)]() {
>     return computeResult(data);
> };
> ```
>
> **C++11 (std::bind workaround):**
> ```cpp
> std::vector<double> data = getLargeDataSet();
> auto processData = std::bind(
>     [](const std::vector<double>& data) {
>         return computeResult(data);
>     },
>     std::move(data)
> );
> ```
> `std::bind`에 rvalue로 전달하면 bind object 내부에 move-constructed 되고, lambda는 이를 const reference로 받는다. (📕 p.226-228)

---

## MC-3: Generic Lambda에서 Perfect Forwarding

**Before (forwarding 미적용):**
```cpp
auto wrapper = [](auto x) {
    return targetFunction(x);
};
```

> [!question] `x`가 lvalue/rvalue 정보를 보존하도록 perfect forwarding을 적용하라.

> [!note]- 정답 확인 (클릭)
> ```cpp
> auto wrapper = [](auto&& x) {
>     return targetFunction(std::forward<decltype(x)>(x));
> };
> ```
>
> **Variadic 버전:**
> ```cpp
> auto wrapper = [](auto&&... args) {
>     return targetFunction(
>         std::forward<decltype(args)>(args)...
>     );
> };
> ```
> `auto&&`는 universal reference이고, `decltype(x)`는 lvalue에 대해 `T&`, rvalue에 대해 `T&&`를 반환하여 `std::forward`에 올바른 타입을 전달한다. (📕 p.229-231)

---

# 📝 빈칸 채우기 (5문제)

### BF-1

> [!question] 빈칸을 채우시오
> `std::move`는 인자를 ____적으로 rvalue로 캐스팅하고, `std::forward`는 ____적으로 rvalue로 캐스팅한다.

> [!note]- 정답 확인 (클릭)
> `std::move`는 인자를 **무조건(unconditionally)**적으로 rvalue로 캐스팅하고, `std::forward`는 **조건(conditionally)**적으로 rvalue로 캐스팅한다. (📕 p.158)

---

### BF-2

> [!question] 빈칸을 채우시오
> Reference collapsing 규칙에서 `T& &&`와 `T&& &`는 모두 ____가 되고, `T&& &&`만 ____가 된다.

> [!note]- 정답 확인 (클릭)
> `T& &&`와 `T&& &`는 모두 **`T&` (lvalue reference)**가 되고, `T&& &&`만 **`T&&` (rvalue reference)**가 된다. "둘 중 하나라도 lvalue reference이면 결과는 lvalue reference"라고 기억하자. (📕 p.199)

---

### BF-3

> [!question] 빈칸을 채우시오
> Perfect forwarding이 실패하는 5가지 경우: ____initializers, ____ 또는 NULL을 null pointer로, declaration-only integral static ____ data members, overloaded ____ names와 template names, 그리고 ____.

> [!note]- 정답 확인 (클릭)
> **Braced** initializers, **0** 또는 NULL을 null pointer로, declaration-only integral static **const** data members, overloaded **function** names와 template names, 그리고 **bitfields**. (📕 p.214)

---

### BF-4

> [!question] 빈칸을 채우시오
> C++14 generic lambda에서 perfect forwarding을 구현하는 패턴:
> ```cpp
> auto f = [](auto&& param) {
>     return func(std::forward<____>(param));
> };
> ```

> [!note]- 정답 확인 (클릭)
> ```cpp
> auto f = [](auto&& param) {
>     return func(std::forward<decltype(param)>(param));
> };
> ```
> **`decltype(param)`**이 정답이다. lvalue가 전달되면 lvalue reference 타입을, rvalue가 전달되면 rvalue reference 타입을 반환한다. (📕 p.230-231)

---

### BF-5

> [!question] 빈칸을 채우시오
> Lambda의 `[=]` 캡처 모드에서 멤버 함수 내의 멤버 변수는 직접 캡처되지 않고, 실제로는 ____ 포인터가 값으로 캡처되어 멤버 변수에는 ____를 통해 접근한다.

> [!note]- 정답 확인 (클릭)
> 실제로는 **`this`** 포인터가 값으로 캡처되어 멤버 변수에는 **`this->member`**를 통해 접근한다. 이 때문에 객체가 파괴된 후 closure를 사용하면 dangling pointer 문제가 발생할 수 있다. (📕 p.220)

---

# 🏆 학습 완료 체크리스트

- [ ] `std::move`와 `std::forward`의 차이를 설명할 수 있다
- [ ] Universal reference와 rvalue reference를 구분할 수 있다
- [ ] 왜 universal reference에 overloading하면 안 되는지 이해한다
- [ ] Reference collapsing 규칙 4가지를 알고 있다
- [ ] Perfect forwarding failure case 5가지를 열거할 수 있다
- [ ] `std::array` vs `std::vector`의 move 비용 차이를 안다
- [ ] Lambda default capture mode의 위험성을 이해한다
- [ ] Init capture로 move-only 객체를 closure에 넣을 수 있다
- [ ] Generic lambda에서 `decltype` + `std::forward` 패턴을 쓸 수 있다
- [ ] Lambda가 `std::bind`보다 나은 이유를 3가지 이상 설명할 수 있다

> [!tip] **기억법 총정리**
> - **`std::move`** = "rvalue야!" (무조건 외침) / **`std::forward`** = "원래 뭐였더라?" (조건부 판단)
> - **Universal reference** = "T&& + type deduction" (둘 다 있어야!)
> - **Reference collapsing** = "lvalue가 하나라도 있으면 lvalue가 이긴다" (lvalue is sticky!)
> - **`[=]` in member function** = "this를 캡처하는 거지, 멤버를 캡처하는 게 아니다!"
> - **RVO와 std::move** = "컴파일러를 도우려다 오히려 방해한다!" (return w;가 최선)
