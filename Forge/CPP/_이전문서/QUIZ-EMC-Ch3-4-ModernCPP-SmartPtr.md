# Effective Modern C++ - Chapter 3 & 4 Study Guide & Quiz

> **Chapter 3: Moving to Modern C++ (Items 7-17)**
> **Chapter 4: Smart Pointers (Items 18-22)**
> Scott Meyers | 난이도 표시: ⭐ 쉬움 / ⭐⭐ 보통 / ⭐⭐⭐ 어려움

---

## 📖 핵심 개념 요약

### Chapter 3: Moving to Modern C++

---

### Item 7: `()` vs `{}` 객체 생성 시 구분하라 (📕 p.49-57)

> [!question] 왜 알아야 하나?
> `{}`와 `()`가 완전히 다른 결과를 만드는 경우가 있다. `vector<int> v{10, 20}`은 원소 2개, `v(10, 20)`은 원소 10개. 이 차이를 모르면 프로덕션에서 데이터가 엉뚱하게 들어간다. `std::make_shared`/`std::make_unique`도 내부적으로 `()`를 쓰기 때문에, braced init과 make 함수의 결과가 다르다는 걸 모르면 버그 원인을 찾을 수 없다.

> [!danger] 모르면 이런 버그가 난다
> `std::vector<int> scores{100, 0}`으로 "100점, 0점" 두 원소를 의도했는데, 동료가 리팩토링하면서 `scores(100, 0)`으로 바꾸면 "값 0인 원소 100개"가 된다. 테스트에서 `size()` 검증 안 하면 런타임까지 살아남는 사일런트 버그.

> [!tip] 한 줄 핵심
> Braces `{}`는 `initializer_list` 생성자를 **강제 우선 매칭**한다 -- 괄호와 결과가 다를 수 있으니 항상 의도를 확인하라.

---

### Item 8: `0`과 `NULL` 대신 `nullptr`을 선호하라 (📕 p.57-62)

> [!question] 왜 알아야 하나?
> `0`과 `NULL`은 정수 타입이다. 포인터가 아니다. 템플릿 타입 추론에서 `0`을 넘기면 `int`로 추론되어 포인터 파라미터에 전달이 불가능하다. 오버로드 해석에서도 포인터 오버로드가 아닌 정수 오버로드가 호출된다.

> [!danger] 모르면 이런 버그가 난다
> `lockAndCall(f, mutex, 0)`에서 `f`가 `shared_ptr<Widget>` 파라미터를 기대하는데, `0`이 `int`로 추론되어 컴파일 에러. 코드 리뷰에서 "왜 0이 포인터로 안 되지?" 하고 30분 삽질한다. `nullptr`로 바꾸면 즉시 해결.

> [!tip] 한 줄 핵심
> 포인터를 의미할 때는 무조건 `nullptr` -- `0`/`NULL`은 정수이지 포인터가 아니다.

---

### Item 9: `typedef` 대신 alias declaration을 선호하라 (📕 p.62-67)

> [!question] 왜 알아야 하나?
> `typedef`로는 템플릿 별칭을 만들 수 없다. `template<typename T> using MyList = std::list<T, MyAlloc<T>>`가 alias template으로는 한 줄인데, `typedef`로는 templatized struct를 감싸는 해킹이 필요하고, 사용할 때마다 `typename`과 `::type`을 붙여야 한다.

> [!danger] 모르면 이런 버그가 난다
> `typedef`로 만든 dependent type을 템플릿 안에서 쓸 때 `typename` 빼먹으면 컴파일 에러. C++14의 `_t` suffix alias (`remove_const_t<T>`)를 모르면 모든 type trait에 `typename std::remove_const<T>::type`을 장황하게 쓰게 된다.

> [!tip] 한 줄 핵심
> `using`은 `typedef`의 상위 호환이다 -- 템플릿화 가능하고, dependent type 문제도 없다.

---

### Item 10: Unscoped enum 대신 scoped enum을 선호하라 (📕 p.67-73)

> [!question] 왜 알아야 하나?
> Unscoped enum의 enumerator는 바깥 스코프로 누출되어 이름 충돌을 일으킨다. 게다가 정수형으로 implicit conversion되므로 `if (priority < 4.5)` 같은 무의미한 비교가 컴파일된다. Scoped enum은 이 두 문제를 모두 차단한다.

> [!danger] 모르면 이런 버그가 난다
> `enum Color { black, white, red };` 선언 후 같은 스코프에서 `auto white = false;` 쓰면 이름 충돌로 컴파일 에러. 또는 unscoped enum 값이 의도치 않게 `double`과 비교되어 논리 버그 발생.

> [!tip] 한 줄 핵심
> `enum class`는 이름 누출과 implicit conversion을 모두 막아준다 -- 예외는 `std::get<>` 인덱스 정도.

---

### Item 11: `private` undefined 함수 대신 `= delete`를 선호하라 (📕 p.73-78)

> [!question] 왜 알아야 하나?
> C++98의 private-undefined 트릭은 링크 타임에야 에러가 난다 (friend나 멤버 함수에서 호출 시). `= delete`는 컴파일 타임에 잡는다. 게다가 non-member 함수와 템플릿 특수화에도 적용 가능해서 원치 않는 implicit conversion을 정밀 차단할 수 있다.

