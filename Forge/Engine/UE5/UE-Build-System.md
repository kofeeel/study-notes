---
title: UE 빌드 시스템 (UBT · UHT · Module · Build.cs · Live Coding)
tags: [unreal-engine, build-system, ubt, uht, module, livecoding, cpp]
created: 2026-06-16
status: 검증완료(2026-06-16)
---

# UE 빌드 시스템

> 대상 엔진: UE5.6 (우리 프로젝트). 일부 설명은 5.7 공식 문서로 교차 확인.

---

## 1. 한 줄 정의

UE 빌드 시스템은 **C++ 소스를 모듈 단위로 모아, 리플렉션 코드를 자동 생성하고(UHT), 실제 컴파일·링크를 지휘하는(UBT) 도구 묶음**이다. `Build.bat` 한 줄로 이 전체가 돈다.

---

## 2. 왜 필요한가 (구체 문제)

순수 C++만으로는 UE가 못 하는 일이 있다:

1. **에디터가 C++ 변수를 모른다.** Details 패널에 `Health` 슬라이더를 띄우려면, 엔진이 런타임에 "이 클래스에 `Health`라는 float가 있다"를 알아야 한다. 표준 C++에는 그런 정보(리플렉션)가 없다.
2. **BP가 C++ 함수를 못 부른다.** Blueprint에서 `TakeDamage` 노드를 쓰려면 그 함수의 이름·인자·타입을 엔진이 알아야 한다.
3. **GC가 포인터를 못 추적한다.** `UPROPERTY()` 없는 `UObject*`는 가비지 컬렉터가 살아있는지 모른다 → 크래시.
4. **수백만 줄을 매번 다 컴파일하면 끔찍하다.** 엔진+게임 합치면 파일이 수만 개. 바뀐 것만 골라 빌드해야 한다.

UE는 이걸 **UHT(정보 생성) + UBT(빌드 지휘)** 두 도구로 푼다.

---

## 3. 어떻게 동작하나 (순서)

`Build.bat Project_KDEditor Win64 Development` 한 줄을 치면 내부에서 이 순서로 돈다:

```
1. UBT 시작
   └ *.Target.cs 읽기      → 무엇을 빌드할지 (Editor? Game?)
   └ *.Build.cs 읽기        → 각 모듈의 의존성 목록
   └ 어떤 파일이 바뀌었나 판정 (증분 빌드)

2. UHT 실행 (헤더 파싱)
   └ UCLASS/USTRUCT/UFUNCTION/UPROPERTY 매크로 스캔
   └ 리플렉션 코드 생성:
       MyActor.generated.h   (헤더에 #include 됨)
       MyActor.gen.cpp       (실제 등록 코드)
   └ 위치: Intermediate/Build/Win64/.../Inc/Project_KD/

3. 컴파일러(MSVC) 실행
   └ 내 .cpp + UHT가 만든 .gen.cpp 함께 컴파일
   └ .obj 생성

4. 링커 실행
   └ .obj들 묶어서 .dll / .exe 생성
   └ 에디터 타깃이면 UnrealEditor-Project_KD.dll
```

핵심은 **2단계(UHT)가 3단계(컴파일)보다 먼저**라는 점이다. 그래서 `.generated.h`가 항상 존재한다고 가정하고 코드를 짤 수 있다.

### 3-1. `.generated.h`와 매크로의 관계

헤더에서 이 규칙은 강제다:

```cpp
#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"   // ★ 반드시 맨 마지막 include

UCLASS()
class PROJECT_KD_API AMyActor : public AActor
{
    GENERATED_BODY()            // ★ 클래스 본문 맨 위
    // ...
};
```

- `MyActor.generated.h`는 **항상 마지막 include**여야 한다 (UHT 규칙). 뒤에 다른 include가 있으면 컴파일 에러.
- `GENERATED_BODY()`는 UHT가 만든 생성자·등록 코드를 그 자리에 펼쳐 넣는 자리표시자다. UCLASS면 필수.

---

## 4. Module 구조와 Build.cs / Target.cs

