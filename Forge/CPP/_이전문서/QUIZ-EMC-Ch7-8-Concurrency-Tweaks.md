# Effective Modern C++ -- Ch.7~8 Concurrency API & Tweaks 마스터 퀴즈

> **범위**: Item 35~42 (Chapter 7: The Concurrency API + Chapter 8: Tweaks)
> **난이도**: ★ Easy --> ★★★ Hard 로 점진적 상승
> **사용법**: 각 문제를 풀고 접힌 답안을 확인하세요. 페이지 참조(📕)로 원서를 복습하세요.

---

## 📖 핵심 개념 요약

### Item 35: Prefer task-based programming to thread-based (📕 p.241-245)

> [!question] 왜 알아야 하나?
> `std::thread`를 직접 쓰면 예외 처리, 스레드 풀, 반환값 전달을 전부 직접 구현해야 한다. `std::async` + `std::future`가 이걸 다 해준다. 게임 서버에서 비동기 IO 처리할 때 thread를 직접 만들면 스레드 수 관리, oversubscription, thread exhaustion 전부 개발자 책임이 되어 관리 지옥에 빠진다.

> [!danger] 모르면 이런 버그가 난다
> `std::thread`에서 예외가 던져지면 호출 스레드로 전파되지 않고 `std::terminate()`로 프로그램이 즉사한다. 요청마다 `std::thread`를 생성하는 서버는 hardware thread 수를 초과하는 순간 context switch 비용이 폭증하고 CPU cache 효율이 바닥을 친다.

> [!tip] 한 줄 핵심
> "스레드를 직접 관리하지 마라 -- `std::async`에 맡기면 반환값, 예외, 스레드 수 관리를 런타임이 알아서 해준다."

### Item 36: Specify std::launch::async if asynchronicity is essential (📕 p.245-250)

> [!question] 왜 알아야 하나?
> `std::async`의 default policy는 `async | deferred`라서, 런타임이 "지금 바쁘니까 나중에 할게"라고 결정하면 실제로는 `get()`/`wait()` 호출 시점에 호출 스레드에서 동기 실행된다. 비동기라고 믿고 짠 코드가 사실은 동기로 돌 수 있다.

> [!danger] 모르면 이런 버그가 난다
> `wait_for()` 루프가 무한 루프에 빠진다. Deferred task는 `wait_for`가 항상 `std::future_status::deferred`를 반환하므로 `ready`가 절대 되지 않는다. 서버에서 이러면 스레드 하나가 영원히 spinning하며 CPU 100%.

> [!tip] 한 줄 핵심
> "비동기 실행이 반드시 필요하면 `std::launch::async`를 명시하라 -- default policy는 동기 실행을 선택할 수 있다."

### Item 37: Make std::threads unjoinable on all paths (📕 p.250-257)

> [!question] 왜 알아야 하나?
> Joinable한 `std::thread`가 소멸자에서 파괴되면 `std::terminate()` 호출로 프로그램이 즉시 종료된다. 예외 경로에서 `join`을 빼먹으면 정상 경로에서는 잘 돌다가 예외가 한 번만 발생하면 크래시. 재현이 어렵고 디버깅이 고통스러운 종류의 버그다.

> [!danger] 모르면 이런 버그가 난다
> 함수 중간에 early return이나 예외가 발생하면 `std::thread`가 joinable 상태로 스코프를 빠져나간다. Destructor에서 `std::terminate()` -- 프로그램 즉사. Implicit join은 예측 불가한 성능 저하, implicit detach는 이미 파괴된 스택 변수에 대한 dangling reference로 메모리 오염.

> [!tip] 한 줄 핵심
> "ThreadRAII 패턴으로 모든 코드 경로에서 `std::thread`가 unjoinable하게 소멸되도록 보장하라."

### Item 38: Be aware of varying thread handle destructor behavior (📕 p.257-262)

> [!question] 왜 알아야 하나?
> `std::async`로 만든 future가 스코프를 빠져나가면 task가 끝날 때까지 blocking할 수 있다. 이걸 모르면 "왜 이 함수가 10초나 걸리지?"라는 미스터리한 성능 문제를 만난다. Future의 소멸자 동작이 생성 방식(async vs packaged_task)에 따라 완전히 다르다.

> [!danger] 모르면 이런 버그가 난다
> `std::async(std::launch::async, ...)`로 만든 future를 로컬 변수로 잡았다가 스코프 끝에서 소멸하면, task 완료까지 implicit join이 발생한다. 10초짜리 task면 함수가 10초간 멈춘다. 프로파일러로 봐도 "어디서 블로킹되는지" 찾기 어렵다.

> [!tip] 한 줄 핵심
> "`std::async`의 마지막 future 소멸자는 implicit join한다 -- future의 lifetime을 의식적으로 관리하라."

### Item 39: Consider void futures for one-shot event communication (📕 p.262-270)

> [!question] 왜 알아야 하나?
> 스레드 간 "준비 완료" 같은 일회성 신호를 보낼 때, condition variable은 mutex 필요 + notify/wait 순서 문제 + spurious wakeup 이라는 삼중고를 안고 있다. `std::promise<void>` + `std::future<void>`는 이 세 문제를 전부 해결한다.

