# 학습 위키 홈

> 저장소: `F:/tools/kofeel/study-repo`  
> 갱신: 2026-06-16

---

## 언리얼 엔진 5

### 애니메이션
- [Motion Matching 학습](Study/UE5-Animation/01-MotionMatching.md) — UE5 MotionMatching 시스템 개념 + 셋업
- [Motion Warping 학습](Study/UE5-Animation/02-MotionWarping.md) — 루트모션 워핑, 타겟 재조준 패턴
- [GASP + Mover 학습 가이드](Engine/UE5/UE5.7%20GASP%20+%20Mover%20학습%20가이드.md) — UE5.7 차세대 캐릭터 이동 시스템

### GAS (Gameplay Ability System)
- [GAS 네이밍 컨벤션](Conventions/UE5-GAS-Naming-Convention.md) — GA/GE/AS/GC 네이밍 규칙 정리
- [ProjectKD GAS 치트시트](../Portfolio/ProjectKD-GAS핵심개념-치트시트.md) — 실 코드 기반 GAS 핵심개념 면접용 요약

### AI / EQS
- [적 AI 포지셔닝 — 토큰/EQS/Crowd 직교 모델](ProjectKD-Wiki/ai-eqs-crowd-eqs.md) — 실전: 공유 BT + EQS 설계 원칙, PIE 함정 5개 + 진단법 (Project_KD)
- [StellarBlade AI 시스템 분석](Game-Analysis/StellarBlade/04-AI-시스템.md) — 실 게임 BT/AI 구조 역설계
- [StellarBlade BT 심층분석](Game-Analysis/StellarBlade/15-BT-심층분석.md) — SB 비헤이비어 트리 패턴 정리
- [StellarBlade BT 인덱스 (149개 트리)](Game-Analysis/StellarBlade/BT-Analysis/md/00-BT-INDEX.md) — 몬스터별 BT 원문 트리

### 네트워크 / 리플리케이션
- [Replication & RPC](Network/UE-Replication-RPC.md) — UPROPERTY Replicated, OnRep_, RPC 패턴
- [Reliable UDP / NetDriver](Network/UE-Reliable-UDP-NetDriver.md) — UE 네트워크 드라이버 구조
- [Network Sync](Network/UE-Network-Sync.md) — 서버-클라 동기화 패턴
- [Networking Fundamentals](Network/Networking-Fundamentals.md) — 게임 네트워킹 기초 개념

### 빌드 시스템
- [UE Build System](Engine/UE5/UE-Build-System.md) — Build.cs, 모듈 시스템, 빌드 구성

### 렌더링
- [UE Rendering Pipeline](Engine/UE5/UE-Rendering-Pipeline.md) — UE5 렌더링 파이프라인 (Nanite/Lumen 포함)
- [UMG / CommonUI](Engine/UE5/UMG_CommonUI_StudyGuide.md) — UI 위젯 시스템 + CommonUI 셋업

### 에셋 시스템
- [UE 에셋 시스템](Engine/UE5/UE%20에셋%20시스템.md) — AssetManager, SoftObjectPtr, 비동기 로드

---

## C++ / 현대 C++

- [EMC Quiz 인덱스](CPP/_이전문서/00-EMC-QUIZ-INDEX.md) — Effective Modern C++ 챕터별 퀴즈 모음
- [Ch1-2 타입 추론](CPP/_이전문서/QUIZ-EMC-Ch1-2-TypeDeduction.md) — auto, decltype, template 타입 추론
- [Ch3-4 모던 C++ / 스마트 포인터](CPP/_이전문서/QUIZ-EMC-Ch3-4-ModernCPP-SmartPtr.md) — nullptr, unique_ptr, shared_ptr
- [Ch5-6 이동 시맨틱 / 람다](CPP/_이전문서/QUIZ-EMC-Ch5-6-MoveSemantics-Lambda.md) — move, forward, 람다 캡처
- [Ch7-8 동시성 / 기타](CPP/_이전문서/QUIZ-EMC-Ch7-8-Concurrency-Tweaks.md) — thread, atomic, 미세 조정

---

## OS (운영체제)

- [OS 학습 가이드](Docs/OperationSystem/00-운영체제-학습-가이드.md) — 공룡책 기반 학습 로드맵
- [OS 개요 및 시스템 구조](Docs/OperationSystem/01-OS-개요-및-시스템-구조.md) — 커널, 시스템 콜, 부팅
- [프로세스 관리](Docs/OperationSystem/02-프로세스-관리.md) — PCB, 스케줄링, 컨텍스트 스위치
- [동기화 및 데드락](Docs/OperationSystem/03-동기화-및-데드락.md) — mutex, semaphore, 데드락 조건
- [메모리 관리](Docs/OperationSystem/04-메모리-관리.md) — 페이징, 세그먼테이션, 가상 메모리
- [저장장치 및 파일 시스템](Docs/OperationSystem/05-저장장치-및-파일시스템.md) — 디스크 스케줄링, inode
- [OS 면접 치트시트](Docs/OperationSystem/06-면접-치트시트.md) — 핵심 개념 1페이지 요약
- [OS 플래시카드](면접필기/OS-flashcards.md) — 암기용 Q&A 카드

---

