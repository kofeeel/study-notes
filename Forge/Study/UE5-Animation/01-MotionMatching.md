---
title: Motion Matching 개념서
tags: [UE5, animation, motion-matching, study]
created: 2026-05-16
updated: 2026-05-16
sources:
  - https://dev.epicgames.com/documentation/en-us/unreal-engine/motion-matching-in-unreal-engine
  - https://dev.epicgames.com/documentation/en-us/unreal-engine/motion-matching-quick-start-in-unreal-engine
  - https://dev.epicgames.com/documentation/en-us/unreal-engine/pose-search-in-unreal-engine
  - https://dev.epicgames.com/community/learning/talks-and-demos/qz4z/unreal-engine-game-animation-sample-project
---

# Motion Matching 개념서

## 목표
- Motion Matching이 **뭘 하는 시스템인지** 한 줄로 설명
- DB / Schema / Channel / Cost가 각각 뭔지 구분
- 인간형 보스에 적용 여부 판단

## 핵심

### 한 줄 정의

**Motion Matching = 매 0.1초마다 "지금 캐릭터 상태"와 "DB에 저장된 모든 애니 프레임"을 비교해서, 가장 어울리는 프레임을 골라 재생하는 시스템.**

### 옛날 방식(State Machine)과 뭐가 다른가

| | State Machine | Motion Matching |
|---|---|---|
| 동작 결정 | 정해진 노드 사이 전환 (걷기→뛰기→정지) | DB 검색으로 매번 새로 결정 |
| 핵심 작업 | 노드 사이 Blend 튜닝 | 검색 (Search) |
| 애니메이션 양↑ 시 | 그래프 폭발 (관리 불가) | 오히려 부드러워짐 |
| 비유 | 지하철 (정해진 노선) | 자전거 (어디든 자유) |

### 시스템 부품 — 자연어 정의

1. **Pose Search Database (DB)** — 캐릭터가 할 수 있는 **모든 동작의 모든 프레임**을 모아둔 자료. 각 프레임은 "위치 / 속도 / 자세" 같은 숫자 묶음으로 변환되어 저장됨.
2. **Pose Search Schema** — DB의 인덱싱 규칙. "어떤 정보를 어떤 가중치로 저장할지" 정의. DB 만들 때 한 번 정하고 끝.
3. **Channel** — Schema가 정의하는 **정보의 종류**. 표준 3가지:
   - **Trajectory** = 어디로 가려고 하는가 (미래/과거 경로)
   - **Pose** = 지금 손/발이 어디 있는가 (자세)
   - **Velocity** = 지금 얼마나 빠르게 움직이는가 (속도)
4. **Cost Function** — "현재 상태"와 "DB의 한 프레임"이 얼마나 **다른지** 점수로 계산. 점수가 낮을수록 비슷. (각 채널의 차이를 제곱해서 가중치로 더하는 방식 = Weighted L2 거리)
5. **Motion Matching Node** — ABP에 두는 검색 노드. 0.1초마다 DB 전체 훑어서 최저 Cost 프레임 선택.
6. **Trajectory Generator** — 입력(WASD/스틱)을 받아 "0.5초 후엔 여기쯤 갈 거야"라는 예측 경로 만드는 컴포넌트. (Chooser Table 또는 Character Mover Component)
7. **Pose History Node** — 지난 N프레임의 캐릭터 자세를 기억. Schema의 "과거 Trajectory" 채널과 비교할 때 사용.

### 작동 흐름 (3단계 반복)

| 단계 | 일어나는 일 |
|---|---|
| 1. 현재 상태 수집 | 입력 → 예측 경로(Trajectory Generator) + 과거 자세(Pose History) → 하나의 **Query Vector** 조립 |
| 2. DB 검색 | DB 모든 프레임 ↔ Query Vector의 Cost 계산 → 최저 Cost 프레임 1개 선택 |
| 3. 선택 프레임 재생 | 그 프레임 위치에서 애니 재생 시작. **0.1초 후 1단계로 돌아감** |

→ 1-3을 끝없이 반복. 입력이 바뀌면 다음 검색에서 자연스럽게 새 프레임으로 갈아탐 = **부드러운 분기**.

### Schema 가중치 튜닝

특정 채널의 가중치를 올리면 그 채널의 차이가 Cost에 더 크게 반영됨 → 그 채널 일치를 우선시함.

| 가중치 ↑ | 결과 |
|---|---|
| Trajectory | 입력 방향 빠르게 따라감 (반응성↑, 관성↓) |
| Pose | 자세 전환 부드러움 (보간 자연, 반응성↓) |
| Velocity | 가감속 부드러움 (급변 억제) |

**콘솔 디버깅**:
- `PoseSearch.Debug 1` — 현재 매칭 프레임 + Cost 점수를 화면에 시각화
- `PoseSearch.Continuing 0` — 매 프레임 강제 재검색 (포즈 튐 원인 발견용)

### 인간형 보스/AI에 쓰나?

**결론: 보스 메인 시스템으로는 거의 안 씀.**

| 이유 | 설명 |
|---|---|
| 1. 데이터 비용 | 보스 1개 위해 수백~수천 프레임 캡쳐 필요 → ROI 낮음 |
| 2. 보스 = 패턴 | 정해진 공격 콤보. State Machine + Montage가 명확하고 효율적 |
| 3. 입력 없음 | AI 결정 기반 → "유저 의도 반영"이라는 MM 강점이 무용 |
| 4. 밸런싱 | 공격 프레임 정확히 통제해야 함. DB Search는 비결정적 |

**예외**: 인간형 + 자연스러운 이동 보스(Elden Ring Malenia급)는 **이동만 MM, 공격은 Montage** 하이브리드.

**소울라이크 보스 실무 표준**:
이동(SM 또는 MM) + 공격 트리거(AI Behavior Tree) + 공격 재생(Montage Root Motion) + **공격 중 위치 보정(Motion Warping)** ← 진짜 핵심

### 한계

DB 외 동작 불가 / Montage 강제 재생 시 Search 중단 → Pose Mismatch / Schema 가중치 튜닝 블랙박스적 / DB 크기 ∝ 메모리·검색 비용 / AI 캐릭터 ROI 낮음

## 체크
1. Motion Matching은 매 몇 초마다 검색? → 기본 0.1초 (SearchInterval)
2. State Machine 대비 핵심 차이? → 노드 전환이 아니라 매 프레임 DB 검색
3. Channel 3종은? → Trajectory, Pose, Velocity
4. Cost 점수의 의미? → 낮을수록 비슷. 최저 Cost 프레임이 선택됨
5. GASP Dodge 어색 1차 원인? → Dodge가 DB 밖 Montage라 Search가 중단됨
6. 인간형 보스 메인으로 쓰나? → 거의 안 씀. 이동만 일부. 공격은 Montage 표준
7. 보스에서 진짜 핵심 시스템은? → Motion Warping (공격 중 위치 보정)