> [!danger] 모르면 이런 버그가 난다
> Condvar로 one-shot event를 구현하면: notify가 wait보다 먼저 호출되면 reacting task가 영원히 대기(deadlock), spurious wakeup으로 아직 준비 안 된 데이터에 접근(data race). Flag polling은 CPU를 100% 점유하며 busy-wait.

> [!tip] 한 줄 핵심
> "일회성 스레드 간 신호에는 `std::promise<void>` + `std::future<void>` -- mutex 불필요, 순서 무관, spurious wakeup 없음."

### Item 40: Use std::atomic for concurrency, volatile for special memory (📕 p.271-279)

> [!question] 왜 알아야 하나?
> Java/C# 경험자가 C++에 오면 `volatile`을 동기화에 쓰는 실수를 거의 반드시 저지른다. C++에서 `volatile`은 memory-mapped IO용이지 동기화용이 아니다. 동기화는 `std::atomic`. 이 구분을 모르면 data race를 만들어놓고 "volatile 썼으니 안전하다"고 착각한다.

> [!danger] 모르면 이런 버그가 난다
> `volatile int flag`로 스레드 간 신호를 보내면: (1) atomicity가 보장되지 않아 torn read/write 가능, (2) 컴파일러/하드웨어가 명령을 재배치하여 flag 설정 전에 데이터가 준비되지 않은 상태에서 다른 스레드가 데이터에 접근. 둘 다 undefined behavior이고, 릴리즈 빌드에서만 발생하는 하이젠버그가 된다.

> [!tip] 한 줄 핵심
> "`std::atomic` = 동기화(atomicity + ordering), `volatile` = 특수 메모리(최적화 방지). Java/C#의 volatile은 C++의 `std::atomic`이다."

### Item 41: Consider pass by value for copyable parameters that are cheap to move and always copied (📕 p.281-291)

> [!question] 왜 알아야 하나?
> "pass by value가 Modern C++에서는 효율적이다"라는 말을 반쯤만 이해하고 모든 곳에 적용하면 성능이 오히려 나빠진다. Pass by value는 4가지 조건(copyable, cheap to move, always copied, no slicing)을 전부 만족할 때만 고려할 가치가 있고, assignment로 복사하는 경우에는 기존 메모리 재활용 기회를 잃어 추가 allocation이 발생한다.

> [!danger] 모르면 이런 버그가 난다
> `changeTo(std::string newPwd) { text = std::move(newPwd); }` -- lvalue 호출 시 기존 `text`의 메모리를 재활용할 수 없어 매번 allocation + deallocation. `const std::string&` 오버로드였으면 기존 capacity 내에서 복사 가능. 대량 호출 시 성능 차이가 유의미하다. 상속 구조에서는 slicing problem으로 derived class 정보가 잘려나간다.

> [!tip] 한 줄 핵심
> "Pass by value는 만능이 아니다 -- copyable + cheap to move + always copied + no slicing 네 조건을 전부 확인하라."

### Item 42: Consider emplacement instead of insertion (📕 p.292-301)

> [!question] 왜 알아야 하나?
> `push_back(MyClass(args))`는 임시 객체 생성 + move + 임시 객체 소멸이라는 불필요한 단계를 거친다. `emplace_back(args)`는 컨테이너 안에서 직접 생성하므로 임시 객체가 아예 없다. 대량 삽입 시 성능 차이가 유의미하고, 특히 인자 타입이 컨테이너 원소 타입과 다를 때(예: `const char*` --> `std::string`) 효과가 크다.

> [!danger] 모르면 이런 버그가 난다
> `ptrs.emplace_back(new Widget, killWidget)` -- node 할당 실패 시 raw pointer가 leak (push_back은 임시 shared_ptr이 먼저 소유권을 잡으므로 안전). `regexes.emplace_back(nullptr)` -- explicit 생성자를 direct initialization으로 우회하여 컴파일은 되지만 런타임 UB. Emplacement의 "더 강력한 초기화"가 의도치 않은 변환을 허용하는 함정.

> [!tip] 한 줄 핵심
> "타입 불일치 + construction + 비중복일 때 emplace가 유리하지만, exception safety와 explicit 생성자 함정을 반드시 점검하라."

---

## 🧠 OX 퀴즈 (10문제) -- ★ Easy

> [!tip] 규칙
> O(맞다) 또는 X(틀리다)로 답한 뒤, 접힌 답안을 확인하세요.

### Q1.
> [!question] `std::thread`로 비동기 실행한 함수가 예외를 던지면, 해당 예외는 호출 스레드에서 `try-catch`로 잡을 수 있다.

> [!note]- 정답 확인 (클릭)
> **X** — `std::thread`에서 예외가 발생하면 `std::terminate`가 호출되어 프로그램이 종료된다. 반면 `std::async`를 사용하면 `future::get()`을 통해 예외를 호출 스레드로 전파할 수 있다. (📕 p.242)

### Q2.
> [!question] `std::async`의 default launch policy는 `std::launch::async`이므로, 항상 새로운 스레드에서 비동기 실행된다.