### 4-1. Module이란

> 모듈은 "Unreal Engine 소프트웨어 아키텍처의 기본 빌딩 블록"이다 (공식 문서). 기능 단위로 코드를 묶은 라이브러리 한 덩어리.

우리 프로젝트의 기본(primary) 모듈은 `Project_KD` 하나. 모든 게임 C++가 여기 들어있다. 모듈로 인정받으려면 **`[모듈명].Build.cs` 파일이 필수**다.

### 4-2. 우리 `Project_KD.Build.cs` (실제)

```csharp
public class Project_KD : ModuleRules
{
    public Project_KD(ReadOnlyTargetRules Target) : base(Target)
    {
        PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;

        PublicDependencyModuleNames.AddRange(new string[] {
            "Core", "CoreUObject", "Engine", "InputCore",
            "EnhancedInput",
            "GameplayAbilities", "GameplayTags", "GameplayTasks",  // GAS 3종 세트
            "AIModule", "NavigationSystem", "MotionWarping",
            "Niagara", "UMG", "Slate", "SlateCore",
            "LevelSequence", "MovieScene"
        });

        PrivateDependencyModuleNames.AddRange(new string[] {  });

        PublicIncludePaths.AddRange(new string[] { "Project_KD" });
    }
}
```

(인라인 주석은 설명용 — 실제 파일엔 없음. `PublicIncludePaths`로 `Project_KD` 폴더를 인클루드 경로에 추가해 `#include "Foo.h"`를 모듈 루트 기준 상대경로로 쓸 수 있게 한다.)

- `ModuleRules`를 상속한 C# 클래스다 (Build.cs는 C++가 아님).
- GAS를 쓰려면 `GameplayAbilities` `GameplayTags` `GameplayTasks` **3개 다** 넣어야 한다 (우리 CLAUDE.md §2-3과 동일).

### 4-3. Public vs Private DependencyModuleNames

| | 언제 쓰나 | 효과 |
|---|---|---|
| **PublicDependencyModuleNames** | 그 모듈 타입을 내 **`.h`(public 헤더)** 에 노출할 때 | 나를 의존하는 다른 모듈도 그 헤더를 자동으로 쓸 수 있음 |
| **PrivateDependencyModuleNames** | 그 모듈을 내 **`.cpp`에서만** 쓸 때 | 외부에 안 새어나감. 컴파일 시간↓ |

공식 규칙: **`.h`에서 쓰면 Public, `.cpp`에서만 쓰면 Private.** Private로 내리면 그 의존성이 나를 의존하는 하위 모듈로 전파되지 않는다 — 단일 게임 모듈(우리처럼 모듈이 `Project_KD` 하나뿐)에서는 전파받을 하위 모듈이 없어 빌드 시간 차이는 실질적으로 없고, **의도 표현(캡슐화)** 의미가 더 크다. 우리는 지금 거의 다 Public에 몰아넣었는데, 헤더에서 안 쓰는 모듈을 Private로 내리는 건 모듈을 더 쪼갤 때 의미가 생긴다. ⚠️ (어떤 모듈이 `.cpp` 전용인지 구체 분류는 안 함 — 미검증)

### 4-4. `*.Target.cs` — 무엇을 빌드할지

모듈이 "코드 덩어리"라면, Target은 "그 덩어리들로 무슨 실행물을 만들지"다. 우리는 둘 있다:

```csharp
// Project_KDEditor.Target.cs  — 에디터에서 열 때
public class Project_KDEditorTarget : TargetRules {
    Type = TargetType.Editor;
    DefaultBuildSettings = BuildSettingsVersion.V5;
    IncludeOrderVersion = EngineIncludeOrderVersion.Unreal5_6;
    ExtraModuleNames.Add("Project_KD");
}

// Project_KD.Target.cs  — 독립 실행(쿡된 게임)
public class Project_KDTarget : TargetRules {
    Type = TargetType.Game;
    // 나머지 동일
}
```

