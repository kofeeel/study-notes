---
tags: [컴퓨터구조, COD6, 면접대비, 면접질문]
type: interview-qa
---

# 컴퓨터 구조 면접 핵심 Q&A

> P&H Computer Organization and Design (COD) 6판 전체 범위 크로스커팅 면접 문제집.
> 기술 용어는 영어 병기. Junior → Mid → Senior → 시나리오 → 함정 질문 순으로 구성.

---

## 🟢 기본 Junior (15문제+)

### Q1. CPU가 명령어(instruction)를 실행하는 과정을 설명하세요.

CPU는 명령어를 **Fetch → Decode → Execute → Memory Access → Write-Back** 다섯 단계로 처리합니다. Fetch 단계에서 PC(Program Counter)가 가리키는 주소의 명령어를 명령어 메모리에서 읽고, Decode에서 제어 신호와 피연산자를 파악합니다. Execute에서 ALU가 연산을 수행하고, Memory Access에서 load/store 명령어는 데이터 메모리에 접근합니다. 마지막으로 Write-Back에서 결과를 레지스터 파일에 저장합니다. 이 구조는 단일 사이클과 파이프라인 CPU 모두의 근간이 됩니다.

> [!tip] Follow-up 질문
> - PC는 언제 업데이트되나요? 분기 명령어가 있을 때는?
> - 단일 사이클(single-cycle) CPU와 멀티사이클(multi-cycle) CPU의 차이는?

---

### Q2. 캐시(cache)가 왜 필요한가요?

CPU 연산 속도와 DRAM 접근 속도 사이에 **수백 배**의 속도 차이(Memory Wall)가 존재합니다. 캐시는 CPU와 메인 메모리 사이에 위치하는 소용량 고속 메모리로, **공간 지역성(spatial locality)**과 **시간 지역성(temporal locality)**을 활용해 자주 쓰이는 데이터를 가까이 둡니다. 캐시 히트(hit) 시 수 사이클, 미스(miss) 시 수백 사이클의 패널티 차이를 고려하면 실제 성능에 결정적인 영향을 줍니다.

> [!tip] Follow-up 질문
> - 시간 지역성과 공간 지역성의 구체적인 예를 드세요.
> - L1/L2/L3 캐시는 왜 계층적으로 구성하나요?

---

### Q3. RISC와 CISC의 차이점은 무엇인가요?

| 항목 | RISC | CISC |
|------|------|------|
| 명령어 수 | 적고 단순 | 많고 복잡 |
| 명령어 길이 | 고정(fixed-length) | 가변(variable-length) |
| 주요 예 | ARM, RISC-V, MIPS | x86, x86-64 |
| 메모리 접근 | Load/Store 전용 | 다양한 주소 모드 |
| 파이프라이닝 | 용이 | 복잡 |

RISC는 컴파일러가 최적화를 담당하고, CISC는 하드웨어가 복잡한 명령어를 처리합니다. 현대 x86 CPU는 내부적으로 RISC 마이크로-ops로 변환하여 실행하므로 경계가 흐려졌습니다.

> [!tip] Follow-up 질문
> - RISC-V가 최근 주목받는 이유는 무엇인가요?
> - ARM이 x86 대비 전력 효율이 좋은 이유는?

---

### Q4. 가상 메모리(virtual memory)란 무엇인가요?

가상 메모리는 각 프로세스에게 **독립적인 주소 공간**을 제공하는 추상화 메커니즘입니다. 프로그램이 사용하는 가상 주소(virtual address)를 OS와 하드웨어(MMU)가 협력해 물리 주소(physical address)로 변환합니다. 실제 물리 메모리보다 큰 주소 공간을 제공하고, 프로세스 간 메모리 격리(isolation)와 보호(protection)를 보장합니다. 변환 정보는 페이지 테이블(page table)에 저장됩니다.

> [!tip] Follow-up 질문
> - 페이지 폴트(page fault)는 언제 발생하며, OS는 어떻게 처리하나요?
> - 가상 메모리가 보안에 기여하는 방식은?

---

### Q5. 파이프라이닝(pipelining)이란 무엇이고 왜 사용하나요?

파이프라이닝은 명령어 실행 단계를 겹쳐서 수행하는 기법입니다. 세탁기-건조기-정리 비유처럼, 한 명령어가 Execute 단계에 있을 때 다음 명령어는 Decode, 그다음은 Fetch를 동시에 수행합니다. 이상적인 경우 CPI(Cycles Per Instruction)가 1에 근접하여 처리량(throughput)이 파이프라인 깊이만큼 향상됩니다. MIPS 5단계 파이프라인이 대표적 예입니다.

> [!tip] Follow-up 질문
> - 파이프라인 단계 수를 늘리면 항상 좋아지나요? (→ 함정 Q38 참조)
> - 파이프라인 해저드(hazard)의 종류는?

---

### Q6. 메모리 계층(memory hierarchy)의 각 레벨과 속도 차이를 설명하세요.

```
레지스터:   ~1 사이클      (~수십 바이트)
L1 캐시:    ~4 사이클      (~32-64 KB)
L2 캐시:    ~12 사이클     (~256 KB - 1 MB)
L3 캐시:    ~40 사이클     (~수 MB - 수십 MB)
DRAM:       ~200 사이클    (~수 GB)
NVMe SSD:  ~수만 사이클   (~수 TB)
HDD:       ~수백만 사이클  (~수 TB)
```

상위 계층일수록 빠르고 비싸며 용량이 작습니다. 이 계층이 효과적인 이유는 프로그램이 지역성(locality)을 보이기 때문이며, 실제로 자주 쓰이는 데이터는 대부분 상위 캐시에서 서비스됩니다.

> [!tip] Follow-up 질문
> - 메모리 계층을 설계할 때 트레이드오프는?
> - 캐시 라인(cache line) 크기가 64 bytes인 이유는?

---

### Q7. 2의 보수(two's complement)란 무엇이고 왜 사용하나요?