> [!note]- 정답 확인 (클릭)
> **X** — Default policy는 `std::launch::async | std::launch::deferred`이다. 런타임 시스템이 비동기 또는 동기(deferred) 실행을 선택한다. 반드시 비동기가 필요하면 `std::launch::async`를 명시해야 한다. (📕 p.246)

### Q3.
> [!question] Joinable한 `std::thread`의 destructor가 호출되면, 자동으로 `join`이 수행된다.

> [!note]- 정답 확인 (클릭)
> **X** — Joinable한 `std::thread`의 destructor는 `std::terminate`를 호출하여 프로그램을 종료한다. Implicit join과 implicit detach 모두 더 나쁜 결과를 초래할 수 있어 표준 위원회가 종료를 택했다. (📕 p.251-252)

### Q4.
> [!question] `volatile int`에 대한 `++` 연산은 멀티스레드 환경에서 atomic하게 수행된다.

> [!note]- 정답 확인 (클릭)
> **X** — `volatile`은 atomicity를 전혀 보장하지 않는다. `volatile int vc`를 두 스레드에서 동시에 `++`하면 data race(undefined behavior)가 발생한다. Atomic 연산이 필요하면 `std::atomic<int>`를 사용해야 한다. (📕 p.272-273)

### Q5.
> [!question] `std::atomic`은 컴파일러의 코드 재배치(reordering)를 제한하지만, `volatile`은 제한하지 않는다.

> [!note]- 정답 확인 (클릭)
> **O** — `std::atomic`에 대한 쓰기 이전의 코드가 쓰기 이후로 재배치되지 않는다 (sequential consistency). `volatile`에는 이런 보장이 없어 컴파일러나 하드웨어가 명령을 재배치할 수 있다. (📕 p.273-275)

### Q6.
> [!question] `std::promise<void>`와 `std::future<void>`를 이용한 이벤트 통신은 여러 번 반복 사용할 수 있다.

> [!note]- 정답 확인 (클릭)
> **X** — `std::promise`는 **한 번만 set** 가능하다. 이 방식은 one-shot 통신에만 적합하다. 반복적 이벤트 통신이 필요하면 condition variable이나 flag를 사용해야 한다. (📕 p.268)

### Q7.
> [!question] `emplace_back`은 항상 `push_back`보다 빠르거나 같다.

> [!note]- 정답 확인 (클릭)
> **X** — 이론적으로는 그렇지만, 실제 Standard Library 구현에서는 insertion이 더 빠른 경우도 있다. 타입, 컨테이너, 삽입 위치, exception safety 등 여러 요인에 따라 다르므로 benchmark가 필요하다. (📕 p.295)

### Q8.
> [!question] Pass by value 방식에서 rvalue 인자를 전달하면, by-reference 방식 대비 move 연산이 1회 추가된다.

> [!note]- 정답 확인 (클릭)
> **O** — Pass by value: 파라미터 move construction 1회 + 함수 내부 move 1회 = 2 moves. By-reference: move 1회만. rvalue 인자에서도 move 1회가 추가된다. (📕 p.285)

### Q9.
> [!question] `std::async`로 생성된 모든 future의 destructor는 task 완료까지 block한다.

> [!note]- 정답 확인 (클릭)
> **X** — Block하는 것은 매우 특수한 경우뿐: (1) `std::async`로 생성된 shared state이고, (2) launch policy가 `std::launch::async`이며, (3) 해당 shared state를 참조하는 **마지막 future**일 때만. 다른 모든 future의 destructor는 단순히 데이터 멤버를 파괴한다. (📕 p.259-260)

### Q10.
> [!question] `volatile std::atomic<int>`처럼 `volatile`과 `std::atomic`을 동시에 사용하는 것은 합법적이다.

> [!note]- 정답 확인 (클릭)
> **O** — 둘은 목적이 다르므로 함께 사용 가능하다. memory-mapped I/O 위치에 여러 스레드가 동시 접근하는 경우 유용하다. `std::atomic`이 atomicity를, `volatile`이 최적화 방지를 담당한다. (📕 p.279)

---

## 💻 코드 분석 퀴즈 (6문제) -- ★★ Medium

### Q11. 이 비동기 코드의 문제점은?

```cpp
void doWork() {
    std::thread t([] {
        // 시간이 오래 걸리는 작업
        heavyComputation();
    });
    
    if (!conditionsAreSatisfied()) {
        return;  // 조건 불만족 시 조기 리턴
    }
    
    t.join();
    useResults();
}
```

> [!note]- 정답 확인 (클릭)
> `conditionsAreSatisfied()`가 `false`를 반환하거나 **예외를 던지면**, `t`가 joinable 상태인 채로 destructor가 호출되어 **`std::terminate` -- 프로그램 종료**. 모든 경로에서 `std::thread`를 unjoinable하게 만들어야 한다. **ThreadRAII** 패턴을 사용하거나, 조기 리턴 전에 `t.join()` 또는 `t.detach()`를 호출해야 한다. (📕 p.251-252)

### Q12. `atomic` vs `volatile` -- 이 코드의 출력은?

```cpp
// Thread A:
std::atomic<bool> ready(false);
int data = 0;

data = 42;
ready = true;

// Thread B:
while (!ready) {}
assert(data == 42);  // 이 assert는 안전한가?
```

