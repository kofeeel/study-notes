# 🎮 Effective Modern C++ — 마스터 퀴즈 & 학습 가이드

> **원서**: Scott Meyers, *Effective Modern C++* (O'Reilly, 2015)
> **총 42개 Item** | **4개 챕터별 퀴즈** | **총 100+ 문제**
> 📕 = 실물 책 페이지 참조

---

## 📋 퀴즈 맵

```
         ┌─────────────────────────────────────────────┐
  ★☆☆   │  Ch1-2: Type Deduction & auto  (Items 1-6)  │ ← 여기서 시작!
         └──────────────────┬──────────────────────────┘
                            ▼
         ┌─────────────────────────────────────────────┐
  ★★☆   │  Ch3-4: Modern C++ & Smart Pointers (7-22)  │
         └──────────────────┬──────────────────────────┘
                            ▼
         ┌─────────────────────────────────────────────┐
  ★★★   │  Ch5-6: Rvalue/Move & Lambda (23-34)        │ ← 최고 난이도
         └──────────────────┬──────────────────────────┘
                            ▼
         ┌─────────────────────────────────────────────┐
  ★★☆   │  Ch7-8: Concurrency & Tweaks (35-42)        │ ← 최종 보스 포함
         └─────────────────────────────────────────────┘
```

---

## 📂 퀴즈 파일

| # | 챕터 | 범위 | 문제 수 | 링크 |
|---|------|------|---------|------|
| 1 | **Type Deduction & auto** | Items 1-6 (📕 p.9-48) | ~23문제 | [[QUIZ-EMC-Ch1-2-TypeDeduction]] |
| 2 | **Modern C++ & Smart Pointers** | Items 7-22 (📕 p.49-156) | ~35문제 | [[QUIZ-EMC-Ch3-4-ModernCPP-SmartPtr]] |
| 3 | **Rvalue/Move & Lambda** | Items 23-34 (📕 p.157-240) | ~33문제 | [[QUIZ-EMC-Ch5-6-MoveSemantics-Lambda]] |
| 4 | **Concurrency & Tweaks** | Items 35-42 (📕 p.241-302) | ~31문제 | [[QUIZ-EMC-Ch7-8-Concurrency-Tweaks]] |

---

## 🎯 추천 학습 루트

### 🏃 Speed Run (1일 코스)
1. 각 퀴즈의 **📖 핵심 개념 요약**만 훑기
2. **OX 퀴즈**만 풀기
3. 틀린 문제 → 📕 원서 해당 페이지 복습

### 📚 Deep Dive (1주 코스)
| Day | 할 일 |
|-----|------|
| Mon | Ch1-2 전체 퀴즈 + 원서 Items 1-6 정독 |
| Tue | Ch3 퀴즈 (Items 7-17) + 원서 정독 |
| Wed | Ch4 퀴즈 (Items 18-22) Smart Pointer 집중 |
| Thu | Ch5 퀴즈 (Items 23-30) Move Semantics — **가장 어려움!** |
| Fri | Ch6 퀴즈 (Items 31-34) Lambda + 미니 챌린지 |
| Sat | Ch7-8 퀴즈 (Items 35-42) + 최종 보스 문제 |
| Sun | 전체 복습 — 틀린 문제 재풀이 + Top 10 Takeaways 확인 |

### 🔥 면접 대비 (3시간 코스)
1. 각 퀴즈의 **🔥 함정 문제**만 모아서 풀기 (총 17문제)
2. Ch7-8의 **🏆 최종 보스 문제** 풀기
3. Ch7-8 하단의 **전체 복습 Top 10 Takeaways** 암기

---

## 📊 Item 전체 목록 (Quick Reference)

### Ch1. Deducing Types
- [ ] Item 1: Understand template type deduction (📕 p.9)
- [ ] Item 2: Understand auto type deduction (📕 p.18)
- [ ] Item 3: Understand decltype (📕 p.23)
- [ ] Item 4: Know how to view deduced types (📕 p.30)

### Ch2. auto
- [ ] Item 5: Prefer auto to explicit type declarations (📕 p.37)
- [ ] Item 6: Use explicitly typed initializer idiom when auto deduces undesired types (📕 p.43)

### Ch3. Moving to Modern C++
- [ ] Item 7: Distinguish between () and {} when creating objects (📕 p.49)
- [ ] Item 8: Prefer nullptr to 0 and NULL (📕 p.58)
- [ ] Item 9: Prefer alias declarations to typedefs (📕 p.63)
- [ ] Item 10: Prefer scoped enums to unscoped enums (📕 p.67)
- [ ] Item 11: Prefer deleted functions to private undefined ones (📕 p.74)
- [ ] Item 12: Declare overriding functions override (📕 p.79)
- [ ] Item 13: Prefer const_iterators to iterators (📕 p.86)
- [ ] Item 14: Declare functions noexcept if they won't emit exceptions (📕 p.90)
- [ ] Item 15: Use constexpr whenever possible (📕 p.97)
- [ ] Item 16: Make const member functions thread safe (📕 p.103)
- [ ] Item 17: Understand special member function generation (📕 p.109)

### Ch4. Smart Pointers
- [ ] Item 18: Use std::unique_ptr for exclusive-ownership (📕 p.118)
- [ ] Item 19: Use std::shared_ptr for shared-ownership (📕 p.125)
- [ ] Item 20: Use std::weak_ptr for dangling-possible pointers (📕 p.134)
- [ ] Item 21: Prefer make_unique/make_shared to direct new (📕 p.139)
- [ ] Item 22: Pimpl Idiom — define special members in impl file (📕 p.147)

### Ch5. Rvalue References, Move Semantics, Perfect Forwarding
- [ ] Item 23: Understand std::move and std::forward (📕 p.158)
- [ ] Item 24: Distinguish universal references from rvalue references (📕 p.164)
- [ ] Item 25: Use std::move on rvalue refs, std::forward on universal refs (📕 p.168)
- [ ] Item 26: Avoid overloading on universal references (📕 p.177)
- [ ] Item 27: Alternatives to overloading on universal references (📕 p.184)
- [ ] Item 28: Understand reference collapsing (📕 p.197)
- [ ] Item 29: Assume move ops are not present, not cheap, not used (📕 p.203)
- [ ] Item 30: Perfect forwarding failure cases (📕 p.207)

### Ch6. Lambda Expressions
- [ ] Item 31: Avoid default capture modes (📕 p.216)
- [ ] Item 32: Use init capture to move objects into closures (📕 p.224)
- [ ] Item 33: Use decltype on auto&& params to std::forward them (📕 p.229)
- [ ] Item 34: Prefer lambdas to std::bind (📕 p.232)

### Ch7. The Concurrency API
- [ ] Item 35: Prefer task-based to thread-based programming (📕 p.241)
- [ ] Item 36: Specify std::launch::async if asynchronicity is essential (📕 p.245)
- [ ] Item 37: Make std::threads unjoinable on all paths (📕 p.250)
- [ ] Item 38: Be aware of varying thread handle destructor behavior (📕 p.258)
- [ ] Item 39: Consider void futures for one-shot event communication (📕 p.262)
- [ ] Item 40: Use std::atomic for concurrency, volatile for special memory (📕 p.271)

### Ch8. Tweaks
- [ ] Item 41: Consider pass by value for copyable, cheap-to-move, always-copied params (📕 p.281)
- [ ] Item 42: Consider emplacement instead of insertion (📕 p.292)

---

> [!tip] 학습 팁
> 체크박스를 활용하세요! 각 Item을 공부할 때마다 `[ ]` → `[x]`로 바꾸면 진행률을 추적할 수 있습니다.
> 실물 책이 있으니 퀴즈에서 틀린 문제는 반드시 📕 페이지를 펼쳐서 원문으로 복습하세요.
