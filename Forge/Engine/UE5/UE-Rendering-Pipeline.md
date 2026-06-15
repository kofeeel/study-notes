---
title: UE 렌더링 파이프라인 기초
tags: [unreal-engine, rendering, deferred-shading, nanite, lumen, gbuffer, shader, drawcall]
created: 2026-06-16
status: 검증완료(2026-06-16)
---

# UE 렌더링 파이프라인 기초

## 1. 한 줄 정의

화면 한 프레임을 그리는 일을 여러 단계(패스)로 쪼개서, 각 단계가 자기 일만 하고 결과를 다음 단계에 넘기는 흐름이다. UE는 기본적으로 **디퍼드 셰이딩(deferred shading)** 방식을 쓴다.

---

## 2. 왜 필요한가 (구체 문제)

라이팅(빛 계산)은 비싸다. 화면에 광원(라이트)이 100개 있고 픽셀이 200만 개라고 하자.

- **순진한 방식(포워드)**: 픽셀마다 그 위에 영향을 주는 모든 라이트를 계산한다. 게다가 한 픽셀 위에 물체가 여러 겹 겹쳐 있으면(앞 물체에 가려지는 뒤 물체) 안 보이는 픽셀까지 라이트 계산을 다 해버린다. → 낭비가 크다.
- **디퍼드 방식**: 일단 라이팅을 미룬다(deferred = 지연). 먼저 화면에 "실제로 보이는" 픽셀의 재질 정보(색, 거칠기, 노멀 등)만 텍스처에 적어둔다. 그다음 그 텍스처를 읽어서 라이팅을 한 번만 계산한다.

핵심 이득: **라이트 비용이 "물체 개수 × 라이트 개수"가 아니라 "화면 픽셀 수 × 라이트 개수"에 비례**한다. 라이트가 많은 씬에서 유리하다.

> 단점도 명확하다. 디퍼드는 GBuffer라는 큰 텍스처 묶음을 메모리에 들고 있어야 하고, MSAA 같은 하드웨어 안티에일리어싱이나 반투명을 자연스럽게 못 다룬다. 그래서 UE는 반투명을 따로 처리하고, 모바일/VR용으로는 포워드 셰이딩도 제공한다.

---

## 3. 어떻게 동작하나 (GBuffer + 패스 순서)

### 3-1. 디퍼드는 크게 두 패스

> "Deferred shading splits the rendering process into two passes: A geometry pass that handles BaseColor, Metallic, and Roughness... and stores them in a temporary buffer, usually called GBuffer. A lighting pass that reads the material attributes from GBuffer, computes lighting." (Epic 공식 문서)

1. **지오메트리 패스**: 재질 정보를 GBuffer에 기록
2. **라이팅 패스**: GBuffer를 읽어 라이팅 계산 → 최종 픽셀 출력

### 3-2. GBuffer가 담는 것

GBuffer는 "여러 장의 텍스처 묶음"이다. 픽셀 하나당 대략 이런 게 들어간다.

- **BaseColor** (기본 색)
- **Metallic / Roughness / Specular** (금속성/거칠기/반사 강도)
- **World Normal** (그 픽셀 표면이 향한 방향)
- **Depth** (카메라로부터의 거리) — 별도 뎁스 버퍼
- 그 외 ShadingModel ID, AO 등

라이팅 패스는 "이 픽셀은 색이 빨강, 거칠기 0.3, 노멀은 이 방향"을 GBuffer에서 읽고, 거기에 라이트를 적용해 "최종적으로 이 픽셀은 이 색"을 계산한다.

### 3-3. 한 프레임 패스 순서 (게임개발자가 알 깊이)

대략 이 순서로 흐른다.

