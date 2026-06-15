# C++ + 알고리즘 출제 예상

## A. C++ 심화

### 1. 가상함수 + vtable
- 가상함수 호출 = **vtable lookup** (런타임 비용)
- **vptr**: 객체당 1개 (객체 메모리 시작 부분)
- 클래스당 1개 vtable (정적 배열)
- 다중상속/가상상속 시 vtable 복잡도 증가
- **가상 소멸자 필수**: 부모 포인터로 자식 delete 시 자식 소멸자 호출 보장
- **순수 가상함수** `= 0` → 추상 클래스 (인스턴스화 불가)

```cpp
class Base {
public:
    virtual ~Base() = default;  // 가상 소멸자 (필수)
    virtual void Func() = 0;     // 순수 가상함수
};
```

### 2. RAII + 스마트 포인터
- **RAII**: 자원 획득 = 객체 생성, 자원 해제 = 소멸자
  - 예외 안전성 보장 (스택 풀린 변수의 소멸자 자동 호출)

- **unique_ptr**: 단독 소유
  - 복사 X, 이동 O
  - 오버헤드 0 (raw pointer와 같음)

- **shared_ptr**: 공유 소유
  - **참조 카운트** (원자적 증감 → 멀티스레드 안전)
  - **Control Block** = ref count + weak count + deleter
  - 메모리 레이아웃: 객체 + Control Block (make_shared면 한 덩어리)

- **weak_ptr**: 순환 참조 해소
  - lock() → shared_ptr 반환 (만료 시 nullptr)

```cpp
// 순환 참조 예시
struct A { shared_ptr<B> b; };
struct B { shared_ptr<A> a; };  // 둘 다 leak → b를 weak_ptr로!
```

### 3. 이동 시맨틱 (C++11+)
- **lvalue**: 이름 있는 값 (`int x = 5; x`)
- **rvalue**: 임시 값 (`5`, `x+y`, `func()`)
- **std::move()**: rvalue 캐스팅 (실제 이동은 안 함, 단지 cast)
- **이동 생성자**: `T(T&& other)` → 자원 빼앗기
- **RVO/NRVO**: 컴파일러가 임시 객체 복사 제거

### 4. STL 내부 구조

| 컨테이너 | 내부 | 접근 | 삽입/삭제 |
|---------|------|------|----------|
| vector | 동적 배열 | O(1) | 끝 amortized O(1) / 중간 O(n) |
| list | 이중 연결 리스트 | O(n) | O(1) (위치 알면) |
| deque | 청크 블록 배열 | O(1) | 양 끝 O(1) |
| map | RB-Tree | O(log n) | O(log n) |
| unordered_map | 해시 테이블 | 평균 O(1), 최악 O(n) | 평균 O(1) |
| set / multiset | RB-Tree | O(log n) | O(log n) |

- vector 증가: capacity 부족 시 보통 **2배** 재할당 → amortized O(1)
- unordered_map 최악 O(n): 해시 충돌 다수 시
- map은 키 정렬 유지, unordered_map은 비정렬

### 5. 기타 자주 묻는 것
- **const correctness**: const T&, T*, const T*, T* const
- **explicit**: 암시적 형변환 방지
- **noexcept**: 예외 안 던짐 보장 → 이동 시 최적화 가능
- **constexpr**: 컴파일 타임 평가
- **inline**: ODR 위반 방지 + 최적화 힌트

---

## B. 알고리즘

### 1. 복잡도
- **Big-O**: 최악 시간/공간
- **상한 vs 하한**: O / Ω / Θ
- **분기 제거 최적화** (크래프톤 기출)
  ```cpp
  // 분기 (예측 실패 시 비용)
  if (v > 0) sum += v;
  // 산술 (분기 없음)
  sum += (v > 0) * v;
  ```