> [!danger] 모르면 이런 버그가 난다
> `bool isLucky(int)`만 있으면 `isLucky('a')`, `isLucky(3.5)`가 컴파일된다. `char`와 `double` 오버로드를 `= delete`로 막지 않으면 implicit conversion으로 의도치 않은 호출이 발생.

> [!tip] 한 줄 핵심
> `= delete`는 public에 선언하라 -- 접근성 에러보다 "삭제됨" 에러 메시지가 훨씬 명확하다.

---

### Item 12: Override 함수에 `override`를 선언하라 (📕 p.78-85)

> [!question] 왜 알아야 하나?
> Override 조건은 6가지나 된다 (virtual, 이름, 파라미터, const, 반환 타입/exception, reference qualifier). 하나라도 틀리면 새 함수가 되는데 컴파일러는 아무 경고도 안 한다. `override` 키워드가 없으면 "override한 줄 알았는데 실은 새 함수"인 버그를 런타임에야 발견한다.

> [!danger] 모르면 이런 버그가 난다
> Base의 `void mf1() const`를 Derived에서 `void mf1()` (const 빠짐)로 "override"하면, 다형성 호출 시 Base 버전이 실행된다. `override` 키워드 없으면 컴파일러가 조용히 넘어가서 디버깅에 수 시간 소모.

> [!tip] 한 줄 핵심
> `override`는 "내가 실수하면 컴파일러가 잡아줘"라는 안전장치다 -- 항상 붙여라.

---

### Item 13: `iterator` 대신 `const_iterator`를 선호하라 (📕 p.85-89)

> [!question] 왜 알아야 하나?
> "수정할 필요 없는 것은 const로"라는 원칙의 iterator 버전이다. C++98에서는 `const_iterator` 사용이 극히 불편했지만, C++11부터 `cbegin()`/`cend()`가 생기고, `insert`/`erase`도 `const_iterator`를 받으므로 실용적으로 쓸 수 있게 되었다.

> [!danger] 모르면 이런 버그가 난다
> 읽기 전용 루프에서 일반 `iterator`를 쓰면 실수로 컨테이너 원소를 수정할 수 있다. `const_iterator`를 쓰면 컴파일러가 수정 시도를 차단.

> [!tip] 한 줄 핵심
> 원소를 수정하지 않을 때는 `cbegin()`/`cend()` -- const 정신을 iterator에도 적용하라.

---

### Item 14: Exception을 방출하지 않는 함수는 `noexcept`로 선언하라 (📕 p.89-96)

> [!question] 왜 알아야 하나?
> `noexcept` 안 달면 `std::vector`가 reallocation할 때 move 대신 copy를 쓴다. 수백만 객체면 성능 차이가 수십 배. STL은 "move if you can, but copy if you must" 정책이라 move constructor가 `noexcept`가 아니면 안전하게 copy로 fallback한다.

> [!danger] 모르면 이런 버그가 난다
> 게임 엔진에서 `std::vector<GameObject>`의 push_back이 비정상적으로 느린 이유가 move constructor에 `noexcept`가 없어서 매번 deep copy를 하고 있었던 것. 프로파일링해야 겨우 발견.

> [!tip] 한 줄 핵심
> Move 연산과 swap에는 반드시 `noexcept` -- STL 성능이 이것에 달려 있다.

---

### Item 15: `constexpr`을 가능한 한 사용하라 (📕 p.96-103)

> [!question] 왜 알아야 하나?
> `constexpr`은 "이 계산을 컴파일 타임에 끝낼 수 있다"는 선언이다. 배열 크기, template 인자, enum 값 등 컴파일 타임 상수가 필요한 곳에 런타임 함수 호출 결과를 쓸 수 없지만, `constexpr` 함수라면 가능하다. 런타임 비용 제로.

> [!danger] 모르면 이런 버그가 난다
> 룩업 테이블 크기를 `constexpr` 함수로 계산하면 컴파일 타임에 확정되지만, 일반 함수면 VLA나 동적 할당이 필요해진다. 또한 `constexpr`은 인터페이스의 일부라 한번 공개하면 제거 시 클라이언트 코드가 깨진다.

> [!tip] 한 줄 핵심
> `constexpr`은 "컴파일 타임에 될 수 있으면 해라"는 최적화 힌트이자 계약이다.

---

### Item 16: `const` 멤버 함수를 thread-safe하게 만들어라 (📕 p.103-108)

> [!question] 왜 알아야 하나?
> `const` 멤버 함수는 "읽기 전용"이라 여러 스레드에서 동시 호출해도 안전할 것으로 기대된다. 하지만 `mutable` 캐시 변수를 수정하면 data race가 발생한다. 호출자는 `const`만 보고 락 없이 호출하므로, 구현자가 thread safety를 보장해야 한다.

> [!danger] 모르면 이런 버그가 난다
> `const` 함수 안에서 `mutable std::atomic<bool> cacheValid`와 `mutable std::atomic<int> cachedValue` 두 개를 각각 atomic으로 만들어도, 두 변수 사이의 원자성은 보장되지 않아 race condition 발생. `std::mutex`가 필요한 상황에 `std::atomic`을 쓰면 캐시된 잘못된 값을 반환한다.