1. **Depth Prepass (조기 뎁스)** — 불투명 물체의 깊이만 먼저 그린다. 이후 패스에서 "어차피 가려질 픽셀"을 미리 걸러내(early-Z) 중복 계산을 줄인다.
2. **Base Pass (지오메트리 패스)** — 불투명/마스크 물체의 재질 정보를 **GBuffer에 기록**.
3. **Lighting Pass** — GBuffer + 섀도우/라이트를 합쳐 라이팅 계산. (UE5에서는 여기에 Lumen GI/반사가 끼어든다.)
4. **Translucency (반투명)** — 유리, 연기, 파티클 등. **디퍼드로 처리 못 해서 별도로**, 보통 포워드 방식으로 뒤에 따로 그린다. 그래서 반투명은 라이팅 제약이 있다.
5. **Post-Process (후처리)** — 톤매핑, 블룸, 모션블러, 안티에일리어싱(TAA/TSR), 색보정 등. 이미 완성된 화면 이미지를 받아 화면 전체에 효과를 입힌다.

> 반투명을 따로 빼는 이유: GBuffer는 픽셀당 "딱 하나의 표면" 정보만 담는다. 반투명은 뒤가 비치므로 한 픽셀에 표면이 여러 겹이라 GBuffer 모델에 안 맞는다.

---

## 4. Material 에디터 → 셰이더 컴파일

UE에서 머티리얼은 노드 그래프로 만든다(아티스트 친화). 하지만 GPU는 노드를 모른다. GPU는 셰이더 코드(HLSL 계열)를 돌린다. 그래서 **번역**이 일어난다.

흐름:

```
Material 그래프 (노드)
   ↓  HLSL 코드로 번역 (Material Editor 우상단 "HLSL Code"로 확인 가능)
.usf / .ush 파일 (UE의 셰이더 소스 = HLSL 기반)
   ↓  플랫폼 독립 전처리 패스 (preprocessing)
   ↓  플랫폼별 컴파일러 (DX는 FXC/DXC, OpenGL은 HLSLCC로 GLSL 크로스컴파일 등)
플랫폼별 셰이더 바이트코드
```

- **USF** = Unreal Shader File. "USF shader files, based on HLSL language, is Unreal Engine shader file format that contains the multi platform shader code." (Epic 공식 문서)
- **USH** = 셰이더 헤더(include용).
- 머티리얼 픽셀 셰이더는 `GetMaterialPixelParameters`로 `FMaterialPixelParameters` 구조체를 채우고, `CalcMaterialParameters`로 나머지를 채운 뒤 `MaterialTemplate.usf`의 함수들로 머티리얼 입력에 접근한다. (Epic 공식 문서)

게임개발자가 체감하는 부분: **머티리얼을 처음 보일 때 "셰이더 컴파일 중" 스피너**가 그것이다. 머티리얼 1개당 쓰임새(패스, 버텍스 팩토리)별로 여러 셰이더 변종(permutation)이 컴파일된다. 그래서 마스터 머티리얼을 늘리면 컴파일 시간/패키징 시간이 늘고, **Material Instance(파라미터만 바꾼 자식)**를 쓰면 재컴파일 없이 값만 바뀐다.

---

## 5. Nanite와 Lumen — 각각 무슨 문제를 푸나 (간단)

### Nanite — 지오메트리(폴리곤) 문제

> "Nanite is Unreal Engine's virtualized geometry system which uses an internal mesh format and rendering technology to render pixel scale detail and high object counts. It intelligently does work on only the detail that is visible on-screen and no more." (Epic 공식 문서)

푸는 문제: 예전에는 폴리곤 수, 드로콜 수, 메시 메모리가 프레임 예산을 잡아먹어서 아티스트가 수동으로 LOD(거리별 저폴리 버전)를 만들어야 했다. Nanite는:

- 메시를 **계층적 삼각형 클러스터(hierarchical clusters of triangle groups)**로 쪼개 놓고, 런타임에 카메라 시점 기준으로 적당한 디테일 클러스터만 골라 스트리밍한다.
- "frame budgets are no longer constrained by polycounts, draw calls, and mesh memory usage." → ZBrush 스컬프트, 포토그래메트리 같은 영화급 원본을 LOD 수작업 없이 바로 쓸 수 있다.

제약 (게임개발자 필수):
- 블렌드 모드는 **Opaque / Masked만** 지원 (반투명 X).
- **Morph Target 미지원**, World Position Offset 제한적.
- 포워드 렌더링, 스테레오 VR, MSAA 미지원.
- 인스턴스 상한 **1600만(16 million)개로 하드락**.
- 버텍스 탄젠트를 저장 안 하고 픽셀 셰이더에서 유도.