2의 보수는 음수를 표현하는 방식으로, 비트를 모두 반전시킨 후 1을 더해 구합니다. **덧셈/뺄셈 회로를 동일하게 재사용**할 수 있어 하드웨어가 단순해집니다. 예를 들어 4비트에서 -3은 0011 → 반전 1100 → +1 → 1101입니다. 1의 보수(ones' complement)는 0이 두 가지(+0, -0)였지만 2의 보수는 0이 하나뿐이라는 장점도 있습니다.

> [!tip] Follow-up 질문
> - 32비트 int의 최솟값은 왜 -2,147,483,648인가요?
> - 오버플로우(overflow)는 언제 발생하나요?

---

### Q8. 레지스터(register)와 메모리의 차이점은?

레지스터는 CPU 내부에 위치하는 가장 빠른 저장소로, 접근에 0~1 사이클이 소요됩니다. 메모리(DRAM)는 CPU 외부에 있어 수백 사이클이 필요합니다. RISC 아키텍처는 **Load/Store 원칙**에 따라 모든 연산을 레지스터 사이에서 수행하고, 데이터를 메모리에서 레지스터로 가져오거나(load) 레지스터에서 메모리로 저장(store)하는 명령어만 메모리에 접근합니다.

> [!tip] Follow-up 질문
> - RISC-V에는 레지스터가 몇 개인가요? x0 레지스터는 특별한가요?
> - 컴파일러가 레지스터 할당(register allocation)을 최적화하는 방식은?

---

### Q9. 직접 사상(direct-mapped) 캐시란 무엇인가요?

직접 사상 캐시는 각 메모리 블록이 캐시의 **정확히 하나의 위치**에만 매핑되는 구조입니다. 주소를 tag / index / offset으로 분할하여, index로 캐시 라인을 찾고 tag로 유효성을 확인합니다. 구현이 단순하고 빠르지만, 같은 index를 공유하는 주소들이 서로를 쫓아내는 **충돌 미스(conflict miss)**가 빈번히 발생할 수 있습니다.

> [!tip] Follow-up 질문
> - 2-way set associative 캐시와의 차이는?
> - Fully associative 캐시는 언제 쓰이나요?

---

### Q10. 인터럽트(interrupt)와 예외(exception)의 차이는?

**인터럽트(interrupt)**는 외부 I/O 디바이스 등 CPU 외부 요인으로 발생하는 비동기 이벤트입니다. **예외(exception)**는 오버플로우, 제로 나눗셈, 잘못된 주소 접근 등 CPU 내부의 명령어 실행 중 발생하는 동기 이벤트입니다. 둘 다 현재 실행을 중단하고 핸들러(handler)로 제어를 넘기지만, 발생 원인과 타이밍이 다릅니다. RISC-V에서는 모두 트랩(trap)으로 통합하여 처리합니다.

> [!tip] Follow-up 질문
> - 인터럽트 핸들러 진입 시 CPU가 저장하는 정보는?
> - 벡터 인터럽트(vectored interrupt)란?

---

### Q11. 부동소수점(floating-point) IEEE 754 표현 방식을 설명하세요.

IEEE 754 단정밀도(single precision)는 32비트를 부호(sign) 1비트, 지수(exponent) 8비트, 가수(mantissa/fraction) 23비트로 구성합니다. 값은 `(-1)^s × 1.fraction × 2^(exponent-127)` 로 계산합니다. 127을 빼는 것이 **바이어스(bias)** 표현입니다. 특수값으로 ±Infinity, NaN(Not a Number), 비정규수(denormalized) 등이 있습니다.

> [!tip] Follow-up 질문
> - 0.1 + 0.2 ≠ 0.3인 이유를 설명하세요.
> - 배정밀도(double precision)와 단정밀도의 차이는?

---

### Q12. ALUOP와 제어 신호(control signal)는 왜 필요한가요?

명령어마다 ALU가 수행해야 할 연산(add, sub, AND, OR 등)과 데이터 경로(datapath) 선택이 달라집니다. 제어 유닛(control unit)은 명령어의 opcode를 디코딩하여 ALUSrc, MemtoReg, RegWrite, MemRead, MemWrite, Branch, ALUOp 등의 제어 신호를 생성합니다. 이 신호들이 멀티플렉서(MUX)와 레지스터 파일, 메모리 접근을 적절히 제어합니다.

> [!tip] Follow-up 질문
> - 하드와이어드(hardwired) 제어와 마이크로프로그래밍(microprogramming)의 차이는?
> - RISC-V R-type과 I-type 명령어의 제어 신호 차이를 설명하세요.

---

### Q13. 스택(stack)과 힙(heap)은 메모리에서 어디에 위치하나요?

일반적으로 프로세스 주소 공간은 낮은 주소부터 텍스트(코드) → 데이터(전역변수) → 힙(동적 할당, 위로 증가) → ... → 스택(함수 호출, 아래로 증가) 순으로 배치됩니다. 스택은 함수 호출 시 스택 프레임(stack frame)을 push하고 반환 시 pop하는 LIFO 구조입니다. 힙은 `malloc`/`new` 등으로 동적 할당하며 명시적으로 해제해야 합니다.

> [!tip] Follow-up 질문
> - 스택 오버플로우(stack overflow)는 어떻게 발생하나요?
> - 함수 호출 시 스택에 저장되는 정보는? (return address, saved registers, local variables)

---

### Q14. 클럭 속도(clock speed)가 높으면 CPU가 빠른가요?

반드시 그렇지는 않습니다. CPU 성능은 **CPU Time = Instruction Count × CPI × Clock Cycle Time**으로 결정됩니다. 클럭 속도(= 1/Clock Cycle Time)가 높아도 CPI가 크거나 명령어 수가 많으면 느릴 수 있습니다. 또한 IPC(Instructions Per Cycle)는 아키텍처와 마이크로아키텍처에 따라 크게 다릅니다. GHz 수치만으로 다른 아키텍처 간 성능을 비교하는 것은 오류입니다.

> [!tip] Follow-up 질문
> - Amdahl의 법칙(Amdahl's Law)을 설명하세요.
> - SPEC CPU 벤치마크가 중요한 이유는?

---

### Q15. Little Endian과 Big Endian의 차이는?

**Little Endian**은 최하위 바이트(LSB)를 낮은 주소에 저장합니다 (x86, RISC-V 기본). **Big Endian**은 최상위 바이트(MSB)를 낮은 주소에 저장합니다 (네트워크 바이트 오더, SPARC). 예를 들어 0x12345678을 저장할 때 Little Endian은 `78 56 34 12`, Big Endian은 `12 34 56 78` 순서입니다. 네트워크 통신 시 서로 다른 엔디안 시스템 간 변환이 필요합니다.

> [!tip] Follow-up 질문
> - ARM은 어떤 엔디안을 사용하나요?
> - 엔디안 문제가 실제로 버그로 이어진 사례를 설명하세요.

---

## 🟡 중급 Mid-level (20문제+)

### Q16. 파이프라인 해저드(pipeline hazard)의 종류와 해결법을 설명하세요.

파이프라인 해저드는 세 가지입니다.

**1) 구조적 해저드(Structural Hazard)**: 두 명령어가 같은 하드웨어 자원을 동시에 필요로 할 때. 해결: 하버드 아키텍처처럼 명령어/데이터 메모리를 분리하거나 자원을 복제.

**2) 데이터 해저드(Data Hazard)**: 이전 명령어 결과를 다음 명령어가 필요로 할 때. 해결: **포워딩/바이패싱(forwarding/bypassing)**으로 결과를 직접 전달. Load-Use 해저드는 **스톨(stall)** 1사이클 불가피.

**3) 제어 해저드(Control Hazard)**: 분기 명령어 결과를 알기 전 이미 Fetch된 명령어들. 해결: **분기 예측(branch prediction)**, **지연 슬롯(delay slot)**, 플러시(flush).

> [!tip] Follow-up 질문
> - Load-Use 해저드에서 왜 포워딩만으로는 스톨을 피할 수 없나요?
> - 컴파일러가 해저드를 줄이는 방법(instruction scheduling)은?

---

### Q17. AMAT(Average Memory Access Time)란 무엇이고 어떻게 계산하나요?

**AMAT = Hit Time + Miss Rate × Miss Penalty**

캐시 계층이 여러 개일 때는 재귀적으로 적용합니다:
```
AMAT = L1 Hit Time + L1 Miss Rate × (L2 Hit Time + L2 Miss Rate × Memory Access Time)
```

예: L1 Hit Time = 1, L1 Miss Rate = 5%, L2 Hit Time = 10, L2 Miss Rate = 20%, Memory = 200 사이클이면:
`AMAT = 1 + 0.05 × (10 + 0.20 × 200) = 1 + 0.05 × 50 = 3.5 사이클`

AMAT를 줄이려면 히트 타임 감소(캐시 크기/구조 최적화), 미스 레이트 감소(더 큰 캐시, 높은 연관성), 미스 패널티 감소(더 나은 버스, 프리패칭)를 추구합니다.

> [!tip] Follow-up 질문
> - 3C 미스(Compulsory, Capacity, Conflict) 각각을 설명하세요.
> - 미스 패널티를 줄이는 하드웨어 기법은?

---

### Q18. TLB(Translation Lookaside Buffer)란 무엇인가요?

TLB는 가상 주소 → 물리 주소 변환을 캐싱하는 소규모 하드웨어 구조입니다. 페이지 테이블은 메모리에 있어 매번 접근하면 수백 사이클이 소요되므로, TLB에 최근 변환 결과를 저장해 대부분의 경우 1~수 사이클에 변환을 완료합니다. TLB **히트**면 즉시 물리 주소 반환, **미스**면 페이지 테이블 워크(page table walk)를 수행합니다. Context switch 시 TLB를 플러시하거나 ASID(Address Space ID)로 구별합니다.

> [!tip] Follow-up 질문
> - TLB 미스 처리를 하드웨어가 하는 경우(x86)와 소프트웨어(RISC-V)가 하는 경우의 차이는?
> - Huge Page(2MB, 1GB)를 쓰면 TLB에 어떤 이점이 있나요?

---

### Q19. Write-back과 Write-through의 차이를 설명하세요.

**Write-through**: 캐시와 메모리 모두에 즉시 씁니다. 구현이 단순하고 일관성 유지가 쉽지만, 모든 쓰기마다 느린 메모리에 접근하여 성능이 떨어집니다. Write buffer로 완화 가능.

**Write-back**: 캐시에만 쓰고, 해당 캐시 라인이 교체될 때 메모리에 씁니다. dirty bit로 수정 여부를 추적합니다. 쓰기 성능이 훨씬 좋지만, 구현이 복잡하고 캐시 일관성(coherence) 관리가 어렵습니다.

현대 CPU는 L1/L2 캐시에 Write-back을 주로 사용합니다.

> [!tip] Follow-up 질문
> - Write-back 캐시에서 캐시 라인을 교체할 때 반드시 메모리에 써야 하나요?
> - Write-allocate vs. No-write-allocate 정책이란?

---

### Q20. 분기 예측(branch prediction)은 어떻게 동작하나요?

분기 예측기는 조건 분기(conditional branch)의 결과를 미리 추측하여 파이프라인 버블을 줄입니다.

