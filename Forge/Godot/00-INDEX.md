---
tags: [sts2, godot, index, moc]
---

# Slay the Spire 2 — Godot 스터디 인덱스

> STS2 소스코드로 배우는 Godot 4 + C# 게임 개발 지식 베이스.
> 목표 프로젝트: **좀슐랭** (좀비 요리/사냥/생존 로그라이크)

---

## 빠른 시작 순서 (Godot 초보자용)

1. [[01-Godot-Engine-Basics]] — Godot의 기본 개념부터
2. [[02-Project-Structure]] — STS2 프로젝트 구조 파악
3. [[03-Architecture-Overview]] — 전체 아키텍처 이해
4. [[04-Model-System]] — 게임 오브젝트 데이터 구조
5. [[05-Hook-System]] — Observer 패턴 구현
6. [[06-GameAction-System]] — 비동기 액션 큐
7. [[10-Card-System]] — 카드 구현 심층 분석
8. [[15-Combat-Loop]] — 전투 루프 전체 흐름

---

## 카테고리별 문서 맵

### 기초 (Godot Fundamentals)
| 문서 | 설명 |
|------|------|
| [[01-Godot-Engine-Basics]] | Node/Scene 시스템, Autoload, Signal, C# vs GDScript |
| [[02-Project-Structure]] | 디렉토리 구조, src/Core 하위 폴더 역할, 에셋 관리 |
| [[03-Architecture-Overview]] | Model-Command-Hook 패턴, 시스템 간 의존성 다이어그램 |

### 핵심 시스템 (Core Systems)
| 문서 | 설명 |
|------|------|
| [[04-Model-System]] | AbstractModel, Canonical/Mutable 상태 분리, ModelId |
| [[05-Hook-System]] | Hook.cs 정적 메서드, IterateHookListeners, Before/After 패턴 |
| [[06-GameAction-System]] | GameAction 추상 클래스, ActionExecutor, 큐 처리 |
| [[07-Command-Utilities]] | Cmd.Wait, SceneTreeTimer, async/await Godot 연동 |
| [[08-CombatState]] | CombatState, CombatManager, 전투 상태 관리 |
| [[09-RunManager]] | RunManager 싱글톤, 런 진행 상태, 세이브 연동 |

### 게임플레이 (Gameplay)
| 문서 | 설명 |
|------|------|
| [[10-Card-System]] | CardModel 구조, 에너지 비용, 업그레이드, OnPlayWrapper |
| [[11-Relic-System]] | RelicModel, 장착/해제, 후크 구독 |
| [[12-Power-System]] | PowerModel, 스택, 턴 종료 처리 |
| [[13-Monster-System]] | MonsterModel, MonsterMove, 인텐트 시스템 |
| [[14-Map-System]] | ActMap, 노드 타입, 맵 생성 |
| [[15-Combat-Loop]] | 턴 시작→카드 플레이→턴 종료 전체 흐름 |
| [[16-Reward-System]] | 전투 보상, 카드/유물 선택 |

### 인프라 (Infrastructure)
| 문서 | 설명 |
|------|------|
| [[17-Save-System]] | 세이브/로드, PrefsSave, RunSave 구조 |
| [[18-Localization]] | LocString, 다국어 처리 방식 |
| [[19-Audio-FMOD]] | FMOD 연동, FmodManager autoload, 사운드 트리거 |
| [[20-Multiplayer]] | 멀티플레이어 아키텍처, NetAction, PlayerChoiceContext |

### 고급 (Advanced)
| 문서 | 설명 |
|------|------|
| [[21-Modding-System]] | 모딩 지원 구조, ModelDb, GenerateSubtypes |

---

## 태그 인덱스

- `#godot-basics` — Godot 엔진 기초
- `#architecture` — 아키텍처 패턴
- `#gameplay` — 게임플레이 시스템
- `#csharp` — C# 특화 내용
- `#zomblang` — 좀슐랭 적용 아이디어

---

## 참고 경로

- 소스 (GDScript/씬): `F:/Projects/godot_study/extracted/`
- 소스 (C# 디컴파일): `F:/Projects/godot_study/decompiled/sts2/MegaCrit/`
