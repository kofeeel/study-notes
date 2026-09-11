# 02. C++ (24장)

[← 목차](README.md)

---

## 객체 · 다형성

<details>
<summary><b>Q1. 가상함수는 어떻게 동작하나? (vtable)</b></summary>

가상함수가 있는 클래스는 클래스당 1개의 **vtable**(함수 포인터 배열)을 갖고, 객체마다 **vptr** 1개가 그걸 가리킴. 부모 포인터로 호출하면 런타임에 vptr → vtable → 실제 함수를 찾아 호출(동적 바인딩). 비용: 간접 호출 1회 + 인라인 불가 + 객체당 포인터 8바이트.

</details>

<details>
<summary><b>Q2. 가상 소멸자는 왜 필요한가?</b></summary>

부모 포인터로 자식 객체를 `delete`할 때 소멸자가 가상이 아니면 정적 바인딩으로 **부모 소멸자만** 호출돼 자식의 자원이 누수(동작은 미정의). 다형성 기반 클래스엔 항상 `virtual ~Base()`. 반대로 STL 컨테이너는 가상 소멸자가 없으므로 상속해서 다형적으로 쓰지 말 것.

</details>

<details>
<summary><b>Q3. 생성자 안에서 가상함수를 호출하면?</b></summary>

부모 생성자가 도는 동안 객체는 아직 "부모 타입"이라 vptr이 부모 vtable을 가리킴 → 자식 오버라이드가 **호출되지 않음**. 이유는 자식 멤버가 아직 초기화되지 않은 상태에서 자식 코드를 돌리면 미정의 동작이라서. UE의 `BeginPlay`/`PostInitializeComponents`가 이 문제를 우회하는 초기화 단계.

</details>

<details>
<summary><b>Q4. 순수 가상함수와 추상 클래스는?</b></summary>

`virtual void f() = 0;` 이 하나라도 있으면 추상 클래스로 인스턴스화 불가. 인터페이스를 강제하는 수단. 자식이 전부 구현해야 구체 클래스가 됨.

</details>

<details>
<summary><b>Q5. override와 final 키워드는 왜 쓰나?</b></summary>

- `override`: 부모 가상함수를 정말 오버라이드하는지 컴파일러가 검사. 없으면 시그니처 오타가 조용히 **새 함수**가 되어 버그. UE에서 항상 붙임.
- `final`: 더 이상 오버라이드 금지. 디바이스 최적화(devirtualization) 힌트지만 mock·확장을 막으므로 신중히.

</details>

<details>
<summary><b>Q6. 컴파일러가 자동 생성하는 특수 멤버 함수는?</b></summary>

C++11 기준 6개: 기본 생성자, 소멸자, 복사 생성자, 복사 대입, 이동 생성자, 이동 대입. **사용될 때만(lazy)** 생성됨. const나 참조 멤버가 있으면 복사 대입은 자동 생성되지 않음(deleted). 복사 금지는 `= delete`가 정답.

</details>

## 자원 관리

<details>
<summary><b>Q7. RAII란?</b></summary>

Resource Acquisition Is Initialization. 자원 획득을 객체 생성(생성자)에, 해제를 소멸자에 묶어 **스코프를 벗어나면 자동 해제**되게 하는 기법. 예외가 나도 스택 풀림 중 소멸자가 호출돼 누수 없음. 스마트 포인터, `lock_guard`, 파일 핸들 래퍼가 전부 RAII.

</details>

<details>
<summary><b>Q8. unique_ptr / shared_ptr / weak_ptr 차이는?</b></summary>

- **unique_ptr**: 단독 소유. 복사 불가, 이동만 가능. 오버헤드 0(raw 포인터와 동일).
- **shared_ptr**: 공유 소유. 참조 카운트를 컨트롤 블록에 두고 **원자적**으로 증감(멀티스레드 안전, 대신 비용). 카운트 0에 삭제.
- **weak_ptr**: 소유하지 않는 관찰자. `lock()`으로 살아있으면 shared_ptr 획득. **순환 참조** 끊는 용도.

</details>

<details>
<summary><b>Q9. shared_ptr 순환 참조는 왜 누수인가?</b></summary>

A가 shared_ptr<B>, B가 shared_ptr<A>를 들면 서로의 카운트가 1 이하로 안 떨어져 둘 다 영원히 삭제 안 됨. 한쪽(보통 자식→부모)을 `weak_ptr`로 바꿔 해결.

</details>

<details>
<summary><b>Q10. make_shared를 쓰는 이유는?</b></summary>

① 객체와 컨트롤 블록을 **한 번의 할당**으로 붙여 배치(캐시 친화, 할당 1회) ② `f(shared_ptr<T>(new T), g())` 처럼 인자 평가 순서 때문에 예외 시 누수될 수 있는 문제를 원천 차단.

</details>

<details>
<summary><b>Q11. new/delete 짝을 안 맞추면? (new[] vs delete)</b></summary>

`new[]`로 만든 배열을 `delete`로 지우면 원소 소멸자가 일부만 돌거나 힙이 깨지는 미정의 동작. 상위 교훈: raw new/delete를 직접 쓰지 말고 `make_unique`, `std::vector`를 쓸 것.

</details>

## Modern C++

<details>
<summary><b>Q12. lvalue와 rvalue, std::move란?</b></summary>

- **lvalue**: 이름이 있고 주소를 취할 수 있는 값(`x`). **rvalue**: 임시값(`x+y`, `func()`, 리터럴).
- `std::move`는 실제로 옮기지 않고 **rvalue로 캐스팅**만 함. 그래서 이동 생성자/대입이 선택되어 자원을 "훔쳐"옴. move 후 원본은 유효하지만 값은 미지정 상태.