- **정적 예측(static)**: 항상 taken 또는 not taken으로 예측. 단순하지만 정확도 낮음.
- **1-bit 예측기**: 마지막 결과를 기억. 루프 종료 시 반드시 한 번 틀림.
- **2-bit 포화 카운터(saturating counter)**: 0~3 값으로 강하게 taken/not taken을 예측. 한 번 틀려도 바로 예측을 바꾸지 않아 노이즈에 강함.
- **상관 예측기(correlating predictor)**: 최근 분기 이력(global history)으로 패턴을 학습.

현대 CPU는 TAGE 등 고급 예측기로 99%+ 정확도를 달성합니다.

> [!tip] Follow-up 질문
> - 예측이 틀렸을 때 CPU는 어떻게 복구하나요?
> - 간접 분기(indirect branch)의 예측은 왜 더 어렵나요?

---

### Q21. 캐시 일관성(cache coherence)이란 무엇인가요?

멀티코어 환경에서 각 코어가 별도의 L1 캐시를 가질 때, 한 코어가 데이터를 수정하면 다른 코어의 캐시에 있는 복사본과 불일치가 발생합니다. **캐시 일관성**은 모든 코어가 메모리의 특정 위치에 대해 최신 값을 보도록 보장하는 속성입니다.

**MESI 프로토콜**이 대표적으로, 캐시 라인 상태를 Modified / Exclusive / Shared / Invalid로 관리합니다. 한 코어가 캐시 라인을 수정하면 다른 코어의 복사본을 Invalid로 만들어 일관성을 유지합니다.

> [!tip] Follow-up 질문
> - MESI에서 Shared 상태의 라인에 쓰기가 발생하면 어떤 전환이 일어나나요?
> - Directory-based coherence vs. Snooping-based coherence의 차이는?

---

### Q22. False sharing이란 무엇이고 어떻게 피하나요?

**False sharing**은 서로 다른 변수가 같은 캐시 라인(cache line, 보통 64 bytes)에 위치할 때, 한 코어가 자신의 변수를 수정하면 그 캐시 라인 전체가 무효화되어 다른 코어도 영향을 받는 현상입니다. 실제로 데이터를 공유하지 않음에도 불구하고 캐시 일관성 트래픽이 발생하여 성능이 저하됩니다.

해결책: 변수를 캐시 라인 크기에 맞게 **패딩(padding)**하거나, 코어별 데이터를 별도 캐시 라인에 배치합니다. C/C++에서는 `alignas(64)` 또는 `__attribute__((aligned(64)))`를 사용합니다.

> [!tip] Follow-up 질문
> - False sharing이 발생하는 코드를 예시로 작성하고 수정하세요.
> - Perf 또는 VTune으로 false sharing을 어떻게 탐지하나요?

---

### Q23. 비순차 실행(Out-of-Order Execution, OoO)이란 무엇인가요?

비순차 실행은 데이터 의존성이 없는 명령어들을 프로그램 순서와 다르게 실행하여 파이프라인 스톨을 최소화하는 기법입니다. **Tomasulo 알고리즘**이 대표적으로, 예약 스테이션(reservation station)과 레지스터 이름 바꾸기(register renaming, ROB)를 활용합니다. 실행은 비순서이지만 **커밋(commit/retire)은 순서대로** 이루어져 프로그래밍 모델의 정확성을 보장합니다. ROB(Reorder Buffer)가 이 역할을 담당합니다.

> [!tip] Follow-up 질문
> - WAR(Write After Read), WAW(Write After Write) 해저드를 레지스터 리네이밍으로 어떻게 해결하나요?
> - OoO 실행이 Spectre 취약점과 어떤 관련이 있나요?

---

### Q24. 캐시 친화적(cache-friendly) 코드란 무엇인가요?

캐시 친화적 코드는 **공간 지역성**을 극대화하는 코드입니다. 가장 대표적인 예는 2차원 배열 순회 순서입니다. C/C++에서 행 우선(row-major) 저장이므로 `a[i][j]`를 `j`를 내부 루프로 순회하면 연속 메모리를 접근하여 캐시 미스를 줄입니다. 반대로 `i`를 내부 루프로 하면 stride가 크고 캐시 미스가 많아 수십 배 느려질 수 있습니다. 행렬 곱셈의 경우 **캐시 블로킹(tiling)**으로 작업 집합(working set)을 캐시 크기에 맞춥니다.

> [!tip] Follow-up 질문
> - AoS(Array of Structures) vs. SoA(Structure of Arrays)의 성능 차이는?
> - 캐시 블로킹(loop tiling)의 구체적인 구현 방법은?

---

### Q25. 슈퍼스칼라(superscalar)와 VLIW의 차이는?

**슈퍼스칼라(superscalar)**: 하드웨어가 런타임에 의존성을 동적으로 분석하여 여러 명령어를 동시 실행합니다. 프로그래머/컴파일러가 병렬성을 명시하지 않아도 됩니다. 현대 고성능 CPU(Intel Core, AMD Ryzen)가 사용합니다.

**VLIW(Very Long Instruction Word)**: 컴파일러가 정적으로 병렬 실행 가능한 명령어를 묶어 하나의 긴 명령어로 만듭니다. 하드웨어가 단순하지만, 컴파일러 품질에 크게 의존하고 바이너리 호환성 문제가 있습니다. Intel Itanium(IA-64)이 실패한 이유 중 하나입니다.

> [!tip] Follow-up 질문
> - 슈퍼스칼라에서 issue width(발행 폭)가 4라는 것의 의미는?
> - 동적 스케줄링과 정적 스케줄링의 트레이드오프는?

---

### Q26. 메모리 접근 패턴과 프리패칭(prefetching)의 관계는?

CPU는 캐시 미스가 예상되면 데이터를 미리 캐시로 가져오는 **하드웨어 프리패처(hardware prefetcher)**를 사용합니다. 스트라이드 패턴(sequential, stride-1) 접근은 프리패처가 쉽게 탐지하여 효과적으로 프리패칭합니다. 그러나 랜덤 접근(포인터 체이싱 등)은 패턴을 예측하기 어려워 프리패칭 효과가 낮습니다. 소프트웨어 프리패칭(`__builtin_prefetch`)으로 컴파일러가 힌트를 제공하기도 합니다.

> [!tip] Follow-up 질문
> - 링크드 리스트가 배열보다 캐시 성능이 나쁜 이유는?
> - 스트리밍 데이터(non-temporal store)를 처리할 때 캐시를 우회하는 이유는?

---

### Q27. 가상 주소를 물리 주소로 변환하는 전체 과정을 설명하세요.

1. CPU가 가상 주소(VA) 생성
2. **TLB** 검색: 히트면 물리 주소(PA) 즉시 반환
3. TLB 미스: **페이지 테이블 워크(page table walk)** 시작
4. CR3 레지스터(x86) 또는 SATP(RISC-V)로 페이지 테이블 기저 주소 참조
5. 다단계 페이지 테이블(예: x86-64는 4단계, PGD→PUD→PMD→PT) 순서대로 메모리 접근
6. 마지막 페이지 테이블 엔트리(PTE)에서 PFN(Physical Frame Number) 획득
7. VA의 page offset과 결합하여 최종 PA 생성
8. TLB에 새 매핑 캐시

> [!tip] Follow-up 질문
> - 4단계 페이지 테이블에서 TLB 미스 1회에 메모리 접근이 몇 번 발생하나요?
> - 페이지 테이블 자체를 캐시에 넣는 방법은?

---

### Q28. 캐시 교체 정책(replacement policy)을 비교하세요.

- **LRU(Least Recently Used)**: 가장 오래 사용되지 않은 라인을 교체. 직관적으로 최적에 가깝지만 구현 비용이 큼.
- **Random**: 무작위 선택. 구현 단순, 최악 케이스가 없어 실용적.
- **FIFO**: 가장 먼저 들어온 라인 교체. Belady's anomaly가 있음.
- **Pseudo-LRU**: LRU 근사. 비트 트리로 구현하여 하드웨어 비용 절감.

실제 캐시는 대부분 **Pseudo-LRU** 또는 **RRIP(Re-reference Interval Prediction)** 계열을 사용합니다.

> [!tip] Follow-up 질문
> - Belady's optimal algorithm이란? 실용적으로 사용 가능한가요?
> - 스캔(scan) 접근 패턴에서 LRU가 특히 나쁜 이유는?

---

### Q29. 메모리 대역폭(memory bandwidth)과 레이턴시(latency)의 차이는?

**레이턴시(latency)**: 하나의 메모리 요청을 시작해서 완료까지 걸리는 시간(예: 100 ns). 레이턴시가 높으면 단일 요청이 느립니다.

**대역폭(bandwidth)**: 단위 시간당 전송 가능한 데이터량(예: 50 GB/s). 대역폭이 좋으면 병렬 대량 전송이 빠릅니다.