> [!note]- 정답 확인 (클릭)
> **안전하다.** `std::atomic`에 대한 `ready = true` 쓰기는 sequential consistency를 보장한다. `ready`에 쓰기 이전의 모든 코드(`data = 42`)가 다른 스레드에서도 `ready` 변경 이전에 관찰된다. 만약 `ready`가 `volatile bool`이었다면, **컴파일러나 하드웨어가 `data = 42`와 `ready = true`의 순서를 바꿀 수 있어** assert가 실패할 수 있다. (📕 p.273-275)

### Q13. 이 future 코드가 hang하는 이유는?

```cpp
void detect() {
    ThreadRAII tr(
        std::thread([] {
            p.get_future().wait();
            react();
        }),
        ThreadRAII::DtorAction::join
    );
    
    // ... 여기서 예외 발생!
    
    p.set_value();  // 이 줄에 도달하지 못함
}
```

> [!note]- 정답 확인 (클릭)
> 예외가 발생하면 `p.set_value()`가 호출되지 않는다. 스레드는 `p.get_future().wait()`에서 **영원히 대기**한다. `ThreadRAII`의 destructor는 `join`을 시도하지만, 스레드가 끝나지 않으므로 **함수가 영원히 hang**한다. 해결: 예외 발생 시에도 `p.set_value()`를 호출하거나, interruptible thread 설계를 사용해야 한다. (📕 p.269)

### Q14. emplace_back의 exception safety 문제

```cpp
std::list<std::shared_ptr<Widget>> ptrs;

// 방법 A:
ptrs.push_back(std::shared_ptr<Widget>(new Widget, killWidget));

// 방법 B:
ptrs.emplace_back(new Widget, killWidget);
```

> [!question] 방법 A와 B 중 어느 것이 exception-safe한가? 그 이유는?

> [!note]- 정답 확인 (클릭)
> **방법 A가 exception-safe**하다. 방법 A에서는 임시 `shared_ptr`이 먼저 생성되어 `new Widget`의 소유권을 즉시 갖는다. list node 할당에서 예외가 발생해도 임시 `shared_ptr`의 destructor가 Widget을 해제한다. 방법 B(`emplace_back`)에서는 raw pointer가 perfect-forwarding으로 전달되는 과정에서 node 할당 실패 시 **raw pointer가 유실되어 메모리 leak**이 발생한다. 해결: `shared_ptr`을 미리 생성하고 `std::move`로 전달. (📕 p.296-298)

### Q15. pass by value의 숨겨진 비용

```cpp
class Password {
public:
    void changeTo(std::string newPwd) {    // pass by value
        text = std::move(newPwd);
    }
private:
    std::string text;  // 현재: "Supercalifragilisticexpialidocious"
};

// 호출:
std::string newPassword = "short";
p.changeTo(newPassword);
```

> [!question] 이 코드에서 pass by value 대신 `const std::string&` 오버로드를 사용했다면 어떤 이점이 있는가?

> [!note]- 정답 확인 (클릭)
> Pass by value: (1) `newPwd` copy construction 시 새 메모리 할당, (2) `text = std::move(newPwd)`에서 기존 text의 메모리 해제 = **할당 + 해제 2번**.
> `const std::string&` 오버로드 (`text = newPwd`): 기존 `text`의 capacity가 `newPwd`보다 크므로 **메모리 재할용 가능 -- 할당/해제 0번**. Assignment를 통한 복사에서 pass by value는 기존 메모리를 재활용할 기회를 잃는다. (📕 p.288-289)

### Q16. std::async의 default policy 함정

```cpp
using namespace std::literals;

auto fut = std::async([] {
    std::this_thread::sleep_for(1s);
    return 42;
});

while (fut.wait_for(100ms) != std::future_status::ready) {
    std::cout << "waiting...\n";
}

std::cout << fut.get() << "\n";
```

> [!question] 이 코드에 잠재적 문제가 있는가?

> [!note]- 정답 확인 (클릭)
> **무한 루프에 빠질 수 있다.** Default launch policy에서 런타임이 deferred 실행을 선택하면, `wait_for`는 항상 `std::future_status::deferred`를 반환한다. 이 값은 `ready`와 같지 않으므로 루프가 영원히 돈다. **해결**: 루프 진입 전 `fut.wait_for(0s) == std::future_status::deferred`를 체크하거나, `std::launch::async`를 명시적으로 지정한다. (📕 p.247-248)

---

## 🔥 함정 문제 (4문제) -- ★★★ Hard

### Q17. `std::thread` destructor의 UB 시나리오

```cpp
void process() {
    std::vector<int> data;
    
    std::thread t([&data] {
        for (int i = 0; i < 1000000; ++i)
            data.push_back(i);
    });
    
    t.detach();
}  // data가 파괴됨, 하지만 스레드는 계속 data에 push_back...
```

> [!question] 이 코드에서 `detach` 대신 `join`을 사용해야 하는 이유를 설명하고, `detach`가 야기하는 구체적인 위험을 서술하라.