> [!tip] 한 줄 핵심
> 변수 1개 보호는 `atomic`, 변수 2개 이상 함께 보호는 `mutex` -- `const` 함수도 예외 아니다.

---

### Item 17: 특수 멤버 함수 생성 규칙을 이해하라 (📕 p.108-115)

> [!question] 왜 알아야 하나?
> Destructor 하나 추가했을 뿐인데 `std::map` 멤버의 이동이 전부 복사로 바뀌어 성능이 수십 배 떨어질 수 있다. Move 연산 자동 생성 조건(copy 미선언 + move 미선언 + destructor 미선언)을 모르면 "왜 느려졌지?"의 원인을 찾을 수 없다.

> [!danger] 모르면 이런 버그가 난다
> `StringTable` 클래스에 로깅용 destructor를 추가했더니, 내부 `std::map<int, std::string>`의 모든 이동이 복사로 대체되어 성능 급락. Rule of Three/Five를 모르면 디버깅 불가.

> [!tip] 한 줄 핵심
> Destructor를 선언하면 move가 죽는다 -- Big Five를 명시적으로 `= default` 하라.

---

### Chapter 4: Smart Pointers

---

### Item 18: Exclusive ownership에 `std::unique_ptr` 사용하라 (📕 p.117-124)

> [!question] 왜 알아야 하나?
> Raw pointer로 `new`한 객체를 `delete` 안 하면 메모리 누수. 예외 경로에서 `delete`를 빼먹으면 더 찾기 어렵다. `unique_ptr`은 스코프를 벗어나면 자동 해제되고, raw pointer와 같은 크기/속도라서 비용이 없다. 게임에서 Actor 수명 관리의 C++ 기본기.

> [!danger] 모르면 이런 버그가 난다
> Factory 함수가 raw pointer를 반환하면 호출자가 `delete` 책임을 잊기 쉽다. 예외 발생 시 `delete`에 도달하지 못해 메모리 누수. `unique_ptr` 반환이면 스코프 벗어날 때 자동 해제.

> [!tip] 한 줄 핵심
> `unique_ptr`은 "비용 없는 자동 해제" -- factory 함수의 기본 반환 타입으로 써라.

---

### Item 19: Shared ownership에 `std::shared_ptr` 사용하라 (📕 p.124-134)

> [!question] 왜 알아야 하나?
> 여러 곳에서 하나의 객체를 공유하면서 "마지막 사용자가 나가면 자동 해제"가 필요할 때가 있다. `shared_ptr`은 reference counting으로 이를 자동화한다. 하지만 raw pointer 2배 크기이고, atomic ref count 조작 비용이 있으므로 "필요할 때만" 써야 한다.

> [!danger] 모르면 이런 버그가 난다
> 하나의 raw pointer로 `shared_ptr` 두 개를 따로 생성하면 control block이 2개 만들어져 이중 해제(UB). `this`에서 `shared_ptr`을 만들 때도 같은 함정 -- `enable_shared_from_this` 없이 `shared_ptr<Widget>(this)`하면 이중 해제.

> [!tip] 한 줄 핵심
> "하나의 raw pointer에서 `shared_ptr` 두 개 만들지 마라" -- control block 중복이 이중 해제를 낳는다.

---

### Item 20: Dangle 가능한 포인터에 `std::weak_ptr` 사용하라 (📕 p.134-138)

> [!question] 왜 알아야 하나?
> `shared_ptr` 순환 참조는 reference count가 영원히 0이 되지 않아 메모리 누수를 일으킨다. `weak_ptr`로 한쪽을 끊으면 cycle이 깨진다. 캐시, Observer 패턴 등 "참조는 하되 수명은 연장하지 않는" 관계에 필수.

> [!danger] 모르면 이런 버그가 난다
> A와 B가 `shared_ptr`로 서로를 가리키면 스코프를 벗어나도 ref count가 1로 남아 영원히 해제 불가. 장시간 운영되는 서버에서 이런 순환 참조가 하나씩 쌓이면 OOM 크래시.

> [!tip] 한 줄 핵심
> `weak_ptr`은 "참조하되 소유하지 않는다" -- 순환 참조 방지와 dangling 감지의 열쇠.

---

### Item 21: `new` 직접 사용 대신 `std::make_unique`/`std::make_shared`를 선호하라 (📕 p.139-147)

> [!question] 왜 알아야 하나?
> `processWidget(shared_ptr<Widget>(new Widget), computePriority())`에서 `new`와 `shared_ptr` 생성자 사이에 예외가 터지면 메모리 누수. `make_shared`는 이 틈이 없다. 게다가 객체와 control block을 단일 할당으로 처리해서 메모리/성능 모두 이득.

> [!danger] 모르면 이런 버그가 난다
> 함수 인자에서 `new`와 `shared_ptr` 생성자를 분리해서 쓰면, 컴파일러의 평가 순서에 따라 예외 발생 시 `new`된 메모리가 누수. 코드 리뷰에서 잡기 매우 어려운 간헐적 메모리 누수.

> [!tip] 한 줄 핵심
> `make_shared`/`make_unique` = exception safety + 단일 할당 -- custom deleter나 braced init이 아니면 항상 사용.

---

### Item 22: Pimpl Idiom과 `std::unique_ptr` (📕 p.147-156)