캐시 미스가 많고 요청이 직렬화된 워크로드(포인터 체이싱)는 레이턴시에 bound되고, 스트리밍 연산(BLAS, memcpy)은 대역폭에 bound됩니다. `roofline model`은 이 두 제약 중 어느 것이 병목인지 시각화합니다.

> [!tip] Follow-up 질문
> - DRAM 내부 구조(row/column, RAS/CAS)가 레이턴시에 어떤 영향을 미치나요?
> - DDR4 vs. DDR5의 대역폭/레이턴시 차이는?

---

### Q30. 분기 예측 실패(branch misprediction)의 성능 영향은?

분기 예측이 틀리면 이미 파이프라인에 들어온 잘못된 명령어들을 플러시(flush)하고 올바른 경로를 다시 Fetch해야 합니다. 이 때 발생하는 성능 손실은 **파이프라인 깊이 × 사이클**입니다. 현대 깊은 파이프라인(15~20 단계)에서는 예측 실패 한 번에 15~20 사이클이 낭비됩니다. 잘못된 분기 예측이 많은 코드(예측 불가능한 분기)는 이를 **분기 없는 코드(branchless code)**로 변환하여 성능을 높일 수 있습니다.

> [!tip] Follow-up 질문
> - 조건 이동(cmov, conditional move) 명령어가 분기를 대체하는 방식은?
> - 프로파일 기반 최적화(PGO, Profile-Guided Optimization)가 분기 예측을 어떻게 돕나요?

---

### Q31. 가상화(virtualization)와 하이퍼바이저(hypervisor)에서 메모리 관리는?

가상화 환경에서는 게스트 OS의 가상 주소 → 게스트 물리 주소 → 호스트 물리 주소의 두 단계 변환이 필요합니다. **소프트웨어 Shadow Page Table**은 두 변환을 합쳐 게스트 VA → 호스트 PA를 직접 매핑하지만 오버헤드가 큽니다. 하드웨어 지원 **Intel EPT(Extended Page Tables)** / **AMD NPT(Nested Page Tables)**는 하드웨어가 두 단계 변환을 자동으로 수행하여 성능을 개선합니다.

> [!tip] Follow-up 질문
> - Type-1 하이퍼바이저와 Type-2 하이퍼바이저의 차이는?
> - IOMMU가 가상화에서 왜 중요한가요?

---

### Q32. 세그멘테이션(segmentation)과 페이징(paging)의 차이는?

**세그멘테이션**: 코드, 데이터, 스택 등 논리적 단위(세그먼트)로 메모리를 분할. 가변 크기라 외부 단편화(external fragmentation)가 발생합니다.

**페이징**: 고정 크기(보통 4KB) 페이지로 분할. 내부 단편화(internal fragmentation)만 발생하며 외부 단편화가 없습니다.

현대 OS는 페이징을 주로 사용하며, x86은 세그멘테이션을 완전히 우회하는 평탄 모델(flat model)을 씁니다.

> [!tip] Follow-up 질문
> - 내부 단편화와 외부 단편화를 각각 설명하고, 어느 것이 더 심각한 문제인가요?
> - x86-64에서 세그먼트 레지스터는 어떤 용도로 남아있나요?

---

### Q33. 멀티스레드 프로그램에서 데이터 레이스(data race)를 하드웨어 레벨에서 설명하세요.

데이터 레이스는 두 스레드가 동기화 없이 같은 메모리 위치에 접근하고 그 중 하나가 쓰기일 때 발생합니다. 하드웨어 레벨에서는 각 코어가 독립적인 캐시를 가지므로, 쓰기가 캐시 일관성 프로토콜을 통해 전파되는 타이밍에 따라 결과가 비결정적이 됩니다. **메모리 모델(memory model)**이 어떤 순서 보장을 제공하는지가 중요하며, C++ `std::atomic`이나 memory fence 명령어로 순서를 명시합니다.

> [!tip] Follow-up 질문
> - TSO(Total Store Order)가 보장하는 것과 보장하지 않는 것은?
> - `mfence`와 `sfence`, `lfence` 명령어의 차이는?

---

### Q34. Instruction-Level Parallelism (ILP)를 높이는 기법들은?

ILP는 동시에 실행할 수 있는 명령어의 수입니다.

- **파이프라이닝(Pipelining)**: 다른 명령어의 다른 단계를 겹쳐 실행
- **슈퍼스칼라(Superscalar)**: 여러 실행 유닛에 여러 명령어를 동시 발행
- **비순차 실행(OoO)**: 의존성 없는 명령어를 재정렬하여 실행
- **추측 실행(Speculative Execution)**: 분기 결과를 예측해 미리 실행
- **레지스터 리네이밍(Register Renaming)**: WAR/WAW 해저드 제거

현실적 ILP 한계(ILP Wall)는 코드의 데이터 의존성과 분기 예측 실패로 이론값보다 훨씬 낮습니다.

> [!tip] Follow-up 질문
> - ILP Wall이란 무엇이며 이를 어떻게 극복했나요?
> - Thread-Level Parallelism(TLP)과 ILP의 차이는?

---

### Q35. 캐시 세트 연관성(set associativity)을 높이면 어떤 영향이 있나요?

연관성을 높이면 **충돌 미스(conflict miss)**가 줄어 전체 미스 레이트가 감소합니다. 그러나 같은 세트 내 여러 웨이(way)를 비교해야 하므로 **히트 타임이 증가**합니다. 또한 태그 비교 회로와 교체 정책 로직이 복잡해져 전력 소모와 면적이 늘어납니다. Fully associative는 미스 레이트가 최소이지만 히트 타임이 가장 크고 하드웨어 비용이 높습니다. 일반적으로 **4-way 또는 8-way**에서 좋은 트레이드오프를 얻습니다.

> [!tip] Follow-up 질문
> - 연관성 증가의 수익 체감(diminishing returns)은 어느 시점부터 나타나나요?
> - L1 캐시가 L3 캐시보다 연관성이 낮은 이유는?

---

## 🔴 고급 Senior (15문제+)

### Q36. VIPT(Virtually Indexed, Physically Tagged)와 PIPT의 차이는?

| 캐시 유형 | 인덱싱 | 태깅 | 특징 |
|----------|--------|------|------|
| VIVT | 가상 | 가상 | 빠르지만 aliasing, homonym 문제 |
| VIPT | 가상 | 물리 | TLB와 병렬 인덱싱 가능, 조건부로 aliasing 없음 |
| PIPT | 물리 | 물리 | 항상 안전하지만 TLB 직렬화 |

**VIPT**는 TLB 변환과 캐시 인덱싱을 동시에 수행할 수 있어 빠릅니다. page offset 비트 내에서 인덱싱이 완료되면(인덱스 비트 ≤ page offset 비트) aliasing이 없어 안전합니다. 현대 L1 캐시는 대부분 VIPT를 사용합니다. L2 이상은 큰 용량으로 인해 PIPT를 사용합니다.

> [!tip] Follow-up 질문
> - VIPT에서 aliasing(동일 물리 주소가 여러 캐시 라인에 매핑)이 발생하는 조건은?
> - Homonym 문제를 ASID로 해결하는 방법은?

---

### Q37. 메모리 일관성 모델(memory consistency model)이란 무엇인가요?

캐시 일관성(coherence)이 단일 메모리 위치의 관측 순서를 보장한다면, **메모리 일관성 모델(consistency model)**은 **여러 메모리 위치**에 대한 접근 순서를 다른 프로세서가 어떻게 관측하는지를 규정합니다.

- **Sequential Consistency(SC)**: 가장 강함. 모든 프로세서의 접근이 단일 전역 순서처럼 보임. 성능 저하 큼.
- **TSO(Total Store Order)**: x86이 보장. 쓰기가 순서대로 보이지만, 스토어가 로드보다 늦게 보일 수 있음.
- **Relaxed(RISC-V, ARM)**: 더 많은 재정렬 허용. 명시적 fence 명령어로 순서 강제.

> [!tip] Follow-up 질문
> - TSO에서 store buffer가 어떤 재정렬을 허용하나요?
> - Java/C++의 `volatile`이 메모리 순서에 대해 보장하는 것은 정확히 무엇인가요?

---

### Q38. Spectre와 Meltdown은 어떤 하드웨어 취약점을 이용하나요?

**Meltdown(CVE-2017-5754)**: 비순차 실행(OoO) 중 권한 없는 커널 메모리를 추측적으로 읽고, 그 값을 캐시 타이밍 사이드 채널(timing side channel)로 유출합니다. 명령이 커밋 전 예외로 취소되어도 캐시 상태가 남습니다. **KPTI(Kernel Page-Table Isolation)**로 완화.