> [!note]- 정답 확인 (클릭)
> `detach` 후 `process()`가 리턴하면 로컬 변수 `data`가 파괴된다. 하지만 detach된 스레드는 **이미 파괴된 `data`의 메모리에 계속 `push_back`을 수행** -- 이는 **undefined behavior**. 해당 스택 메모리는 이후 다른 함수(`f`)가 사용할 수 있고, 스레드가 그 메모리를 오염시키면 **디버깅이 극히 어려운 크래시**가 발생한다. 이것이 표준 위원회가 joinable thread의 destructor에서 implicit detach를 거부한 핵심 이유이다. `join`을 사용하면 스레드 완료를 기다리므로 `data`가 안전하게 사용된다. (📕 p.252-253)

### Q18. shared state와 future destructor의 블로킹

```cpp
{
    auto fut = std::async(std::launch::async, [] {
        std::this_thread::sleep_for(std::chrono::seconds(10));
    });
}  // fut의 destructor -- 여기서 10초간 block됨!
```

> [!question] 왜 block되는가? 이것이 `std::packaged_task`로 생성한 future와 다른 이유를 설명하라.

> [!note]- 정답 확인 (클릭)
> `std::async`(launch::async)로 생성된 shared state를 참조하는 **마지막 future**의 destructor는 task 완료까지 **implicit join**을 수행한다. 이는 detach의 위험(dangling reference)을 피하기 위한 표준 위원회의 타협안이다. `std::packaged_task`로 생성한 future는 이 규칙에 해당하지 않는다 -- shared state가 `std::async`가 아닌 `packaged_task`에서 생성되었기 때문. `packaged_task`의 경우 스레드 관리(join/detach)를 `std::thread`를 통해 직접 해야 한다. (📕 p.259-262)

### Q19. emplace_back + explicit constructor 함정

```cpp
std::vector<std::regex> regexes;

regexes.push_back(nullptr);     // 라인 A
regexes.emplace_back(nullptr);  // 라인 B
```

> [!question] 라인 A와 B 중 어느 것이 컴파일되고, 어느 것이 컴파일 에러인가? 그 이유는?

> [!note]- 정답 확인 (클릭)
> **라인 A: 컴파일 에러. 라인 B: 컴파일 성공 (하지만 undefined behavior!).**
> `std::regex`의 `const char*` 생성자는 **explicit**이다. `push_back`은 **copy initialization** (`std::regex r = nullptr`)을 사용하므로 explicit 생성자가 거부된다. `emplace_back`은 **direct initialization** (`std::regex r(nullptr)`)을 사용하므로 explicit 생성자가 허용된다. 하지만 null pointer는 유효한 정규식이 아니므로 **런타임에 undefined behavior**. Emplacement 함수 사용 시 explicit 생성자를 통한 의도치 않은 변환에 주의해야 한다. (📕 p.299-300)

### Q20. volatile의 착각

```cpp
volatile int sharedFlag = 0;

// Thread A:
prepareData();
sharedFlag = 1;  // "데이터 준비 완료" 신호

// Thread B:
while (sharedFlag == 0) {}  // 신호 대기
useData();
```

> [!question] Java/C# 경험자가 흔히 범하는 이 코드의 두 가지 결함을 설명하라.

> [!note]- 정답 확인 (클릭)
> **결함 1: Atomicity 부재.** `volatile`은 C++에서 atomicity를 보장하지 않는다. `sharedFlag`의 읽기/쓰기가 원자적이지 않을 수 있다 (구현에 따라). 두 스레드가 동시에 접근하면 data race = undefined behavior.
> **결함 2: 코드 재배치.** `volatile`은 컴파일러/하드웨어의 명령 재배치를 막지 않는다. `prepareData()`가 `sharedFlag = 1` 이후로 재배치될 수 있어, Thread B가 `useData()`를 호출할 때 데이터가 아직 준비되지 않았을 수 있다.
> **올바른 해결**: `std::atomic<int> sharedFlag`를 사용하면 atomicity와 sequential consistency(재배치 방지)를 모두 보장한다. Java/C#의 `volatile`은 C++의 `std::atomic`에 더 가깝다. (📕 p.271-275)

---

## 🎯 시나리오 문제 (4문제) -- ★★ Medium

### Q21. push_back vs emplace_back 선택

> [!question] 다음 상황에서 `push_back`과 `emplace_back` 중 어느 것을 사용해야 하는가?
> ```cpp
> std::vector<std::string> names;
> 
> // 상황 A: string literal 추가
> names.___("Alice");
> 
> // 상황 B: 기존 std::string 추가
> std::string name = "Bob";
> names.___(name);
> 
> // 상황 C: 중복 불허 set에 추가
> std::set<std::string> uniqueNames;
> uniqueNames.___("Charlie");
> ```

> [!note]- 정답 확인 (클릭)
> **상황 A: `emplace_back` 유리.** 인자 타입(`const char*`)이 컨테이너 타입(`std::string`)과 다르고, 끝에 construct하므로 임시 객체 생성을 피할 수 있다.
> **상황 B: 거의 동일.** 인자가 이미 `std::string`이므로 `push_back`이든 `emplace_back`이든 copy construction이 일어난다. 성능 차이 없음.
> **상황 C: `insert`가 나을 수 있음.** 중복 검사를 위해 emplace는 내부적으로 node를 먼저 생성한 뒤 비교하는데, 이미 존재하는 값이면 node 생성/소멸 비용이 낭비된다. 중복이 많을수록 insertion이 유리. (📕 p.295-296)

