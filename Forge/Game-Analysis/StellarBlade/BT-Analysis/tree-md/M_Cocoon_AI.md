# M_Cocoon_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⏳ WaitTimeRandom [🛡️TimeLimit(3.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⏳ WaitTimeRandom [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s [Timer_LinkWait])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                └── ⚔️ UseSkill(ThornAttack)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 7 |
| Dec/CheckActorEffect | 6 |
| Dec/IsAlive | 3 |
| Task/WaitTimeRandom | 3 |
| Task/Wait | 2 |
| Dec/TimeLimit | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/AggroLevel | 1 |
| Task/UseSkill | 1 |

## 스킬 목록
- UseSkill(ThornAttack)