**Spectre(CVE-2017-5753, 5715)**: 분기 예측기를 조작하여 피해자 프로세스/커널의 추측 실행 경로가 시크릿에 의존한 캐시 접근을 하도록 유도합니다. **Retpoline**, **IBRS**, **IBPB** 등 소프트웨어+마이크로코드 완화.

두 취약점 모두 **추측 실행 + 캐시 타이밍 채널**의 조합을 악용합니다.

> [!tip] Follow-up 질문
> - Flush+Reload, Prime+Probe, Evict+Time 캐시 타이밍 공격의 차이는?
> - KPTI가 성능에 미치는 영향은 어떤 워크로드에서 가장 큰가요?

---

### Q39. Non-blocking cache(lockup-free cache)란 무엇인가요?

**Blocking cache**: 캐시 미스 발생 시 미스가 해결될 때까지 이후 모든 메모리 요청을 블로킹합니다.

**Non-blocking cache(lockup-free)**: 미스 처리 중에도 다른 캐시 히트 요청을 계속 서비스합니다. **MSHR(Miss Status Holding Register)**이 진행 중인 미스를 추적합니다. OoO 프로세서와 함께 사용하면 미스 레이턴시를 숨길 수 있어 **Memory-Level Parallelism(MLP)**을 활용합니다.

MSHR 수가 동시에 처리할 수 있는 미스의 수를 결정합니다. 현대 CPU는 10~20개 이상의 MSHR을 가집니다.

> [!tip] Follow-up 질문
> - MLP(Memory-Level Parallelism)란 무엇이고, 이를 높이려면?
> - OoO 프로세서가 없으면 non-blocking cache의 이점이 감소하는 이유는?

---

### Q40. Hardware Prefetching의 종류와 한계를 설명하세요.

**스트림 프리패처(stream prefetcher)**: 연속적인 미스를 감지해 다음 캐시 라인을 미리 가져옵니다. stride-1 접근에 최적.

**스트라이드 프리패처(stride prefetcher)**: 일정한 stride 패턴(예: 8바이트씩)을 감지해 프리패칭. 구조체 배열 접근에 유효.

**SMS/SPP(Spatial/Signature Path Prefetcher)**: 더 복잡한 공간 패턴 학습.

**한계**: 불규칙한 포인터 체이싱, 간접 참조, 작은 작업 집합(모두 캐시에 들어가는 경우) 등에서는 효과 없음. 공격적 프리패칭은 불필요한 캐시 오염(cache pollution)을 유발할 수 있습니다.

> [!tip] Follow-up 질문
> - 소프트웨어 프리패칭(`_mm_prefetch`)을 언제 사용해야 하나요?
> - 프리패칭이 오히려 성능을 해치는 경우는?

---

### Q41. Memory Wall이란 무엇이고 현대 아키텍처는 어떻게 대응하나요?

**Memory Wall**은 CPU 연산 속도가 메모리 접근 속도보다 훨씬 빠르게 증가하면서 메모리 레이턴시가 성능의 주요 병목이 되는 현상입니다(Wulf & McKee, 1995). CPU GHz는 수십 배 증가했지만 DRAM 레이턴시는 수 배만 개선되었습니다.

**대응 기법**:
- 더 큰 다단계 캐시
- Non-blocking cache + MLP
- 하드웨어/소프트웨어 프리패칭
- 메모리 레이턴시 숨기기(latency hiding): OoO 실행, HBM(High Bandwidth Memory)
- Compute-Near-Memory(PIM, Processing In Memory)
- 3D-stacked DRAM(HBM2E, LPDDR5)

> [!tip] Follow-up 질문
> - HBM(High Bandwidth Memory)이 GDDR보다 AI 가속기에 적합한 이유는?
> - Near-Memory Computing의 아키텍처적 도전 과제는?

---

### Q42. GPU의 SIMT(Single Instruction Multiple Threads) 모델을 설명하세요.

SIMT는 GPU의 실행 모델로, 하나의 명령어가 워프(warp, NVIDIA) 또는 웨이브프론트(wavefront, AMD)라 불리는 32/64개의 스레드에 동시에 적용됩니다. 모든 스레드는 같은 명령어를 실행하지만 각자 다른 데이터를 처리합니다(SIMD와 유사). **발산(divergence)**: 조건 분기에서 스레드마다 다른 경로를 가면 마스킹으로 직렬화되어 효율이 떨어집니다. 따라서 GPU 코드는 워프 내 분기를 최소화해야 합니다.

> [!tip] Follow-up 질문
> - GPU 메모리 계층(레지스터 → 공유 메모리 → L1/L2 → GDDR)을 설명하세요.
> - 코얼레스드 메모리 접근(coalesced memory access)이 왜 중요한가요?

---

### Q43. TAGE(TAgged GEometric history length) 분기 예측기란?

TAGE는 현재 최고 수준의 분기 예측 알고리즘 중 하나입니다. **기하급수적으로 늘어나는 히스토리 길이**를 사용하는 여러 태그된 컴포넌트(predictor tables)로 구성됩니다(예: 2, 4, 8, 16, 32, 64, 128 비트). 가장 긴 히스토리와 매칭되는 컴포넌트의 예측을 사용하고, 더 짧은 히스토리 컴포넌트는 대안(alternate prediction)으로 사용합니다. 예측 정확도 99.x% 이상을 달성하며 Intel, AMD의 현대 CPU에 변형이 탑재되어 있습니다.

> [!tip] Follow-up 질문
> - 예측기가 훈련(training)에 취약한 경우는? (Spectre와 연관)
> - O-GEHL, ITTAGE(간접 분기용 TAGE)의 원리는?

---

### Q44. 멀티코어에서 Cache Coherence Directory Protocol이란?

스누핑(snooping) 기반 프로토콜은 모든 코어가 공유 버스를 모니터링하므로 코어 수가 늘면 버스 트래픽이 폭증합니다. **Directory Protocol**은 각 캐시 라인의 상태와 소유 코어를 추적하는 디렉토리를 둡니다. 한 코어가 데이터를 수정할 때 디렉토리를 통해 해당 라인을 가진 다른 코어들에게만 무효화(invalidation) 메시지를 보냅니다. 확장성이 뛰어나 수십~수백 코어 서버(NUMA 시스템)에서 사용합니다.

> [!tip] Follow-up 질문
> - NUMA(Non-Uniform Memory Access) 아키텍처에서 원격 메모리 접근의 레이턴시 영향은?
> - Interconnect topology(메시, 링, 트리 등)가 일관성 트래픽에 미치는 영향은?

---

### Q45. 마이크로아키텍처 최적화 기법인 μOp 캐시(Decoded Instruction Cache)를 설명하세요.

x86 명령어는 가변 길이로 디코딩 비용이 큽니다. **μOp 캐시(Intel: Decoded ICache, DSB)**는 디코딩된 마이크로-ops를 캐싱하여, 반복 실행되는 루프의 경우 재디코딩 없이 μOp을 바로 공급합니다. Intel Nehalem 이후 표준으로, 정렬된 루프가 DSB에 완전히 들어가면 프론트엔드 병목 없이 peak IPC에 가까워집니다. Intel VTune의 `DSB_coverage` 지표로 활용도를 확인합니다.

> [!tip] Follow-up 질문
> - 루프 언롤링(loop unrolling)이 DSB에서 오히려 해가 되는 경우는?
> - Loop Stream Detector(LSD)란?

---

### Q46. SMT(Simultaneous Multi-Threading, HyperThreading)의 원리와 한계는?

SMT는 하나의 물리 코어가 여러 하드웨어 스레드의 상태(레지스터 파일, PC)를 동시에 유지하면서, 한 스레드가 캐시 미스나 대기 중일 때 다른 스레드의 명령어를 실행 유닛에 채워 넣는 기법입니다. 면적 오버헤드 약 5~10%로 처리량을 20~30% 향상시킬 수 있습니다.

**한계**: L1/L2 캐시를 공유하므로 캐시 용량이 반감 효과. 보안 측면에서 Spectre류 사이드 채널 공격의 공격 면 확대. 레이턴시 크리티컬 태스크에서는 오히려 성능 저하.

> [!tip] Follow-up 질문
> - HyperThreading을 비활성화하면 언제 성능이 향상되나요?
> - SMT와 코어 증가 중 어느 것이 처리량 향상에 더 효과적인가요?

---

### Q47. DRAM 내부 구조와 Refresh의 영향을 설명하세요.

DRAM 셀은 커패시터에 전하를 저장하므로 시간이 지나면 방전됩니다. 이를 막기 위해 주기적으로 **리프레시(refresh)**를 수행해야 합니다. DDR4는 64ms마다 모든 행(row)을 리프레시합니다. 리프레시 중에는 해당 뱅크에 접근할 수 없어 **리프레시 오버헤드**가 발생합니다(약 2~3%). **RowHammer** 공격은 인접 행을 반복 활성화(activate)하여 다른 행의 비트를 뒤집는 DRAM 취약점입니다. 완화책으로 **TRR(Target Row Refresh)**, ECC DRAM 등이 사용됩니다.