### Q22. pass by value vs reference 설계 선택

> [!question] 다음 두 함수 설계 중 어느 것이 적절한가?
> ```cpp
> // 설계 A: pass by value
> class Widget {
> public:
>     void setName(std::string name) {
>         m_name = std::move(name);
>     }
> };
> 
> // 설계 B: overloading
> class Widget {
> public:
>     void setName(const std::string& name) { m_name = name; }
>     void setName(std::string&& name) { m_name = std::move(name); }
> };
> ```
> 각 설계의 trade-off를 비용 관점에서 분석하라.

> [!note]- 정답 확인 (클릭)
> **설계 A (pass by value)**:
> - lvalue: copy 1 + move 1 (기존 `m_name` 메모리 재활용 불가, allocation + deallocation)
> - rvalue: move 2
> - 장점: 코드 1개, object code 1개, 유지보수 간편
>
> **설계 B (overloading)**:
> - lvalue: copy 1 (기존 `m_name.capacity()` >= `name.size()`면 **메모리 재할당 없음**)
> - rvalue: move 1
> - 장점: assignment 시 메모리 재활용 가능, 최소 비용
> - 단점: 함수 2개 선언/구현/유지보수
>
> **결론**: `setName`처럼 assignment로 복사하는 경우, 기존 메모리 재활용이 중요하므로 **설계 B가 더 효율적**. `addName`(push_back)처럼 construction으로 복사하는 경우, 설계 A도 합리적 (move 1회 추가 비용은 수용 가능). (📕 p.285-290)

### Q23. 스레드 풀 설계에서의 선택

> [!question] 서버 애플리케이션에서 수천 개의 요청을 동시에 처리해야 한다. `std::thread`를 직접 만드는 것과 `std::async`를 사용하는 것 중 어떤 접근이 적절한가?

> [!note]- 정답 확인 (클릭)
> **일반적으로 `std::async`가 적절하다.** 이유:
> 1. `std::thread`를 요청마다 생성하면 **thread exhaustion** (`std::system_error` 예외) 위험
> 2. 스레드 수가 hardware thread를 초과하면 **oversubscription** -- context switch 비용 폭증
> 3. `std::async`는 런타임 시스템에 스레드 관리를 위임하여 **load balancing, oversubscription 방지**를 자동 처리
>
> **단, 다음 경우 `std::thread` 직접 사용 고려**:
> - 스레드 priority/affinity 설정이 필요할 때 (`native_handle` 필요)
> - 고정 하드웨어에서 커스텀 thread pool이 더 효율적일 때
> - C++ Standard Library 구현이 thread pool을 제공하지 않을 때 (📕 p.243-245)

### Q24. one-shot event vs 반복 event

> [!question] 다음 두 시나리오에서 각각 어떤 event communication 방식을 선택해야 하는가?
> - **시나리오 A**: 초기화 스레드가 데이터 준비를 완료한 뒤, 워커 스레드 5개에게 "시작" 신호를 보낸다 (1회).
> - **시나리오 B**: 프로듀서가 데이터를 생산할 때마다 컨슈머에게 알린다 (반복).

> [!note]- 정답 확인 (클릭)
> **시나리오 A: `std::promise<void>` + `std::shared_future<void>`.**
> One-shot 이벤트에 완벽. `share()`로 `shared_future`를 만들어 5개 스레드가 각각 복사본을 `wait()`. Mutex 불필요, spurious wakeup 없음, 순서 무관.
> ```cpp
> std::promise<void> p;
> auto sf = p.get_future().share();
> for (int i = 0; i < 5; ++i)
>     threads.emplace_back([sf] { sf.wait(); doWork(); });
> p.set_value();  // 모든 스레드 깨움
> ```
>
> **시나리오 B: condition variable + mutex + flag.**
> `std::promise`는 one-shot이므로 반복 사용 불가. Condvar는 `notify_one()`/`notify_all()`로 반복 통지 가능. Spurious wakeup 대비 lambda predicate를 wait에 전달.
> ```cpp
> cv.wait(lk, [] { return dataReady; });
> ```
> (📕 p.262-270)

---

## 📝 빈칸 채우기 (5문제) -- ★★ Medium

### Q25.

> [!question] `std::thread`의 destructor에서 joinable thread가 파괴되면 `___`가 호출되어 프로그램이 종료된다. 이를 방지하려면 모든 경로에서 thread를 `___` 상태로 만들어야 한다.

> [!note]- 정답 확인 (클릭)
> **`std::terminate`**, **unjoinable** (📕 p.251)

### Q26.

> [!question] Future destructor의 특수 blocking 동작은 3가지 조건을 모두 만족할 때만 발생한다: (1) `___`로 생성된 shared state, (2) launch policy가 `___`, (3) 해당 shared state를 참조하는 `___` future.

> [!note]- 정답 확인 (클릭)
> **`std::async`**, **`std::launch::async`**, **마지막(last)** (📕 p.259-260)

### Q27.