</details>

<details>
<summary><b>Q13. 이동 시맨틱이 왜 성능에 도움이 되나?</b></summary>

복사는 깊은 복사(새 버퍼 할당 + 원소 복사)지만 이동은 포인터 몇 개만 바꿔치기. `vector<string>` 재할당·함수 반환·`push_back(std::move(s))` 등에서 힙 할당을 제거. `noexcept`가 붙은 이동 생성자여야 vector 재할당 시 이동을 선택함.

</details>

<details>
<summary><b>Q14. RVO/NRVO란?</b></summary>

함수가 지역 객체를 값으로 반환할 때 컴파일러가 복사/이동을 생략하고 호출자 메모리에 **직접 생성**하는 최적화. C++17부터 임시값 반환은 생략이 보장됨. 그래서 `return std::move(local);`은 오히려 NRVO를 막는 안티패턴.

</details>

<details>
<summary><b>Q15. auto와 decltype 차이는?</b></summary>

`auto`는 템플릿 인자 추론 규칙이라 **const와 참조가 decay**됨(`auto a = constRef;` → 복사). `decltype`은 표현식 타입을 그대로 보존. 참조를 유지하려면 `const auto&`로 명시.

</details>

<details>
<summary><b>Q16. 람다의 정체와 캡처 주의점은?</b></summary>

컴파일러가 만든 익명 클래스(클로저)의 인스턴스로 `operator()`를 가짐. 캡처: `[x]` 값, `[&x]` 참조, `[=]`/`[&]` 전부.
함정: 함수 밖으로 나가는 람다(콜백, 스레드)에 `[&]` 참조 캡처하면 원본 소멸 후 댕글링. 저장·전달은 `std::function` 또는 템플릿.

</details>

<details>
<summary><b>Q17. nullptr이 NULL보다 나은 이유는?</b></summary>

`NULL`은 정수 0이라 `f(int)`와 `f(T*)` 오버로딩에서 int 쪽이 선택되는 모호함. `nullptr`은 전용 타입 `std::nullptr_t`라 포인터로만 변환됨.

</details>

<details>
<summary><b>Q18. constexpr / explicit / noexcept 각각 한 줄로.</b></summary>

- `constexpr`: 컴파일 타임 계산 가능하게. 런타임 인자면 일반 함수처럼 동작.
- `explicit`: 단일 인자 생성자의 암시적 형변환 방지(`Vec v = 3;` 차단).
- `noexcept`: 예외 안 던짐을 보장 → 이동 최적화 선택, 컴파일러 최적화. 어기면 `std::terminate`.

</details>

## const · 초기화 · 기타

<details>
<summary><b>Q19. const int* p 와 int* const p 차이는?</b></summary>

`const int* p` = 가리키는 **값**을 못 바꿈(포인터는 이동 가능). `int* const p` = **포인터** 자체를 못 바꿈. 별표 기준으로 왼쪽이면 값, 오른쪽이면 포인터. const 멤버 함수(`void f() const`)는 멤버 변경 금지, 캐시처럼 논리적으로 const면 `mutable`.

</details>

<details>
<summary><b>Q20. 멤버 초기화 리스트를 쓰는 이유는?</b></summary>

본문 대입은 기본 생성 후 대입으로 2번 일어나지만 초기화 리스트는 1번에 초기화. const 멤버, 참조 멤버, 기본 생성자 없는 멤버는 초기화 리스트로만 가능. 초기화 순서는 리스트 순서가 아니라 **선언 순서**.

</details>

<details>
<summary><b>Q21. 정적 초기화 순서 문제(SIOF)와 해결은?</b></summary>

서로 다른 번역 단위의 전역/static 객체 초기화 순서는 미보장이라 한쪽이 다른 쪽을 먼저 참조하면 UB. 해결: 함수 안 지역 static으로 감싸는 **Meyer's Singleton** — 첫 호출 때 초기화되어 순서가 보장됨(C++11부터 스레드 안전).

</details>

<details>
<summary><b>Q22. 템플릿과 가상함수(런타임 다형성)의 차이는?</b></summary>

템플릿은 **컴파일 타임** 다형성 — 타입별 코드 생성, 인라인 가능해 빠르지만 바이너리 커지고 컴파일 느림. 가상함수는 **런타임** 다형성 — 하나의 코드로 여러 타입을 포인터로 다루지만 간접 호출 비용. 타입이 컴파일 시점에 정해지면 템플릿, 실행 중 바뀌면 가상함수.

</details>

<details>
<summary><b>Q23. 메모리 정렬(alignment)과 패딩이란?</b></summary>

CPU가 타입 크기 배수 주소에서 읽는 게 빠르거나 필수라, 컴파일러가 멤버 사이에 패딩을 넣음. `struct {char a; int b; char c;}`는 12바이트, 순서를 `int, char, char`로 바꾸면 8바이트. 게임에서 큰 배열 구조체는 멤버 순서로 메모리·캐시 절약.

</details>

<details>
<summary><b>Q24. 얕은 복사와 깊은 복사 차이는?</b></summary>

얕은 복사는 포인터 값만 복사해 두 객체가 같은 힙을 가리킴 → 이중 해제·댕글링. 깊은 복사는 새 버퍼를 할당해 내용을 복사. 자원을 직접 소유하는 클래스는 복사 생성자/대입/소멸자를 함께 정의(Rule of 3/5)하거나, 스마트 포인터·vector에 맡겨 아예 안 쓰는 게 Rule of Zero.

</details>

---

원본: `../02-CPP-알고리즘.md`, wiki `cpp-interview.md` / `c.md`, `../CPP/_이전문서/QUIZ-EMC-*.md`
