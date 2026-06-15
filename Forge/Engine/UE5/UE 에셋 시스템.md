### 1. 레퍼런스 카운팅 (Reference Counting)

빨간 경고 = 에셋이 **메모리에 로드된 상태**

UE는 에셋을 `UObject` 기반으로 관리하고, 내부적으로 GC(Garbage Collection)가 참조 카운트를 추적함.

cpp

````cpp
// UObject가 살아있는 이유 — 누군가 이걸 들고 있음
UPROPERTY() // 이 매크로가 없으면 GC가 수거해버림
UTexture2D* MyTexture;
```
→ **`UPROPERTY()` 없는 포인터는 GC에 의해 댕글링 포인터가 될 수 있다**는 이유가 여기서 나옴

---

### 2. 에셋 의존성 그래프
UE는 에셋 간 참조를 **방향 그래프**로 관리함
```
Lvl_Combat (레벨)
    └── BP_CombatEnemySpawner (블루프린트)
            └── IA_ChargedAttack (InputAction) ← 지금 삭제하려는 것
````

→ 하위 에셋 삭제 시 상위가 깨지는 구조. **의존성 방향을 항상 의식**해야 함

---

### 3. Soft Reference vs Hard Reference

저 경고가 뜨는 이유 = **Hard Reference**로 참조하고 있어서

|종류|선언|동작|
|---|---|---|
|Hard|`UPROPERTY() UObject*`|참조 대상이 항상 메모리에 로드됨|
|Soft|`TSoftObjectPtr<>`|필요할 때만 로드, 없어도 null로 graceful 처리|

→ `FInventoryItemData`의 아이콘을 `TSoftObjectPtr<UTexture2D>`로 쓰는 이유가 바로 이것

---

### 4. Undo 히스토리가 에셋을 붙잡는 이유

에디터의 Undo 스택도 `UObject` 참조를 들고 있음. 즉 **에디터 툴도 GC 관점에서는 일반 코드와 동일한 규칙**을 따름.

---

**핵심 요약**: UE 메모리/에셋 시스템의 근간인 `UObject + GC + 레퍼런스 추적` 구조를 이 다이얼로그 하나가 시각적으로 보여주는 것.