> [!question] `std::atomic`은 `___`를 위한 도구이고, `volatile`은 `___`를 위한 도구이다. 전자는 `___` consistency를 보장하며, 후자는 컴파일러의 `___` load 제거와 `___` store 제거를 방지한다.

> [!note]- 정답 확인 (클릭)
> **concurrent programming (동시성 프로그래밍)**, **special memory (특수 메모리)**, **sequential**, **redundant**, **dead** (📕 p.279, 276)

### Q28.

> [!question] Pass by value가 적합한 파라미터의 4가지 조건: (1) `___` 타입, (2) `___`가 저렴, (3) 항상 `___`되는 경우, (4) `___` problem이 우려되지 않을 때.

> [!note]- 정답 확인 (클릭)
> **copyable**, **move**, **copied (복사)**, **slicing** (📕 p.285-291)

### Q29.

> [!question] Emplacement 함수는 `___` initialization을 사용하므로 `___` 생성자도 호출할 수 있지만, insertion 함수는 `___` initialization을 사용하므로 explicit 생성자를 거부한다.

> [!note]- 정답 확인 (클릭)
> **direct**, **explicit**, **copy** (📕 p.299-300)

---

## 🏆 최종 보스 문제 (2문제) -- ★★★ Hard

### Q30. 전체 Item 종합: 완벽한 스레드 안전 컨테이너

> [!question] 다음 코드를 개선하라. Item 35(task vs thread), Item 37(unjoinable thread), Item 38(future destructor), Item 40(atomic vs volatile), Item 42(emplacement)를 모두 적용하여 문제점을 식별하고 수정하라.

```cpp
class SharedLog {
    volatile int messageCount = 0;           // (1)
    std::vector<std::string> messages;
    std::mutex mtx;
    
public:
    void addMessage(const char* msg) {
        std::lock_guard<std::mutex> lock(mtx);
        messages.push_back(msg);             // (2)
        messageCount++;
    }
    
    void processAsync() {
        std::thread t([this] {               // (3)
            std::lock_guard<std::mutex> lock(mtx);
            for (auto& m : messages) process(m);
        });
        // t가 여기서 joinable 상태로 파괴될 수 있음  // (4)
    }
};
```

> [!note]- 정답 확인 (클릭)
> **(1) `volatile` --> `std::atomic<int>`**: `volatile`은 concurrency에 쓸모없다. 여러 스레드에서 `messageCount`를 읽을 수 있으므로 `std::atomic<int>`로 변경 (Item 40, 📕 p.271).
>
> **(2) `push_back(msg)` --> `emplace_back(msg)`**: `const char*`에서 `std::string`으로의 임시 객체 생성을 피할 수 있다 (Item 42, 📕 p.293-294).
>
> **(3) `std::thread` --> `std::async`**: 반환값이 필요없더라도 task-based가 thread management를 자동 처리한다. 또는 최소한 ThreadRAII를 사용해야 한다 (Item 35, 📕 p.241).
>
> **(4) Joinable thread 파괴 방지**: `processAsync`에서 `t`가 joinable 상태로 함수가 끝나면 `std::terminate`. ThreadRAII(DtorAction::join)으로 감싸거나, `std::async`를 사용하여 반환된 future를 멤버 변수로 보관 (Item 37, 📕 p.251).
>
> **개선된 코드:**
> ```cpp
> class SharedLog {
>     std::atomic<int> messageCount{0};
>     std::vector<std::string> messages;
>     std::mutex mtx;
>     std::future<void> pendingWork;  // future를 멤버로 보관
>     
> public:
>     void addMessage(const char* msg) {
>         std::lock_guard<std::mutex> lock(mtx);
>         messages.emplace_back(msg);
>         ++messageCount;
>     }
>     
>     void processAsync() {
>         pendingWork = std::async(std::launch::async, [this] {
>             std::lock_guard<std::mutex> lock(mtx);
>             for (auto& m : messages) process(m);
>         });
>     }
> };
> ```

### Q31. 전체 Book 종합: Perfect forwarding + Move semantics + Concurrency

> [!question] 아래 `ThreadSafeQueue` 클래스에서 (A)~(E)에 들어갈 적절한 코드를 작성하고, 각각 어느 Item에 근거하는지 설명하라.

```cpp
template<typename T>
class ThreadSafeQueue {
    std::queue<T> data;
    mutable std::mutex mtx;
    std::condition_variable cv;

public:
    // (A): T가 copyable이고 cheap to move면서 항상 복사되는 경우의 push 설계
    void push(___) {
        std::lock_guard<std::mutex> lock(mtx);
        data.push(___);
        cv.notify_one();
    }
    
    // (B): 임의 인자로 직접 T를 생성하는 emplace 설계
    template<typename... Args>
    void emplace(Args&&... args) {
        std::lock_guard<std::mutex> lock(mtx);
        data.emplace(___);
        cv.notify_one();
    }
    
    // (C): pop에서 condition variable 올바르게 사용
    T pop() {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, ___);
        ___ result = std::move(data.front());
        data.pop();
        return result;
    }
    
    // (D): 비동기로 N개 항목 처리 -- task-based, 안전한 future 관리
    ___ processAsync(int n, std::function<void(T)> func) {
        return std::async(___, [this, n, func] {
            for (int i = 0; i < n; ++i) {
                func(this->pop());
            }
        });
    }
};
```