### 2. 핵심 알고리즘
- **정렬**
  - 퀵: 평균 O(n log n), 최악 O(n²), in-place
  - 머지: O(n log n) 보장, 메모리 O(n), **안정**
  - 힙: O(n log n) 보장, in-place, 불안정
  - 안정 정렬: 머지, 버블, 삽입
- **탐색**
  - 이진 탐색 O(log n) — 정렬 배열만
  - BFS (큐) / DFS (스택 또는 재귀)
- **DP**
  - 메모이제이션 (Top-down): 재귀 + 캐시
  - 타뷸레이션 (Bottom-up): 반복문 + 배열
- **그래프**
  - 다익스트라 (양수 가중치): O(E log V) with 힙
  - 플로이드-워셜 (모든 쌍): O(V³)
  - A* (휴리스틱): 게임 길찾기 표준

### 3. 자료구조
- **Heap (우선순위 큐)**: 삽입/삭제 O(log n), top O(1)
- **Trie**: 문자열 검색 O(L)
- **Segment Tree**: 구간 합/최솟값 O(log n)
- **Union-Find**: 거의 O(1) (경로 압축 + 랭크)
- **Hash Table 충돌 해결**: 체이닝 / 오픈 어드레싱 (Linear/Quadratic Probing)

### 4. 게임 특화
- **FSM (Finite State Machine)**: 상태 + 전이
- **Behavior Tree**: 노드(Sequence/Selector/Decorator) 합성
- **공간 분할**: Quad/Octree, BVH, KD-Tree (충돌 광역 검사)

### 5. 행렬 곱셈 (구현 패턴 + 시각화)
> 그래픽스 변환행렬(이동·회전·스케일)에서 계속 쓰임. 핵심 = **3중 루프 + 누적합**.

**공식:** `C[i][j] = Σ_k A[i][k] × B[k][j]`

**① 큰 그림 — C[i][j]는 "A의 행"과 "B의 열"이 만나는 교차점** (예: 2×3 · 3×2)
```
                    ┌─ B (3×2) ─┐
                    │   7    8  │
       arr2[k][0] → │   9   10  │
       (세로 ↓)     │  11   12  │
                    └───────────┘
┌─ A (2×3) ───┐     ┌─ C (2×2) ─┐
│  1   2   3  │ →→  │ [58]  64  │   ← C[0][0]
│  4   5   6  │     │ 139  154  │
└─────────────┘     └───────────┘
   arr1[0][k] →
   (가로 →)
```
C[0][0]에서 **왼쪽**으로 밀면 A의 0행 `[1 2 3]`, **위**로 밀면 B의 0열 `[7 9 11]`. 둘을 곱해 더한 값.

**② k가 하는 일 — 행(가로)·열(세로)을 동시에 슬라이드하며 짝짓기**
```
          k=0    k=1    k=2
 A행 i=0:  1      2      3      ← arr1[0][k]  (가로 →)
           ×      ×      ×
 B열 j=0:  7      9     11      ← arr2[k][0]  (세로 ↓)
          ───    ───    ───
           7  +  18  +  33   =  58   → answer[0][0]
```
A는 **열 인덱스**가, B는 **행 인덱스**가 움직임. 둘 다 같은 `k`라서 짝이 맞음.

**③ 누적 추적** — `answer[i][j] += arr1[i][k] * arr2[k][j]`

| k | arr1[0][k] | arr2[k][0] | 곱 | answer[0][0] |
|---|-----------|-----------|-----|--------------|
| 0 | 1 | 7 | 7 | 0+7 = **7** |
| 1 | 2 | 9 | 18 | 7+18 = **25** |
| 2 | 3 | 11 | 33 | 25+33 = **58** |

**인덱스 역할:** `i`=A행/C행(셀 동안 고정) · `j`=B열/C열(고정) · `k`=공통차원(곱을 누적, 진짜 계산)