- **Editor 타깃**: 에디터로 프로젝트를 열고 코드 변경을 보려면 이게 필요.
- **Game 타깃**: 쿡된 콘텐츠로 도는 독립 실행 파일.
- 멀티플레이어용 `Client` / `Server` 타깃도 있지만, 쓰려면 각각 `.Target.cs`가 있어야 함 (우린 싱글이라 불필요).
- `IncludeOrderVersion = Unreal5_6` → 5.6 기준 IWYU 헤더 정렬 규칙 적용.

### 4-5. IWYU 포함 정책

> 모듈은 "Include What You Use(IWYU)" 표준을 따른다 — "실제로 쓰는 코드에만 헤더 include를 제한한다" (공식 문서).

쉽게: **쓰는 것만 직접 include하라.** 옛날엔 `Engine.h` 같은 거대 헤더 하나로 다 끌어왔는데(컴파일 폭발), 지금은 `GameFramework/Actor.h`처럼 필요한 것만 콕 집어 넣는다. 우리 글로벌 CLAUDE.md의 "헤더 인클루드 순서" 규칙이 이 정책의 실천이다.

---

## 5. 빌드 구성 (Debug / DebugGame / Development / Shipping)

`Build.bat ... <Configuration>` 의 마지막 인자. 무엇을 최적화하느냐가 다르다:

| 구성 | 엔진 최적화 | 게임코드 최적화 | 용도 |
|---|---|---|---|
| **Debug** | ✕ | ✕ | 엔진까지 디버깅. 느림. `-debug` 플래그 필요 |
| **DebugGame** | ○ | ✕ | **게임 코드만** 디버깅 (엔진은 빠르게 유지) — 게임 버그 잡을 때 |
| **Development** | ○ (대부분) | ○ (대부분) | **에디터 기본값.** 개발 중 일상 빌드 |
| **Shipping** | ○ | ○ | 출시용. 콘솔 명령·프로파일링 제거, 최고 성능 |
| **Test** | ○ | ○ | Shipping과 비슷하나 일부 콘솔·stats·프로파일링 살림 |

우리 `ue-build-check`는 항상 **`Development`** 로 빌드한다 (`Project_KDEditor Win64 Development`). 이유: 에디터 타깃의 표준 개발 구성이라, 우리가 평소 에디터에서 보는 것과 동일한 바이너리를 만든다.

---

## 6. Live Coding vs 일반 빌드

### 6-1. 둘의 차이

| | 일반 빌드 (`Build.bat`) | Live Coding |
|---|---|---|
| 언제 | 에디터 꺼진 상태 | 에디터 **켜진 채로** |
| 트리거 | 커맨드라인 / IDE 빌드 | 에디터·IDE에서 **Ctrl+Alt+F11** |
| 방식 | .dll/.exe 전체 재생성 + 재시작 | 도는 엔진에 바이너리를 **패치** (재시작 X) |
| 속도 | 느림 (링크까지) | 빠름 (변경분만) |

Live Coding은 Live++ 기반으로 **"엔진이 도는 동안 C++를 다시 빌드해 바이너리를 패치"** 한다 (공식 문서). PIE 중에도 안전하게 함수 본문을 고칠 수 있다.

### 6-2. Live Coding의 한계 (중요)

공식 문서 + 우리 경험상:

- **`.cpp` 생성자에서 정한 기본값은 기존 인스턴스에 반영 안 됨** (공식). 반대로 **`.h`의 기본값 변경은 반영됨**.
- **새 함수/새 변수 묶음/대규모 리팩토링은 예측 불가** — 흔히 크래시 (공식). 즉 **시그니처를 바꾸는 큰 변경은 Live Coding 말고 클린 빌드**가 정석.
- UCLASS/UFUNCTION/USTRUCT 구조 변경은 Object Reinstancing으로 처리되지만 포인터 관리가 까다로워 불안정.

### 6-3. ⚠️ 우리 프로젝트 함정 — Build.bat ↔ Live Coding 충돌

`ue-build-check`(`Build.bat`)를 돌릴 때 **에디터가 켜져 있으면 빌드 실패**한다:

