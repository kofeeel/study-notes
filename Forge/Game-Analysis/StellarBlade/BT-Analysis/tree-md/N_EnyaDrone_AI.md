# N_EnyaDrone_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ➡️ Sequence [🛡️IsAlive(Target)]
            ├── ⚔️ UseSkill(Heal combo=TableCommand) [🛡️CheckActorEffect(Target.N_Enya_Check ON)]
            └── ⏳ WaitTimeRandom
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 2 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Sequence | 1 |
| Task/UseSkill | 1 |
| Dec/CheckActorEffect | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(Heal combo=TableCommand)