> [!tip] Follow-up 질문
> - DRAM tRCD, tCAS, tRP 타이밍이 레이턴시에 어떻게 기여하나요?
> - RowHammer 공격을 소프트웨어적으로 어떻게 수행하나요?

---

### Q48. RISC-V의 설계 철학과 확장 가능한 ISA(Instruction Set Architecture) 구조를 설명하세요.

RISC-V는 BSD 라이선스의 오픈소스 ISA로, **모듈식(modular)** 설계가 핵심입니다. 기본 정수 명령어 집합(RV32I/RV64I)에 필요한 확장(extension)을 조합합니다: M(정수 곱나눗셈), A(원자 연산), F(단정밀도 부동소수점), D(배정밀도), C(압축 명령어), V(벡터) 등. 이 모듈성으로 임베디드부터 서버까지 단일 ISA로 커버합니다. 특히 **RV64GC** (G = IMAFDZicsr\_Zifencei)가 Linux용 표준 조합입니다.

> [!tip] Follow-up 질문
> - RISC-V의 x86 대비 바이너리 호환성 문제는 어떻게 해결하나요?
> - RISC-V에서 시스템 콜과 CSR(Control and Status Register) 접근 방식은?

---

### Q49. Persistent Memory(NVM, PMem)가 메모리 계층에 미치는 영향은?

Intel Optane PMem(3D XPoint)과 같은 바이트 주소 지정 가능한 비휘발성 메모리는 DRAM보다 용량이 크고(TB급) 비휘발성이지만, DRAM보다 레이턴시가 3~5배 높습니다. **메모리 계층에서 DRAM과 SSD 사이**를 채웁니다. 프로그래밍 모델이 복잡합니다: 크래시 일관성(crash consistency)을 보장하기 위해 **CLFLUSHOPT**, **SFENCE**로 명시적 플러시가 필요합니다. PMDK(Persistent Memory Development Kit) 같은 라이브러리가 이를 추상화합니다.

> [!tip] Follow-up 질문
> - PMem의 App Direct 모드와 Memory 모드의 차이는?
> - 크래시 일관성을 위한 로깅(logging)과 Copy-on-Write 기법의 차이는?

---

### Q50. Branch Target Buffer(BTB)와 Return Address Stack(RAS)을 설명하세요.

**BTB(Branch Target Buffer)**: 분기 명령어의 주소 → 분기 목적지 주소를 캐싱합니다. 직접 분기(direct branch)의 목적지를 빠르게 예측하여 Fetch 단계에서 바로 다음 PC를 결정합니다.

**RAS(Return Address Stack)**: 함수 호출 시 반환 주소(return address)를 하드웨어 스택에 push하고, `ret` 명령어 예측 시 pop합니다. 간접 분기인 `ret`의 목적지를 거의 완벽하게 예측합니다. 재귀 깊이가 RAS 크기를 초과하면 예측 실패가 증가합니다.

> [!tip] Follow-up 질문
> - BTB 미스와 분기 예측 미스(direction miss)는 어떻게 다른가요?
> - Spectre Variant 2(BTB poisoning)는 어떻게 BTB를 악용하나요?

---

## 🎭 시나리오 기반 (10문제+)

### Q51. 코드 리뷰 중 캐시 미스가 많은 코드를 발견했습니다. 어떻게 최적화하나요?

> [!example] 시나리오
> 행렬 행렬 곱셈 코드가 예상보다 10배 느립니다.

1. **프로파일링 먼저**: `perf stat`, `cachegrind`, VTune으로 캐시 미스 위치 확인
2. **접근 패턴 분석**: 행 우선 순서 확인. 전치(transpose) 행렬을 미리 만들어 `B[k][j]`를 `B_T[j][k]`로 변경
3. **캐시 블로킹(tiling)**: 블록 크기를 L1/L2 캐시 크기에 맞게 조정. 32×32 또는 64×64 블록 시도
4. **SIMD 벡터화**: AVX2/AVX-512로 여러 원소를 동시에 연산
5. **루프 언롤링**: 루프 오버헤드 감소, 레지스터 재사용
6. **라이브러리 활용**: OpenBLAS, MKL 같은 최적화된 BLAS 라이브러리 고려

> [!tip] Follow-up 질문
> - 타일 크기를 어떻게 결정하나요?
> - `perf stat`에서 확인해야 할 핵심 카운터는?

---

### Q52. 컨텍스트 스위칭(context switching)의 하드웨어 비용을 설명하세요.

> [!example] 시나리오
> 스레드를 많이 만들면 무조건 빠를까요?

컨텍스트 스위칭 시 발생하는 하드웨어 비용:
1. **레지스터 저장/복원**: 범용 레지스터, FPU/SIMD 레지스터(수백 바이트)
2. **TLB 플러시(또는 ASID 변경)**: 새 프로세스로 전환 시 TLB 전체 무효화 → 이후 TLB 미스 급증
3. **캐시 오염(cache pollution)**: 새 스레드의 작업 집합이 캐시를 채우면 이전 스레드 데이터 교체
4. **BTB/분기 예측기 오염**: 스레드마다 다른 분기 패턴으로 예측기 재훈련 비용

**결론**: 스레드가 과도하게 많으면 실제 계산보다 컨텍스트 스위칭 오버헤드가 지배적이 됩니다.

> [!tip] Follow-up 질문
> - ASID(Address Space ID)가 TLB 플러시를 어떻게 완화하나요?
> - 코루틴(coroutine)이 스레드보다 컨텍스트 스위칭 비용이 낮은 이유는?

---

### Q53. 게임에서 프레임 드랍이 발생합니다. 메모리 관점에서 원인을 분석하세요.

> [!example] 시나리오
> 오픈 월드 게임에서 특정 구역 진입 시 프레임이 60→20으로 급락합니다.

**가능한 메모리 원인들**:

1. **텍스처/메시 스트리밍**: 새 구역 진입 시 대량의 데이터를 저장소에서 VRAM으로 업로드. 스트리밍 시스템이 따라오지 못하면 버블 발생.

2. **GC(가비지 컬렉션) 스파이크**: C#(Unity) 또는 Lua 환경에서 대량의 힙 할당/해제가 누적되어 GC 트리거.

3. **캐시 미스 증가**: 새 구역의 오브젝트들이 캐시에 없어 첫 접근 시 Compulsory Miss 급증.

4. **VRAM ↔ RAM 스와핑**: VRAM이 부족하면 GPU가 시스템 RAM에서 데이터를 가져오는 PCIe 전송이 발생. 레이턴시 수백 μs.

5. **TLB 미스 증가**: 새 메모리 매핑이 많아질 때.

> [!tip] Follow-up 질문
> - GPU 프레임 분석 도구(RenderDoc, NSight)에서 확인해야 할 메모리 지표는?
> - 텍스처 스트리밍을 최적화하는 방법은?

---

### Q54. 서버에서 동일한 알고리즘이 싱글코어보다 멀티코어에서 느립니다. 왜 그럴까요?

> [!example] 시나리오
> 4코어로 병렬화했더니 싱글코어 대비 1.2배만 빨라졌습니다.

**원인 분석**:

1. **Amdahl의 법칙**: 순차 실행 부분이 5%만 있어도 이론 최대 속도향상은 ~16배. 실제 병렬 코드 비율 점검.

2. **False sharing**: 각 스레드가 같은 캐시 라인의 다른 변수를 수정 → 캐시 일관성 트래픽 폭주.

3. **Lock contention**: 뮤텍스 경합으로 대부분의 시간을 대기에 소비.

4. **NUMA 효과**: 원격 소켓 메모리 접근으로 레이턴시 2~3배 증가.

5. **캐시 스래싱(thrashing)**: 4개 스레드의 합산 작업 집합이 L3 캐시를 초과하여 DRAM 접근 급증.

> [!tip] Follow-up 질문
> - `perf lock` 또는 `lockstat`으로 락 경합을 어떻게 분석하나요?
> - NUMA-aware 메모리 할당(`numactl`, `libnuma`)이란?

---

### Q55. 실시간 시스템에서 캐시 미스 시간이 비결정적입니다. 어떻게 해결하나요?

> [!example] 시나리오
> 자동차 제어 ECU에서 최악 실행 시간(WCET)이 예측 불가합니다.

캐시의 비결정성은 실시간 시스템의 WCET 분석을 어렵게 만듭니다.