```
에디터 ON → Live Coding이 .dll을 잠금
         → Build.bat의 링크 단계 실패
         → "Unable to build while Live Coding is active"
         → Result: Failed (OtherCompilationError)
```

이건 **코드 잘못이 아니라 환경(red)** 이다. 이때 단서: 로그에 `Reflection code generated`가 떠 있으면 **UHT는 통과한 것** → 헤더 매크로는 유효하다는 증거. 해결:

- (a) 에디터에서 **Ctrl+Alt+F11**로 Live Coding 검증 (에디터 유지, 빠름), 또는
- (b) 에디터 종료 후 클린 빌드 (전체 링크 증거 필요할 때). ⚠️ 에디터는 사용자가 직접 종료 — 미저장 작업 보호.

---

## 7. 면접 Q&A

**Q1. UHT와 UBT의 차이는?**
UHT(UnrealHeaderTool)는 **헤더를 파싱해 리플렉션 코드를 생성**하는 도구다 — `UCLASS`/`UPROPERTY` 매크로를 읽어 `.generated.h`와 `.gen.cpp`를 만든다. UBT(UnrealBuildTool)는 **빌드 전체를 지휘**하는 도구로, `.Target.cs`/`.Build.cs`를 읽어 무엇을·어떤 의존성으로·어떤 구성으로 빌드할지 정하고, UHT 실행 → 컴파일 → 링크를 순서대로 돌린다. UHT는 UBT가 부르는 한 단계.

**Q2. `.h`에 쓰는 모듈은 Public, `.cpp`에만 쓰는 모듈은 Private에 넣으라는 이유는?**
public 헤더에 노출되는 타입은 나를 의존하는 다른 모듈도 그 헤더를 include하면서 따라 쓰게 된다. 그래서 `.h`에 등장하는 의존성은 `PublicDependencyModuleNames`에 넣어 전파를 허용해야 컴파일이 된다. 반대로 `.cpp` 내부에서만 쓰는 건 외부에 새어나갈 필요가 없으니 `Private`에 넣는다 — 의존성 전파를 막아 **빌드 시간이 줄고 캡슐화가 유지**된다.

**Q3. Live Coding으로 멤버 변수를 추가하면 안 되는 이유는?**
Live Coding은 도는 바이너리를 in-place로 패치하는 방식이라 **함수 본문 같은 작은 변경엔 안전**하지만, 새 변수·새 함수 묶음·대규모 리팩토링은 메모리 레이아웃과 리플렉션 등록을 바꿔 예측 불가하게 동작하고 흔히 크래시한다. 클래스 구조(시그니처)를 바꿀 땐 에디터를 닫고 **클린 빌드**해야 한다. 그래서 우리 룰도 ".h(시그니처)는 클린 빌드로 검증"이다.

---

## 출처

- [Unreal Build Tool in Unreal Engine | UE5.7 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-build-tool-in-unreal-engine)
- [Unreal Engine Modules | UE5.7 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-modules) — Module 정의, IMPLEMENT_MODULE, Build.cs, Public/Private DependencyModuleNames, Public/Private 폴더, IWYU
- [Build Configurations Reference | 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/build-configurations-reference?application_version=4.27) — Debug/DebugGame/Development/Shipping/Test, Target 타입(Game/Editor/Client/Server)
- [Using Live Coding to Recompile Unreal Engine Applications at Runtime | UE5.7 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime) — Ctrl+Alt+F11, 생성자 기본값 한계, 대규모 변경 한계
- [Objects | UE 공식](https://dev.epicgames.com/documentation/en-us/unreal-engine/objects?application_version=4.27) — UObject 리플렉션 매크로
- [Unreal Property System (Reflection) | Epic 공식 블로그](https://www.unrealengine.com/blog/unreal-property-system-reflection) — UHT가 .generated.h / .gen.cpp 생성, GENERATED_BODY
- 우리 프로젝트 실제 파일: `Source/Project_KD/Project_KD.Build.cs`, `Source/Project_KDEditor.Target.cs`, `Source/Project_KD.Target.cs`, `.claude/skills/ue-build-check/SKILL.md`