> [!note]- 정답 확인 (클릭)
> **(A) Push -- pass by value (Item 41)**:
> ```cpp
> void push(T value) {
>     std::lock_guard<std::mutex> lock(mtx);
>     data.push(std::move(value));  // Item 25: 마지막 사용에서 move
>     cv.notify_one();
> }
> ```
> `T`가 copyable + cheap to move + 항상 복사되므로 pass by value 적합. lvalue는 copy+move, rvalue는 move+move. (📕 p.283)
>
> **(B) Emplace -- perfect forwarding (Item 42, Item 25)**:
> ```cpp
> data.emplace(std::forward<Args>(args)...);
> ```
> Variadic template + `std::forward`로 인자를 queue 내부에서 직접 construct. 임시 객체 없음. (📕 p.294)
>
> **(C) Pop -- condvar + lambda predicate (Item 39)**:
> ```cpp
> cv.wait(lock, [this] { return !data.empty(); });
> T result = std::move(data.front());  // auto도 가능 (Item 5)
> ```
> Lambda predicate로 spurious wakeup 방지. `std::move`로 front의 값을 효율적으로 추출. (📕 p.264-266)
>
> **(D) processAsync -- task-based + explicit async (Item 35, Item 36)**:
> ```cpp
> std::future<void> processAsync(int n, std::function<void(T)> func) {
>     return std::async(std::launch::async, [this, n, func] { ... });
> }
> ```
> `std::future<void>` 반환으로 caller가 future를 보관 가능 (Item 38 -- future destructor blocking 제어). `std::launch::async` 명시로 deferred 실행 방지 (Item 36). (📕 p.241, 246, 259)

---

## 📚 전체 복습 -- Effective Modern C++ Top 10 Takeaways

> [!danger] 이 책 전체에서 가장 중요한 10가지 교훈

### 1. Type Deduction을 정확히 이해하라 (Item 1-4)
Template, `auto`, `decltype`의 추론 규칙은 Modern C++의 기반이다. by-value는 const/volatile/reference를 벗기고, universal reference는 lvalue를 reference로 추론하며, `decltype((x))`는 `x`와 다른 타입을 반환할 수 있다.

### 2. auto를 선호하되, proxy class를 경계하라 (Item 5-6)
`auto`는 미초기화 방지, 플랫폼 호환성, 성능(closure 직접 저장)을 제공한다. 단, `std::vector<bool>::reference` 같은 invisible proxy class에서는 explicitly typed initializer idiom(`static_cast`)을 사용하라.

### 3. 중괄호 초기화의 장단점을 파악하라 (Item 7)
Narrowing conversion 방지, most vexing parse 회피 등의 장점이 있으나, `std::initializer_list` 오버로드를 강하게 선호하여 의도치 않은 생성자가 호출될 수 있다.

### 4. Smart pointer로 자원을 관리하라 (Item 18-22)
`std::unique_ptr`(exclusive), `std::shared_ptr`(shared), `std::weak_ptr`(관찰). `std::make_shared`/`std::make_unique`로 exception safety 확보. Raw `new`를 함수 인자에 직접 전달하지 마라.

### 5. Move semantics와 perfect forwarding을 올바르게 사용하라 (Item 23-30)
`std::move`는 rvalue로의 무조건 캐스트, `std::forward`는 조건부 캐스트. Universal reference(`T&&`)와 rvalue reference(`Widget&&`)를 구분하라. 마지막 사용에서만 move/forward를 적용하라.

### 6. Lambda를 std::bind보다 선호하라 (Item 31-34)
Lambda는 더 읽기 쉽고, 더 효율적이며, C++14에서는 `std::bind`의 모든 유스케이스를 대체할 수 있다. Default capture mode(`[=]`, `[&]`)의 dangling 위험을 주의하라.

### 7. Task-based 프로그래밍을 선호하라 (Item 35-36)
`std::async`가 `std::thread`보다 우월하다 -- 반환값 접근, 예외 전파, 자동 스레드 관리. Default launch policy의 deferred 함정에 주의하고, 필요시 `std::launch::async`를 명시하라.

### 8. 스레드 안전성을 RAII로 보장하라 (Item 37-39)
Joinable `std::thread`의 destructor는 프로그램을 종료한다. ThreadRAII 패턴으로 모든 경로에서 unjoinable을 보장하라. One-shot event에는 `std::promise<void>`/`std::future<void>`를 고려하라.

### 9. std::atomic과 volatile을 혼동하지 마라 (Item 40)
`std::atomic` = concurrency (atomicity + ordering). `volatile` = special memory (최적화 방지). Java/C#의 `volatile`은 C++의 `std::atomic`에 해당한다.

### 10. Emplacement과 pass by value를 적재적소에 활용하라 (Item 41-42)
Pass by value는 copyable + cheap to move + always copied일 때만. Assignment 복사에서는 메모리 재활용이 불가하므로 주의. Emplacement은 타입 불일치 + construction + 비중복 조건에서 유리하지만, exception safety와 explicit constructor 함정을 경계하라.