> [!question] 왜 알아야 하나?
> Pimpl은 헤더에서 구현 세부사항을 숨겨 컴파일 의존성을 줄이는 고전 기법이다. `unique_ptr<Impl>`로 구현하면 자동 해제까지 얻지만, destructor/move를 헤더가 아닌 .cpp에서 정의해야 하는 함정이 있다. 이걸 모르면 "incomplete type" 컴파일 에러에 막힌다.

> [!danger] 모르면 이런 버그가 난다
> `Widget` 헤더에 destructor를 선언하지 않으면 compiler-generated destructor가 헤더에서 inline 생성되는데, 이 시점에 `Impl`은 incomplete type이라 `unique_ptr`의 `static_assert`가 실패. Move 연산도 같은 이유로 실패.

> [!tip] 한 줄 핵심
> Pimpl + `unique_ptr` = destructor/move를 `.h`에 선언, `.cpp`에서 `= default` 정의 -- 이 패턴을 외워라.

---

## 🧠 OX 퀴즈 (12문제)

> [!question] **Q1.** ⭐ `std::vector<int> v{10, 20};`은 값이 20인 원소 10개를 가진 벡터를 생성한다.
> (📕 p.55)

> [!note]- 정답 확인 (클릭)
> **X (거짓)** — `{10, 20}`은 `std::initializer_list` 생성자를 호출하여 **원소가 10과 20인 벡터 2개**를 생성한다. 값 20인 원소 10개는 `std::vector<int> v(10, 20);`으로 괄호를 사용해야 한다. (📕 p.55)

---

> [!question] **Q2.** ⭐ `nullptr`의 타입은 `void*`이다.
> (📕 p.58)

> [!note]- 정답 확인 (클릭)
> **X (거짓)** — `nullptr`의 타입은 `std::nullptr_t`이다. `void*`가 아니며, `std::nullptr_t`는 모든 raw pointer 타입으로 implicitly convert 가능하다. (📕 p.58)

---

> [!question] **Q3.** ⭐⭐ Alias template을 사용하면 dependent type 문제로 인한 `typename` 키워드가 필요 없어진다.
> (📕 p.63-65)

> [!note]- 정답 확인 (클릭)
> **O (참)** — Alias template(예: `template<typename T> using MyList = std::list<T, MyAlloc<T>>`)은 컴파일러가 항상 타입임을 알기에 `typename`이 불필요하다. 반면 nested typedef(`MyAllocList<T>::type`)는 dependent type이므로 `typename` 필요. (📕 p.63-65)

---

> [!question] **Q4.** ⭐ Scoped enum (`enum class`)의 enumerator는 enclosing scope로 누출된다.
> (📕 p.67)

> [!note]- 정답 확인 (클릭)
> **X (거짓)** — Scoped enum의 enumerator는 enum 스코프 내에서만 보인다. 사용 시 `Color::red`처럼 scope qualifier가 필요하다. 누출되는 것은 **unscoped enum** (C++98 스타일). (📕 p.67)

---

> [!question] **Q5.** ⭐⭐ `= delete`로 선언된 함수는 `private`에 두는 것이 관례이다.
> (📕 p.74-75)

> [!note]- 정답 확인 (클릭)
> **X (거짓)** — `= delete`는 **`public`으로 선언하는 것이 관례**이다. 접근성 에러(private) 대신 삭제 에러 메시지가 더 명확하고 유용하기 때문이다. (📕 p.74-75)

---

> [!question] **Q6.** ⭐⭐ `override` 키워드 없이도 derived class 함수가 base class virtual 함수를 override할 수 있다.
> (📕 p.78-81)

> [!note]- 정답 확인 (클릭)
> **O (참)** — `override`는 필수가 아니라 권장 사항이다. 없어도 조건이 맞으면 override된다. 하지만 `override` 사용 시 조건 불일치를 **컴파일 에러로 잡을 수 있어** 사용을 강력히 권장한다. (📕 p.78-81)

---

> [!question] **Q7.** ⭐⭐ `noexcept` 함수 안에서 non-`noexcept` 함수를 호출하면 컴파일 에러가 발생한다.
> (📕 p.95-96)

> [!note]- 정답 확인 (클릭)
> **X (거짓)** — C++은 `noexcept` 함수가 non-`noexcept` 함수에 의존하는 것을 허용하며, 컴파일러는 일반적으로 경고도 발생시키지 않는다. C 라이브러리나 C++98 레거시 코드 등 정당한 이유가 있기 때문이다. (📕 p.95-96)

---

> [!question] **Q8.** ⭐⭐⭐ `const` 객체에 `std::move`를 적용하면 move constructor가 호출된다.
> (📕 p.159-160)

> [!note]- 정답 확인 (클릭)
> **X (거짓)** — `std::move(const_obj)`의 결과는 `const T&&` (rvalue reference to const)이다. Move constructor는 `T&&` (non-const)를 받으므로 매칭되지 않고, 대신 **copy constructor** (`const T&`)가 호출된다. Move 요청이 조용히 copy로 전환된다. (📕 p.159-160)

---

> [!question] **Q9.** ⭐ `std::unique_ptr`는 `std::shared_ptr`로 쉽게 변환할 수 있다.
> (📕 p.124)