```cpp
vector<vector<int>> solution(vector<vector<int>> arr1, vector<vector<int>> arr2) {
    int n = arr1.size();       // 결과 행
    int m = arr2[0].size();    // 결과 열
    int K = arr2.size();       // 공통 차원 (arr1 열 = arr2 행)
    vector<vector<int>> answer(n, vector<int>(m, 0));  // ★ 크기 잡고 0 초기화
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            for (int k = 0; k < K; k++)
                answer[i][j] += arr1[i][k] * arr2[k][j];
    return answer;
}
```

### 6. BFS 그리드 최단거리 (템플릿)
> 길찾기 단골. 핵심 = **방문 체크 + 거리 기록**을 `dist` 하나로 동시에.
> *왜 최단?* 시작점에서 가까운 칸부터 물결처럼 퍼짐 → 도착칸 **첫 도달 = 최단**.

```cpp
int dx[4]{0, 1, 0, -1}, dy[4]{1, 0, -1, 0};   // x=행(첫 인덱스), y=열

int solution(vector<vector<int>> maps) {
    int n = maps.size(), m = maps[0].size();
    vector<vector<int>> dist(n, vector<int>(m, 0));   // 0 = 미방문 겸 거리

    queue<pair<int,int>> q;
    q.push({0, 0});
    dist[0][0] = 1;                                   // 시작칸 거리 1

    while (!q.empty()) {
        int x = q.front().first, y = q.front().second;
        q.pop();                                      // ★ 빠지면 front 고정 → 무한루프
        if (x == n-1 && y == m-1) return dist[x][y];  // 도착 즉시 반환

        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i], ny = y + dy[i];
            if (nx >= 0 && nx < n && ny >= 0 && ny < m   // 범위
                && maps[nx][ny] == 1                     // 벽 아님
                && dist[nx][ny] == 0) {                  // 미방문
                dist[nx][ny] = dist[x][y] + 1;           // 거리 기록 = 방문 표시
                q.push({nx, ny});
            }
        }
    }
    return -1;                                        // 못 닿음
}
```

**3대 함정 (전부 여기서 막힘):**

| 함정 | 증상 | 해결 |
|------|------|------|
| `dist`/visited 없음 | 같은 칸 무한 재방문 → 타임아웃 | `dist[nx][ny]==0` 체크 |
| `q.pop()` 빠짐 | front 고정 → 무한루프 | front 읽고 **바로** pop |
| `answer++`로 거리 셈 | 칸 개수일 뿐, 거리가 아님 | `dist[다음]=dist[현재]+1` |

**프로그래머스 적응 포인트:** 보드(`maps`)를 **완성본으로 줌** (1=길 / 0=벽). 백준처럼 입력 파싱 불필요.
- 추적법 ① **별도 `dist` 배열** — 원본 보존, 디버깅 쉬움 ← **권장 (면접 무난)**
- 추적법 ② in-place `maps[nx][ny] = maps[x][y]+1` — 배열 절약, 원본 훼손

---

## 자체 점검 질문
1. unique_ptr이 복사 안 되고 이동만 되는 이유 (소유권 단일성)
2. shared_ptr 참조 카운트가 atomic인 이유 + Control Block 위치
3. vector capacity가 size보다 큰 이유 + 2배 증가의 amortized 분석
4. 퀵정렬 최악 O(n²) 케이스 + 회피법 (Pivot 선택)
5. unordered_map이 map보다 느려질 수 있는 상황 (해시 충돌)
6. 가상 소멸자 없으면 발생하는 문제 (예시 코드)
7. std::move 후 원본 객체 상태 (Valid but unspecified)

## 알고리즘 손풀기 추천 (프로그래머스)
- **Lv1 20문제** (배열/문자열/해시) — 구현력 회복
- **Lv2 10문제** (정렬/완전탐색/스택·큐/BFS·DFS) — 사고력
- **카카오 블라인드 기출** — 실전 감각 (한국 게임사 시험 스타일 유사)
- 프로그래머스 = 다수 한국 게임사 필기 플랫폼 → 환경 적응 효과