### Lumen — 라이팅(간접광) 문제

> "Lumen is Unreal Engine 5's fully dynamic global illumination and reflections system... It is the default global illumination and reflections system." (Epic 공식 문서)

푸는 문제: 빛은 벽에 튕겨 주변을 밝히고(간접광=GI), 표면은 주변을 비춘다(반사). 예전엔 이걸 미리 구워서(라이트맵 베이크) 정적으로만 쓰거나 비싼 레이트레이싱을 써야 했다. Lumen은:

- **무한 바운스 디퓨즈 간접광 + 간접 반사**를 **베이크 없이 동적으로** 계산. 낮→밤 시간 변화, 움직이는 물체 모두 GI에 반영된다.
- Lumen GI가 SSGI/DFAO를, Lumen Reflections가 SSR을 대체한다.
- 레이트레이싱 방식 2종: **하드웨어 레이트레이싱**(GPU RT코어, 고품질, 기본값, 비쌈)과 **소프트웨어 레이트레이싱**(Signed Distance Field 기반).
- 설계 타깃: **차세대 콘솔(next-generation consoles)**. (⚠️ 미검증: "60fps" 수치는 공식 Lumen GI 문서 본문에 명시돼 있지 않음 — 출처 불명확하여 삭제)

> 한 줄 요약: **Nanite = 폴리곤 무한, Lumen = 빛 동적.** 둘은 독립이라 따로 켜고 끌 수 있다.

---

## 6. 드로콜·배칭·인스턴싱

### 드로콜(Draw Call)이란

CPU가 GPU에게 "이 메시를 이 셰이더로 그려라"라고 보내는 명령 1건. 드로콜 1건마다 CPU↔GPU 통신 오버헤드가 있다. **드로콜이 많으면 GPU가 놀아도 CPU가 병목**이 된다. 그래서 "같은 거 여러 개"는 한 번에 묶어 보내는 게 핵심.

UE 내부에서 드로콜은 `FMeshDrawCommand`로 표현된다.

> "`FMeshDrawCommand` ... a fully stateless draw description that stores everything that the RHI needs to know about a mesh draw" (Epic 공식 문서)

### 배칭(Batching)

성격이 같은 드로콜을 한 묶음으로 합치는 것. UE는 스태틱 메시의 드로 커맨드를 씬 추가 시 **한 번 캐싱(cached mesh draw commands)**해두고 매 프레임 재사용한다(매번 다시 만들지 않음).

### 인스턴싱(Instancing)

**같은 메시 + 같은 셰이더 바인딩**을 위치만 다르게 N개 그릴 때, 드로콜 1건으로 N개를 그리는 것. 나무 1000그루를 1000번이 아니라 (이상적으로) 1번에.

> "in order to merge two draws into an instanced one, they must have identical shader bindings (`FMeshDrawCommand::MatchesForDynamicInstancing`)." (Epic 공식 문서)

UE에서 인스턴싱을 쓰는 법:
- **`UInstancedStaticMeshComponent` (ISM)** — 동일 메시 인스턴스들의 드로콜을 합친다. 인스턴스별 데이터는 Per Instance Custom Data로 넘긴다.
- **`UHierarchicalInstancedStaticMeshComponent` (HISM)** — ISM + 거리 컬링/LOD(폴리아지에 적합).
- **Dynamic Instancing** — 셰이더 바인딩이 같은 드로콜들을 엔진이 자동으로 인스턴스 드로로 병합.
- **Merge Actors 툴**의 Batch 옵션으로 스태틱 메시 액터들을 ISM으로 묶을 수 있다.

게임개발자 실천: **드로콜을 줄이는 가장 쉬운 길은 "유니크한 메시/머티리얼 종류를 줄이는 것"**. 같은 머티리얼·같은 메시를 쓰면 합쳐지고, 머티리얼이 제각각이면 안 합쳐진다.

---

## 7. 흔한 함정