> [!note]- 정답 확인 (클릭)
> **O (참)** — `std::shared_ptr<T> sp = std::move(uniquePtr);`로 간단히 변환된다. 하지만 **반대 방향 (shared -> unique)은 불가능**하다. (📕 p.124)

---

> [!question] **Q10.** ⭐⭐ `std::shared_ptr`의 custom deleter는 smart pointer 타입의 일부이다.
> (📕 p.126-127)

> [!note]- 정답 확인 (클릭)
> **X (거짓)** — `std::unique_ptr`에서는 deleter가 타입의 일부이지만, `std::shared_ptr`에서는 **타입의 일부가 아니다**. 따라서 다른 deleter를 가진 `shared_ptr`들도 같은 컨테이너에 저장 가능하다. (📕 p.126-127)

---

> [!question] **Q11.** ⭐⭐⭐ User-declared destructor가 있는 클래스에서는 move 연산이 자동 생성되지 않는다.
> (📕 p.111-113)

> [!note]- 정답 확인 (클릭)
> **O (참)** — Move 연산 자동 생성 조건: copy 연산 미선언 + move 연산 미선언 + **destructor 미선언**. 세 조건 모두 충족해야 한다. Destructor만 추가해도 move가 억제되어 copy로 대체된다. (📕 p.111-113)

---

> [!question] **Q12.** ⭐⭐ `std::make_shared`를 사용하면 항상 `new`를 직접 사용하는 것보다 좋다.
> (📕 p.142-145)

> [!note]- 정답 확인 (클릭)
> **X (거짓)** — 대부분의 경우 `make_shared`가 우수하지만, **(1) custom deleter 필요 시**, **(2) braced initializer 사용 시**, **(3) 클래스에 커스텀 `operator new`/`delete` 있을 때**, **(4) 대형 객체 + `weak_ptr` 잔존 시 메모리 지연 해제** 문제가 있을 때는 `new` 직접 사용이 적합하다. (📕 p.142-145)

---

## 💻 코드 분석 퀴즈 (8문제)

> [!question] **Q1.** ⭐⭐ 이 코드의 출력은?
> ```cpp
> std::vector<int> v1(10, 20);
> std::vector<int> v2{10, 20};
> std::cout << v1.size() << " " << v2.size();
> ```
> (📕 p.55)

> [!note]- 정답 확인 (클릭)
> **`10 2`** — `v1(10, 20)`: 괄호 사용 -- 값 20인 원소 **10개**. `v2{10, 20}`: braces 사용 -- `initializer_list` 생성자 호출, 원소 **10과 20** 총 2개. (📕 p.55)

---

> [!question] **Q2.** ⭐⭐ 이 코드는 컴파일되는가?
> ```cpp
> enum Color { black, white, red };
> auto white = false; // ???
> ```
> (📕 p.67)

> [!note]- 정답 확인 (클릭)
> **컴파일 에러** — Unscoped enum `Color`의 enumerator `white`가 enclosing scope로 누출되므로, `auto white = false;`는 이름 충돌로 에러 발생. `enum class Color`로 변경하면 해결된다. (📕 p.67)

---

> [!question] **Q3.** ⭐⭐⭐ 이 Derived 클래스에서 실제로 override되는 함수는 몇 개인가?
> ```cpp
> class Base {
> public:
>     virtual void mf1() const;
>     virtual void mf2(int x);
>     virtual void mf3() &;
>     void mf4() const;
> };
>
> class Derived : public Base {
> public:
>     virtual void mf1();              // (A)
>     virtual void mf2(unsigned int x); // (B)
>     virtual void mf3() &&;           // (C)
>     void mf4() const;                // (D)
> };
> ```
> (📕 p.80-81)

> [!note]- 정답 확인 (클릭)
> **0개 -- 하나도 override되지 않는다!**
> - **(A)** `mf1`: Base는 `const`, Derived는 non-`const` -- const 불일치
> - **(B)** `mf2`: Base는 `int`, Derived는 `unsigned int` -- 파라미터 타입 불일치
> - **(C)** `mf3`: Base는 `&` (lvalue ref-qualified), Derived는 `&&` (rvalue ref-qualified) -- reference qualifier 불일치
> - **(D)** `mf4`: Base에서 `virtual`이 아님 -- override 자체가 불가능
> (📕 p.80-81)

---

> [!question] **Q4.** ⭐⭐ 이 코드의 문제점은?
> ```cpp
> auto pw = new Widget;
> std::shared_ptr<Widget> spw1(pw, loggingDel);
> std::shared_ptr<Widget> spw2(pw, loggingDel);
> ```
> (📕 p.129)

> [!note]- 정답 확인 (클릭)
> **Undefined Behavior -- 이중 해제** — 하나의 raw pointer `pw`로 두 개의 `shared_ptr`를 생성하면 **각각 별도의 control block**이 생성된다. 두 control block 모두 reference count가 0이 되면 같은 객체를 **두 번 delete**한다. `std::make_shared`를 사용하거나, 두 번째는 첫 번째 `shared_ptr`로부터 복사 생성해야 한다. (📕 p.129)

---

> [!question] **Q5.** ⭐⭐ 이 코드에서 잠재적 resource leak이 발생하는 이유는?
> ```cpp
> processWidget(std::shared_ptr<Widget>(new Widget),
>               computePriority());
> ```
> (📕 p.140-141)