**해결 방법**:
1. **캐시 잠금(cache locking)**: 중요 코드/데이터를 캐시에 고정하여 교체 불가. 일부 임베디드 CPU 지원.
2. **스크래치패드 메모리(Scratchpad Memory, SPM)**: 프로그래머가 직접 관리하는 소규모 빠른 메모리. 캐시처럼 자동이 아닌 명시적 관리로 결정성 확보.
3. **캐시 파티셔닝**: 태스크별 캐시 way 할당 (Intel CAT: Cache Allocation Technology).
4. **소프트웨어 캐시 분석**: WCET 도구(AbsInt aiT)로 정적 캐시 분석.

> [!tip] Follow-up 질문
> - Intel CAT(Cache Allocation Technology)는 어떻게 동작하나요?
> - 하드리얼타임(hard real-time)과 소프트리얼타임(soft real-time)의 차이는?

---

### Q56. 데이터베이스 시스템에서 인덱스 스캔보다 순차 스캔이 빠른 경우가 있습니다. 왜인가요?

인덱스(B-Tree 등)는 포인터 체이싱으로 랜덤 I/O를 유발합니다. 테이블 전체를 스캔하는 경우 데이터가 디스크/메모리에서 연속적으로 읽혀 **프리패칭과 스트리밍에 유리**합니다. 대량의 행을 반환해야 하는 쿼리라면 인덱스 오버헤드(인덱스 블록 접근 + 랜덤 힙 접근)가 순차 스캔보다 더 많은 I/O를 유발합니다. 쿼리 옵티마이저는 선택도(selectivity)가 낮을 때 인덱스를 건너뛰고 순차 스캔을 선택합니다.

> [!tip] Follow-up 질문
> - 컬럼 스토어(columnar store) DB가 OLAP에 유리한 캐시 이유는?
> - 버퍼 풀(buffer pool)이 OS 페이지 캐시와 경쟁하는 문제는?

---

### Q57. WebServer 레이턴시를 줄이기 위해 캐시 관점에서 무엇을 확인하나요?

> [!example] 시나리오
> HTTP API 응답 시간이 p99에서 급격히 증가합니다.

**하드웨어 캐시 관점**:
1. **LLC(Last Level Cache) 미스 레이트**: `perf stat -e LLC-load-misses`로 확인. 높으면 작업 집합이 캐시를 초과.
2. **NUMA 원격 접근**: NUMA 토폴로지 고려한 스레드/메모리 배치.
3. **iTLB 미스**: 대형 바이너리에서 코드 캐시 미스. 프로파일로 핫 코드 경로 확인, PGO로 핫 코드를 메모리 연속 배치.
4. **False sharing**: 공유 카운터, 리퀘스트 큐 구조체 패딩 점검.

**소프트웨어 캐시**: CDN, Redis 등은 별도 문제.

> [!tip] Follow-up 질문
> - 코드 크기가 커질수록 L1 instruction cache 미스가 증가하는 이유는?
> - Transparent Huge Pages(THP)가 TLB 미스를 줄이는 원리는?

---

### Q58. AI 추론 서버에서 GPU 메모리 병목을 진단하세요.

> [!example] 시나리오
> GPU 사용률은 30%인데 처리량이 예상보다 낮습니다.

**원인 분석**:
1. **Memory-Bound 연산**: 행렬 연산이 작아 Arithmetic Intensity(연산/바이트)가 낮으면 VRAM 대역폭 포화.
2. **커널 런치 오버헤드**: 작은 배치 크기로 커널 수가 많으면 CPU↔GPU 동기화 오버헤드.
3. **VRAM 부족으로 CPU 오프로드**: 모델이 VRAM을 초과하여 PCIe 전송 발생.
4. **Roofline 분석**: cuBLAS/CUTLASS GEMM의 arithmetic intensity 확인.

**해결**: 배치 크기 증가, 양자화(INT8/INT4), Flash Attention(메모리 효율적 attention), kv-cache 최적화.

> [!tip] Follow-up 질문
> - Flash Attention이 기존 Attention보다 메모리 효율이 좋은 이유는?
> - Tensor Parallelism vs. Pipeline Parallelism의 메모리 트레이드오프는?

---

### Q59. 임베디드 시스템에서 메모리 제약 환경 최적화 방법은?

> [!example] 시나리오
> 256KB Flash, 32KB RAM의 MCU에서 동작해야 합니다.

1. **코드 크기 최적화**: `-Os` 컴파일, LTO(Link Time Optimization), 미사용 함수 제거(`--gc-sections`)
2. **데이터 배치 최적화**: const 데이터는 Flash에, 런타임 데이터만 RAM에
3. **스택 크기 최소화**: 재귀 대신 반복, 로컬 대형 배열 회피
4. **압축 데이터 구조**: 비트필드, 구조체 패킹(`__attribute__((packed))`)
5. **XIP(Execute in Place)**: Flash에서 직접 코드 실행, RAM 복사 불필요

> [!tip] Follow-up 질문
> - MCU에서 캐시가 없는 경우 Flash 접근 레이턴시는 어떻게 되나요?
> - Harvard 아키텍처 MCU에서 명령어 버스와 데이터 버스가 분리된 장점은?

---

### Q60. 데이터센터에서 메모리 용량을 줄이면서 성능을 유지하려면?

> [!example] 시나리오
> 서버 DRAM 비용을 50% 절감하라는 요구가 있습니다.

1. **메모리 압축(memory compression)**: zswap, zRAM으로 메모리 내 압축. CPU 오버헤드 vs. 메모리 절감 트레이드오프.
2. **Huge Pages 활용**: TLB 압력 감소로 같은 메모리로 더 많은 처리량.
3. **워크로드 분석**: 실제 Active Working Set 측정(`/proc/meminfo`의 Active vs. Inactive). 과도하게 할당된 경우 많음.
4. **NUMA 토폴로지 최적화**: 메모리 적지만 로컬리티 높이면 성능 유지.
5. **메모리 모아두기(memory pooling)**: 프로세스 간 공유 메모리(shared memory)로 중복 제거.

> [!tip] Follow-up 질문
> - KSM(Kernel Samepage Merging)이 가상화 환경에서 메모리를 절약하는 원리는?
> - Memory overcommit이 위험한 이유는?

---

## ⚠️ 함정 질문 (10문제+)

> [!warning] 함정 질문이란?
> 직관적으로 "당연히 YES"라고 대답하고 싶지만 실제로는 트레이드오프가 있는 질문들입니다.

### Q61. 캐시가 크면 클수록 항상 좋은가요?

**아니오.** 캐시가 커질수록:
- **히트 타임 증가**: 용량이 커지면 검색해야 할 태그가 많아지고 SRAM 전파 지연이 늘어납니다. L1이 커지면 클럭 속도를 낮춰야 할 수 있습니다.
- **전력 소모 증가**: 캐시 태그 접근, 데이터 배열 전력이 선형 이상으로 증가.
- **실리콘 면적**: 캐시가 차지하는 면적은 추가 실행 유닛에 쓸 수 있습니다.
- **AMAT 관점**: `AMAT = Hit Time + Miss Rate × Miss Penalty`. 히트 타임이 충분히 늘면 큰 캐시도 손해.

최적 캐시 크기는 워크로드의 working set 크기에 따라 결정됩니다.

> [!tip] Follow-up 질문
> - L1 캐시가 작은 이유와 L3 캐시가 큰 이유를 설명하세요.
> - 워크로드별 최적 캐시 크기를 어떻게 결정하나요?

---

### Q62. 파이프라인이 깊으면(깊은 단계 수) 항상 빠른가요?

**아니오.** 파이프라인이 깊어질수록:
- **분기 예측 실패 패널티 증가**: 플러시해야 할 단계가 많아 낭비 사이클 증가. Pentium 4(NetBurst, ~31 단계)는 이 때문에 실패.
- **단계 간 레지스터 오버헤드**: 각 단계 사이 파이프라인 레지스터(flip-flop)로 인한 지연.
- **해저드 처리 복잡도 증가**: 데이터 포워딩 경로가 복잡해짐.
- **클럭 속도 증가 한계**: 전력 밀도(power density)와 발열 문제.

최적 파이프라인 깊이는 기술 노드와 워크로드에 따라 다릅니다(현대 고성능 CPU는 보통 14~19 단계).

> [!tip] Follow-up 질문
> - Intel NetBurst 아키텍처가 실패한 이유를 설명하세요.
> - 파이프라인 깊이와 주파수, 발열의 관계는?

---

### Q63. 가상 메모리는 물리 메모리를 늘려주나요?

