# N_DummyRavenBeast_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.Check_DummyA ON) && 🛡️CheckActorEffect(Self.Check_DummyB ON)]
        │   └── 🏠 MoveToHome
        └── ❓ Selector [🛡️IsAlive(Target) && 🛡️CheckActorEffect(Self.Check_DummyA ON) && 🛡️CheckActorEffect(NOT Self.Check_DummyB ON)]
            └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 4 |
| Selector | 3 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Sequence | 1 |
| Task/MoveToHome | 1 |
| Task/MoveToTarget | 1 |