> [!note]- 정답 확인 (클릭)
> **컴파일러의 평가 순서 자유도 때문** — 컴파일러가 **(1) `new Widget` --> (2) `computePriority()` --> (3) `shared_ptr` 생성자** 순서로 실행할 수 있다. 만약 (2)에서 예외 발생 시 `new`로 생성된 `Widget`은 아직 `shared_ptr`에 저장되지 않았으므로 메모리 누수. `std::make_shared<Widget>()`을 사용하면 해결. (📕 p.140-141)

---

> [!question] **Q6.** ⭐⭐⭐ 이 코드가 컴파일 에러를 발생시키는 이유는?
> ```cpp
> // widget.h
> class Widget {
> public:
>     Widget();
>     // ~Widget() 없음!
> private:
>     struct Impl;
>     std::unique_ptr<Impl> pImpl;
> };
>
> // main.cpp
> #include "widget.h"
> Widget w; // 에러!
> ```
> (📕 p.149-151)

> [!note]- 정답 확인 (클릭)
> **Incomplete type에 대한 `sizeof`/`delete` 적용 시도** — Compiler-generated destructor는 **header에서 inline으로 생성**되는데, 이 시점에서 `Widget::Impl`은 incomplete type이다. `unique_ptr`의 default deleter가 `static_assert`로 complete type을 요구. **해결**: `.h`에 `~Widget();` 선언, `.cpp`에서 `Widget::~Widget() = default;` 정의. (📕 p.149-151)

---

> [!question] **Q7.** ⭐⭐ 이 코드의 문제점은?
> ```cpp
> class Widget {
> public:
>     int magicValue() const {
>         if (cacheValid) return cachedValue;
>         else {
>             auto val1 = expensiveComputation1();
>             auto val2 = expensiveComputation2();
>             cachedValue = val1 + val2;  // (1)
>             cacheValid = true;          // (2)
>             return cachedValue;
>         }
>     }
> private:
>     mutable std::atomic<bool> cacheValid{false};
>     mutable std::atomic<int> cachedValue;
> };
> ```
> (📕 p.106-107)

> [!note]- 정답 확인 (클릭)
> **멀티스레드 환경에서 중복 계산 (race condition)** — Thread A가 `cachedValue` 대입(1) 후 `cacheValid = true`(2) 전에, Thread B가 `cacheValid`를 체크하면 `false`를 보고 동일한 비싼 계산을 중복 수행한다. 순서를 뒤집으면(cacheValid 먼저 true) 더 심각: Thread B가 아직 갱신 안 된 `cachedValue`를 반환. **두 변수를 함께 조작해야 하므로 `std::mutex` 필요.** (📕 p.106-107)

---

> [!question] **Q8.** ⭐ 이 코드는 컴파일되는가?
> ```cpp
> template<typename FuncType, typename MuxType, typename PtrType>
> decltype(auto) lockAndCall(FuncType func, MuxType& mutex, PtrType ptr) {
>     std::lock_guard<std::mutex> g(mutex);
>     return func(ptr);
> }
>
> auto result = lockAndCall(f1, f1m, 0);      // f1: std::shared_ptr<Widget> 파라미터
> auto result2 = lockAndCall(f3, f3m, nullptr); // f3: Widget* 파라미터
> ```
> (📕 p.60-61)

> [!note]- 정답 확인 (클릭)
> **첫 번째 호출은 컴파일 에러, 두 번째는 성공** — `0`은 `int`로 추론되어 `std::shared_ptr<Widget>`에 전달 불가. `nullptr`은 `std::nullptr_t`로 추론되어 모든 포인터 타입으로 implicit conversion 가능하므로 성공. (📕 p.60-61)

---

## 🔥 함정 문제 (5문제)

> [!question] **Q1.** ⭐⭐⭐ 다음 코드에서 `Widget` 클래스에 `std::initializer_list<bool>` 생성자가 있다면, `Widget w{0};`는 어떤 생성자를 호출하는가?
> ```cpp
> class Widget {
> public:
>     Widget(int i);                              // (A)
>     Widget(std::initializer_list<bool> il);      // (B)
> };
> Widget w{0}; // ???
> ```
> (📕 p.53-54)

> [!note]- 정답 확인 (클릭)
> **컴파일 에러!** — Braced initializer는 `std::initializer_list` 생성자에 **최우선 매칭**을 시도한다. `int 0`을 `bool`로 변환하려 하지만 이는 **narrowing conversion** (`int` --> `bool`)이므로 braced initialization에서 금지. 결과적으로 (B)에 매칭 시도하되 narrowing으로 실패, (A)는 고려되지 않아 **컴파일 에러**. `Widget w(0);`으로 하면 (A)가 정상 호출된다. (📕 p.53-54)

---

> [!question] **Q2.** ⭐⭐⭐ 다음 상황에서 A와 B 모두 `std::shared_ptr`로 서로를 가리키고, 외부에서 모든 참조가 제거되면 어떻게 되는가?
> ```cpp
> class B; // forward declaration
> class A {
> public:
>     std::shared_ptr<B> bPtr;
> };
> class B {
> public:
>     std::shared_ptr<A> aPtr;
> };
>
> {
>     auto a = std::make_shared<A>();
>     auto b = std::make_shared<B>();
>     a->bPtr = b;
>     b->aPtr = a;
> } // a, b 스코프 밖으로
> ```
> (📕 p.136-137)