- **반투명에 라이팅이 안 먹는다고 당황** → 반투명은 디퓨드 라이팅 패스 밖(포워드)에서 별도 처리라 제약이 정상이다. 반투명 라이팅 모드를 확인하라.
- **머티리얼 종류를 마구 늘림** → 셰이더 변종 폭발 → 컴파일 시간/패키지 용량 증가 + 드로콜 병합 깨짐. 마스터 머티리얼 + Material Instance로 가라.
- **Nanite면 무조건 빠르다고 오해** → Nanite는 반투명·Morph·일부 WPO를 못 쓴다. 캐릭터(스키닝/모프) 워크플로우와 충돌할 수 있다. 적합한 에셋(불투명 환경 메시)에 써라.
- **"인스턴싱했는데 드로콜이 안 줄어요"** → 머티리얼/셰이더 바인딩이 인스턴스마다 다르면(예: 인스턴스별 다른 다이내믹 머티리얼) 병합 조건이 깨진다. 차이는 Per Instance Custom Data로 넘겨라.
- **Lumen 켜고 60fps 안 나옴** → 하드웨어 RT는 비싸다. 소프트웨어 RT나 스케일러빌리티 설정, 씬 복잡도를 조절하라.

---

## 8. 면접 Q&A

**Q1. 디퍼드 셰이딩이 포워드보다 라이트 많은 씬에서 유리한 이유는?**
A. 디퍼드는 화면에 보이는 픽셀의 재질을 GBuffer에 먼저 쓰고, 라이팅을 "화면 픽셀 수 × 라이트 수"로 한 번에 계산한다. 가려지는 픽셀이나 물체 개수에 라이트 비용이 비례하지 않아서, 라이트가 많을수록 포워드 대비 절약된다. 대가는 GBuffer 메모리 대역폭과 MSAA/반투명 제약이다.

**Q2. Nanite와 Lumen은 각각 무슨 문제를 푸나? 같이 켜야 하나?**
A. Nanite는 지오메트리(폴리곤/드로콜/LOD) 문제를, Lumen은 동적 간접광·반사(GI) 문제를 푼다. 서로 독립이라 따로 켜고 끌 수 있다. 다만 둘 다 불투명 환경 메시에서 시너지가 좋고, 캐릭터 같은 스키닝/반투명 에셋엔 Nanite 제약이 있다.

**Q3. 드로콜이 많아 CPU 병목일 때 어떻게 줄이나?**
A. 유니크한 메시/머티리얼 종류를 줄이고(같은 셰이더 바인딩이어야 병합됨), 같은 메시 다수는 ISM/HISM으로 인스턴싱, Merge Actors로 합치고, 가시성 컬링을 쓴다. UE는 셰이더 바인딩이 동일한 드로콜을 Dynamic Instancing으로 자동 병합한다(`MatchesForDynamicInstancing`).

---

## 출처

- Nanite Virtualized Geometry — https://dev.epicgames.com/documentation/en-us/unreal-engine/nanite-virtualized-geometry-in-unreal-engine
- Nanite Technical Details — https://dev.epicgames.com/documentation/unreal-engine/nanite-technical-details
- Lumen Global Illumination and Reflections — https://dev.epicgames.com/documentation/en-us/unreal-engine/lumen-global-illumination-and-reflections-in-unreal-engine
- Mobile Rendering and Shading Modes (디퍼드 2패스/GBuffer 설명) — https://dev.epicgames.com/documentation/unreal-engine/mobile-rendering-and-shading-modes-for-unreal-engine
- Forward Shading Renderer — https://dev.epicgames.com/documentation/en-us/unreal-engine/forward-shading-renderer-in-unreal-engine
- Shader Development (USF/HLSL, 컴파일 흐름) — https://dev.epicgames.com/documentation/unreal-engine/shader-development-in-unreal-engine
- Mesh Drawing Pipeline (FMeshDrawCommand, 캐싱, Dynamic Instancing) — https://dev.epicgames.com/documentation/en-us/unreal-engine/mesh-drawing-pipeline-in-unreal-engine
- Instanced Static Mesh Component — https://dev.epicgames.com/documentation/unreal-engine/instanced-static-mesh-component-in-unreal-engine
- Guidelines for Optimizing Rendering for Real-Time — https://dev.epicgames.com/documentation/en-us/unreal-engine/guidelines-for-optimizing-rendering-for-real-time-in-unreal-engine