## 컴퓨터 구조

- [학습 가이드](Study/ComputerArchitecture/00-학습가이드.md) — Patterson & Hennessy 기반 로드맵
- [추상화와 기술](Study/ComputerArchitecture/01-컴퓨터-추상화와-기술.md) — 8대 아이디어, 성능 측정
- [명령어 — 컴퓨터의 언어](Study/ComputerArchitecture/02-명령어-컴퓨터의-언어.md) — MIPS/RISC-V ISA
- [컴퓨터 산술](Study/ComputerArchitecture/03-컴퓨터-산술.md) — ALU, 정수/부동소수점
- [프로세서](Study/ComputerArchitecture/04-프로세서.md) — 단일/파이프라인 데이터패스
- [메모리 계층구조](Study/ComputerArchitecture/05-메모리-계층구조.md) — 캐시, DRAM, 지역성
- [병렬 프로세서](Study/ComputerArchitecture/06-병렬-프로세서.md) — SIMD, 멀티코어, GPU
- [면접 핵심 Q&A](Study/ComputerArchitecture/07-면접-핵심-QnA.md) — 자주 나오는 질문 모음

---

## 네트워크 (CS 기초)

- [CS 기초](면접필기/01-CS.md) — 네트워크/자료구조/알고리즘 면접 핵심

---

## 게임 디자인 / 게임 분석

- [게임 에셋 분석 방법론](Game-Analysis/00-게임-에셋-분석-방법론.md) — 실 게임 역설계 워크플로우
- [StellarBlade 인덱스](Game-Analysis/StellarBlade/00-INDEX.md) — SB 전체 분석 허브
- [StellarBlade 전투 시스템](Game-Analysis/StellarBlade/03-전투시스템.md) — 패링/경직/콤보 설계 분석
- [StellarBlade UE5 GAS 설계안](Game-Analysis/StellarBlade/13-UE5-GAS-설계안.md) — SB 구조를 UE5 GAS로 재설계한 레퍼런스 (우리 아키텍처와 교차검증)
- [StellarBlade 캐릭터 시스템](Game-Analysis/StellarBlade/05-캐릭터-시스템.md) — 스탯/상태/진행 구조

---

## 그래픽스

- [그래픽스 면접 필기](면접필기/03-그래픽스.md) — 렌더링 파이프라인, 셰이더, PBR 핵심

---

## Project_KD 실전 경험

### 문제 해결 위키 (실 구현 + 삽질 기록)
- [적 AI 포지셔닝 — 토큰/EQS/Crowd 직교 모델](ProjectKD-Wiki/ai-eqs-crowd-eqs.md) — EQS 설계, BT 거리밴드 함정 5개, melee 포위 EQS 검증 완료
- [WeaponTrace 패링 시스템](ProjectKD-Wiki/weapontrace.md) — 패링 게이팅 구조 규명, per-faction CDO 비대칭, 윈도우별 트레이스 오버라이드
- [원거리 적 발사체 조준/궤적](ProjectKD-Wiki/ranged-enemy-projectile-aiming.md) — muzzle→타겟 방향, 직선 vs 포물선 판단 기준

### 포트폴리오 문서
- [학습 인덱스](../Portfolio/ProjectKD-코드해설-00-학습인덱스.md) — 코드 해설 시리즈 전체 목차
- [코드해설 01 — WeaponTrace 패링](../Portfolio/ProjectKD-코드해설-01-WeaponTrace패링.md) — 면접용 코드 해설
- [코드해설 02 — 적 AI 직교 모델](../Portfolio/ProjectKD-코드해설-02-적AI직교모델.md) — 면접용 코드 해설
- [코드해설 03 — 사망/처형/데스블로](../Portfolio/ProjectKD-코드해설-03-사망처형데스블로.md) — 면접용 코드 해설
- [코드해설 04 — 히트피드백/비대칭 경직](../Portfolio/ProjectKD-코드해설-04-히트피드백-비대칭경직.md) — 면접용 코드 해설
- [GAS 핵심개념 치트시트](../Portfolio/ProjectKD-GAS핵심개념-치트시트.md) — 실 소스 기반 면접 대비
- [전투 시스템 원페이저](../Portfolio/ProjectKD-전투시스템-원페이저.md) — 1장 요약
- [담당 작업 시스템 정리](../Portfolio/ProjectKD-담당작업-시스템정리.md) — 이력서/포폴 기재용
- [문제 해결 사례집](../Portfolio/ProjectKD-문제해결-사례집.md) — STAR 형식 문제-원인-해결 사례
- [패링 기능 명세서](../Portfolio/ProjectKD-패링-기능명세서.md) — 기능 스펙 문서

---

## 면접 준비 (통합)

- [학습 계획](면접필기/00-학습계획.md) — 면접 준비 로드맵
- [C++ / 알고리즘](면접필기/02-CPP-알고리즘.md) — C++ 심화 + PS 핵심
- [그래픽스](면접필기/03-그래픽스.md) — 그래픽스 면접 필기
- [물리](면접필기/04-물리.md) — 물리 시뮬레이션 면접 핵심
- [OS 핵심 — 공룡책](면접필기/06-OS핵심-공룡책.md) — OS 면접 핵심 정리