> [!note]- 정답 확인 (클릭)
> **메모리 누수 -- A와 B 모두 영원히 해제되지 않는다!** — 스코프를 벗어나면 `a`, `b`의 ref count는 2에서 1로 감소하지만, `A->bPtr`가 B를, `B->aPtr`가 A를 여전히 가리키므로 **ref count가 0이 되지 않는다**. 순환 참조(cycle). 해결: B의 `aPtr`을 `std::weak_ptr<A>`로 변경. (📕 p.136-137)

---

> [!question] **Q3.** ⭐⭐⭐ 다음 코드에서 `StringTable` 클래스에 destructor를 추가한 뒤 성능이 급격히 저하된 이유는?
> ```cpp
> class StringTable {
> public:
>     StringTable() { makeLogEntry("Creating StringTable object"); }
>     ~StringTable() { makeLogEntry("Destroying StringTable object"); } // 추가!
> private:
>     std::map<int, std::string> values;
> };
> ```
> (📕 p.112-113)

> [!note]- 정답 확인 (클릭)
> **Destructor 선언으로 move 연산 자동 생성이 억제되어 모든 "move"가 copy로 대체됨** — User-declared destructor가 존재하면 move constructor와 move assignment operator가 자동 생성되지 않는다. `std::map<int, std::string>`의 복사는 이동보다 **수 배~수십 배 느리다**. 해결: `StringTable(StringTable&&) = default;`와 `StringTable& operator=(StringTable&&) = default;`를 명시적으로 선언. (📕 p.112-113)

---

> [!question] **Q4.** ⭐⭐⭐ `std::make_shared<std::vector<int>>(10, 20)`의 결과 벡터의 크기는?
> (📕 p.143)

> [!note]- 정답 확인 (클릭)
> **10 (원소 10개, 각 값 20)** — Make 함수는 내부적으로 **괄호 `()`를 사용**하여 perfect forwarding한다. 따라서 `std::vector<int>(10, 20)`과 동일하게 작동: 값 20인 원소 10개. Braced initializer(`{10, 20}`)로 만들고 싶다면 먼저 `auto initList = {10, 20};`를 만든 뒤 `std::make_shared<std::vector<int>>(initList);`로 전달해야 한다. (📕 p.143)

---

> [!question] **Q5.** ⭐⭐⭐ 다음 코드에서 `Widget::process()`의 문제점은?
> ```cpp
> std::vector<std::shared_ptr<Widget>> processedWidgets;
>
> class Widget {
> public:
>     void process() {
>         // ... 처리 ...
>         processedWidgets.emplace_back(this); // ???
>     }
> };
> ```
> (📕 p.130-131)

> [!note]- 정답 확인 (클릭)
> **`this` (raw pointer)로 새 `shared_ptr` 생성 시 새 control block이 만들어져 이중 해제 (UB)** — 이미 외부에서 `shared_ptr`이 이 객체를 관리하고 있다면, `this`로 새로운 `shared_ptr`을 만들면 **별도의 control block**이 생성된다. 해결: `std::enable_shared_from_this<Widget>`를 상속받고, `processedWidgets.emplace_back(shared_from_this());`를 사용. (📕 p.130-131)

---

## 🎯 시나리오 문제 (5문제)

> [!question] **Q1.** ⭐ 팩토리 함수가 힙에 객체를 생성해서 반환하는데, 호출자가 exclusive ownership을 가진다. 어떤 스마트 포인터를 반환해야 하는가?
> (📕 p.119)

> [!note]- 정답 확인 (클릭)
> **`std::unique_ptr`** — Exclusive ownership이므로 `std::unique_ptr`가 적합. 호출자가 나중에 shared ownership이 필요하면 `std::shared_ptr`로 변환 가능. Factory 함수는 항상 가장 제한적인(lightweight) 스마트 포인터를 반환하는 것이 좋다. (📕 p.119)

---

> [!question] **Q2.** ⭐⭐ 캐시 시스템을 구현하는데, 캐시된 객체는 클라이언트가 사용 중이면 살아있어야 하고 모든 클라이언트가 사용을 마치면 자동 해제되어야 한다. 캐시 자체는 객체 소유에 참여하지 않아야 한다. 캐시에 저장할 포인터의 타입은?
> (📕 p.136)

> [!note]- 정답 확인 (클릭)
> **캐시에는 `std::weak_ptr` 저장, 클라이언트에게는 `std::shared_ptr` 반환** — `weak_ptr`는 reference count에 영향을 주지 않으므로 캐시가 소유권에 참여하지 않는다. `expired()` 또는 `lock()`으로 객체 생존 여부를 확인하고, 만료되면 새로 로드. (📕 p.136)

---

> [!question] **Q3.** ⭐⭐ Observer 패턴에서 Subject가 Observer 목록을 관리하는데, Observer가 언제든 파괴될 수 있다. Subject는 Observer의 수명을 제어하지 않지만 dangling pointer를 방지하고 싶다. 어떤 포인터를 사용해야 하는가?
> (📕 p.137)