**아니오. (부분적으로만 사실)** 가상 메모리가 제공하는 것:
- 프로세스에게 큰 **주소 공간 추상화** (64비트 → 128TB 이상)
- 실제 물리 메모리보다 큰 합산 가상 메모리를 여러 프로세스에 제공 가능 (오버커밋)
- 디스크 스와핑으로 물리 메모리를 보완하지만 성능 비용이 극심

**가상 메모리가 하지 않는 것**: 물리 DRAM 용량 증가. 모든 페이지에 실제로 접근하면 스와핑이 발생하고 성능이 폭락합니다.

> [!tip] Follow-up 질문
> - Demand paging이란 무엇이고 어떻게 물리 메모리를 절약하나요?
> - OOM Killer는 언제 어떤 기준으로 프로세스를 종료하나요?

---

### Q64. 멀티코어가 많을수록 프로그램이 빠른가요?

**항상 그렇지는 않습니다.** **Amdahl의 법칙**: 순차 실행 비율 `s`가 있으면 이론 최대 속도향상 = `1 / (s + (1-s)/N)`. 순차 부분 10%만 있어도 코어가 아무리 많아도 최대 10배 향상에 불과합니다.

추가로:
- 코어 간 동기화 오버헤드
- 캐시 일관성 트래픽 증가
- 메모리 대역폭 포화(메모리가 병목이면 코어 추가 의미 없음)
- False sharing

**Gustafson의 법칙**: 문제 크기를 코어 수에 비례해 키우면 더 많은 병렬 이득.

> [!tip] Follow-up 질문
> - Amdahl의 법칙과 Gustafson의 법칙이 다른 관점을 취하는 이유는?
> - 락프리(lock-free) 알고리즘이 항상 뮤텍스보다 빠른가요?

---

### Q65. CPU 주파수를 두 배로 높이면 프로그램이 두 배 빠른가요?

**아니오.** CPU Time = IC × CPI × (1/f). 주파수를 2배 올리면 이론적으로 2배 빠르지만:
- **메모리 바운드 프로그램**: CPU가 메모리 응답을 기다리는 시간이 대부분. 메모리 레이턴시는 클럭과 독립적(절대 ns 단위)이므로 사이클 수로 보면 2배 증가. 실제로 거의 빨라지지 않음.
- **발열 제한**: 주파수 증가는 전력 소모 증가(P ∝ f³)로 현실적으로 2배 달성 어려움.
- **IPC 변화**: 주파수를 높이기 위해 파이프라인을 깊게 하면 IPC가 낮아질 수 있음.

> [!tip] Follow-up 질문
> - 동적 주파수 조절(DVFS)이 성능과 전력에 미치는 영향은?
> - Turbo Boost가 지속적으로 유지되지 않는 이유는?

---

### Q66. Write-through 캐시는 항상 데이터 손실을 방지하나요?

**부분적으로만 사실.** Write-through는 메모리에 즉시 쓰므로 캐시 손실 시 데이터가 메모리에 있습니다. 그러나:
- **Write buffer**: Write-through도 write buffer를 사용하면, 버퍼에 있는 데이터는 전력 손실 시 유실.
- **DRAM 자체**: DRAM은 휘발성이므로 전원 꺼지면 write-through로 써도 사라짐.
- **진정한 내구성**: NVMe SSD + 파워로스 보호 캐패시터(power loss protection) 필요.

> [!tip] Follow-up 질문
> - 데이터베이스에서 `fsync()`가 왜 중요한가요?
> - Write-ahead logging(WAL)이 내구성을 어떻게 보장하나요?

---

### Q67. 더 빠른 CPU는 항상 더 많은 에너지를 소비하나요?

**아니오.** **에너지 효율**을 구분해야 합니다.

- **전력(Power, W)**: 순간 소비 전력. 빠른 CPU는 높은 주파수와 전압으로 전력이 큼.
- **에너지(Energy, J)**: 전력 × 시간. 2배 빠른 CPU가 동일 작업을 절반 시간에 완료하면 에너지는 비슷하거나 적을 수 있음.
- **Race-to-halt**: 빠르게 처리하고 저전력 상태로 진입하는 것이 에너지 효율적일 수 있음.
- **현대 모바일 SoC**: 빅리틀(big.LITTLE), DynamIQ로 작업 성격에 따라 코어를 선택하여 에너지 최적화.

> [!tip] Follow-up 질문
> - 정적 전력(static power)과 동적 전력(dynamic power)의 차이는?
> - 저전압-고주파 vs. 고전압-저주파 중 어느 것이 에너지 효율적인가요?

---

### Q68. TLB 미스가 발생하면 항상 페이지 폴트가 발생하나요?

**아니오.** TLB 미스와 페이지 폴트는 다릅니다:
- **TLB 미스**: 변환 정보가 TLB에 없음. 페이지 테이블에는 있을 수 있음. 페이지 테이블 워크로 해결. 페이지 폴트 없음.
- **페이지 폴트**: 페이지 테이블에 유효 매핑이 없거나(Present bit = 0), 접근 권한 위반. OS 개입 필요.

**정리**: TLB 미스 → 페이지 테이블 검색 → 매핑 있으면 TLB 채움(페이지 폴트 없음) / 매핑 없으면 페이지 폴트.

> [!tip] Follow-up 질문
> - Minor page fault와 major page fault의 차이는?
> - Copy-on-Write(CoW)는 어떤 종류의 페이지 폴트를 사용하나요?

---

### Q69. 캐시 미스가 줄면 항상 프로그램이 빨라지나요?

**반드시 그렇지는 않습니다.** 캐시 미스를 줄이려다 다른 오버헤드가 늘 수 있습니다:
- **소프트웨어 프리패칭 과다**: prefetch 명령어 자체가 명령어 캐시와 실행 유닛을 점유.
- **캐시 블로킹 과다 적용**: 블록 크기가 너무 작으면 루프 오버헤드 증가, 너무 크면 효과 없음.
- **데이터 구조 패딩 과도**: 패딩으로 캐시 라인이 채워지면 오히려 메모리 사용량과 대역폭 증가.
- **병렬 실행 기회 감소**: 순차적으로 최적화하다 병렬성을 파괴.

항상 **프로파일링 → 가설 → 측정** 순서로 접근해야 합니다.

> [!tip] Follow-up 질문
> - 조기 최적화(premature optimization)가 위험한 이유를 하드웨어 관점에서 설명하세요.
> - A/B 성능 비교 시 통계적으로 유의미한 측정을 하려면?

---

### Q70. 원자 연산(atomic operation)은 락(lock)보다 항상 빠른가요?

**아니오.** 원자 연산도 비용이 있습니다:
- **Cache line lock**: x86의 LOCK prefix는 메모리 버스/캐시 라인을 잠가 다른 코어의 접근을 차단. 높은 경합(contention) 시 lock-based 방식보다 느릴 수도 있음.
- **메모리 배리어 포함**: CAS(Compare-and-Swap) 등은 암묵적 메모리 펜스를 포함.
- **경합이 높을 때**: 모든 코어가 같은 atomic 변수에 접근하면 캐시 line bouncing으로 성능 폭락.
- **올바른 경우**: 경합이 낮고 임계 구역이 짧을 때 원자 연산이 뮤텍스보다 훨씬 빠름.

> [!tip] Follow-up 질문
> - CAS(Compare-and-Swap) 루프가 ABA 문제에 취약한 이유는?
> - `std::memory_order_relaxed`와 `std::memory_order_seq_cst`의 성능 차이는?

---

## 빠른 참조 요약

> [!summary] 핵심 공식 모음
> - **CPU Time** = IC × CPI × Clock Cycle Time
> - **AMAT** = Hit Time + Miss Rate × Miss Penalty
> - **Amdahl의 법칙** = 1 / (s + (1-s)/N)
> - **Power** ∝ C × V² × f (동적 전력)
> - **IPC** = 1 / CPI (Cycles Per Instruction의 역수)

> [!summary] 면접 키워드 체크리스트
> - [ ] 5단계 파이프라인 (IF/ID/EX/MEM/WB)
> - [ ] 3C 미스 (Compulsory, Capacity, Conflict)
> - [ ] MESI 프로토콜
> - [ ] TLB → Page Table Walk → Page Fault 흐름
> - [ ] Spectre: 추측실행 + 캐시 타이밍
> - [ ] TAGE 분기 예측기
> - [ ] VIPT 조건 (index bits ≤ page offset bits)
> - [ ] Memory Consistency vs. Cache Coherence
> - [ ] Amdahl vs. Gustafson
> - [ ] OoO: ROB + Reservation Station + Register Renaming

> [!quote] 면접 마인드셋
> 모든 하드웨어 설계에는 트레이드오프가 있습니다.
> "항상 좋다", "항상 나쁘다"는 없습니다.
> **"조건에 따라 다르다"**가 대부분의 올바른 답입니다.