> [!note]- 정답 확인 (클릭)
> **`std::weak_ptr<Observer>`** — Subject가 Observer의 수명을 제어하지 않으므로 `shared_ptr`는 부적절 (수명 연장). Raw pointer는 dangling 감지 불가. `weak_ptr`는 `expired()` 체크로 dangling 감지 가능하며 reference count에 영향 없음. (📕 p.137)

---

> [!question] **Q4.** ⭐⭐ 트리 자료구조에서 부모 --> 자식 포인터와 자식 --> 부모 포인터에 각각 어떤 포인터를 사용해야 하는가?
> (📕 p.137-138)

> [!note]- 정답 확인 (클릭)
> **부모 --> 자식: `std::unique_ptr` / 자식 --> 부모: raw pointer** — 부모가 자식을 exclusive하게 소유하므로 `unique_ptr`. 자식은 부모보다 먼저 파괴되지 않으므로 dangling 위험이 없어 raw pointer로 충분 (오버헤드 없음). 순환 참조도 발생하지 않는다. (📕 p.137-138)

---

> [!question] **Q5.** ⭐⭐⭐ Pimpl Idiom을 구현하면서 컴파일 의존성을 줄이고 싶다. `std::unique_ptr<Impl>`를 사용할 때, 헤더 파일에 반드시 선언해야 하지만 .cpp에서 정의해야 하는 특수 멤버 함수들은 무엇인가?
> (📕 p.151-153)

> [!note]- 정답 확인 (클릭)
> **Destructor, Move constructor, Move assignment operator** — 세 함수 모두 `unique_ptr<Impl>`을 파괴하거나 이동시키는 코드를 생성하는데, 이때 `Impl`이 complete type이어야 한다. 헤더에서 inline으로 생성되면 incomplete type 에러. Copy 연산은 deep copy 로직을 직접 작성해야 하므로 당연히 .cpp에서 구현. (📕 p.151-153)

---

## 📝 빈칸 채우기 (5문제)

> [!question] **Q1.** ⭐
> Move 연산이 자동 생성되려면, 클래스에 user-declared \_\_\_\_\_ 연산, \_\_\_\_\_ 연산, \_\_\_\_\_ 가 모두 없어야 한다.
> (📕 p.111)

> [!note]- 정답 확인 (클릭)
> **copy / move / destructor** — "Move operations are generated for classes (when needed) only if these three things are true: No copy operations are declared, No move operations are declared, No destructor is declared." (📕 p.111)

---

> [!question] **Q2.** ⭐⭐
> `std::shared_ptr`의 크기가 raw pointer의 2배인 이유는 내부에 resource를 가리키는 포인터와 \_\_\_\_\_\_을 가리키는 포인터가 있기 때문이다.
> (📕 p.125-128)

> [!note]- 정답 확인 (클릭)
> **control block** — Control block에는 reference count, weak count, custom deleter, custom allocator 등이 포함된다. (📕 p.125-128)

---

> [!question] **Q3.** ⭐⭐
> `std::make_shared`의 효율성 이점은 객체와 \_\_\_\_\_\_을 \_\_\_\_\_\_ 메모리 할당으로 처리하기 때문이다.
> (📕 p.142)

> [!note]- 정답 확인 (클릭)
> **control block / 단일 (single)** — `new`를 직접 사용하면 객체를 위한 할당과 control block을 위한 할당, 총 2회 필요. `make_shared`는 1회로 줄인다. (📕 p.142)

---

> [!question] **Q4.** ⭐⭐
> 클래스 멤버 함수 안에서 `this`의 `std::shared_ptr`를 안전하게 생성하려면, 클래스가 `std::enable_shared_from_this<T>`를 상속받고 \_\_\_\_\_\_\_\_\_ 함수를 호출해야 한다.
> (📕 p.131)

> [!note]- 정답 확인 (클릭)
> **`shared_from_this()`** — 이 함수는 현재 객체의 기존 control block을 찾아 새 `shared_ptr`를 생성하므로 이중 control block 문제를 방지한다. (📕 p.131)

---

> [!question] **Q5.** ⭐⭐⭐
> `constexpr` 함수는 컴파일 타임 상수 인자로 호출하면 \_\_\_\_\_\_ 결과를 생성하고, 런타임 값으로 호출하면 \_\_\_\_\_\_ 결과를 생성한다. C++11에서 `constexpr` 함수는 \_\_\_\_\_\_문 하나만 포함할 수 있었지만, C++14에서 이 제약이 \_\_\_\_\_\_되었다.
> (📕 p.97-100)

> [!note]- 정답 확인 (클릭)
> **컴파일 타임 (compile-time) / 런타임 (runtime) / return / 완화 (relaxed)** — C++14에서는 루프, 지역 변수, 여러 statement 등이 허용되어 `constexpr` 함수의 표현력이 대폭 향상되었다. (📕 p.97-100)

---

> **학습 팁**: 각 Item의 "Things to Remember" 섹션을 먼저 암기하고, 코드 예제를 직접 타이핑하며 컴파일해보는 것이 가장 효과적입니다. 특히 braced init + `initializer_list`, 특수 멤버 함수 생성 규칙, `shared_ptr` control block은 실무에서 자주 만나는 함정이므로 완벽히 이해하세요.
