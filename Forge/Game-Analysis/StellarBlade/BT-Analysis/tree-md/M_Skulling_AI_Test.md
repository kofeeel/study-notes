# M_Skulling_AI_Test

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence
        │   ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        │   └── ❓ Selector
        │       ├── 🚶 MoveToTarget
        │       └── ⚔️ UseSkill(CombinationToHammer|CombinationToSword|CombinationToSpear|CombinationToGunner combo=TableCommand)
        └── ➡️ Sequence
            ├── 👁️ DetectTarget [🛡️DetectResult(==)]
            └── ❓ Selector [🛡️IsAlive(Target)]
                └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait)]
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(MeleeAttack combo=TableCommand)
                    │   └── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 5 |
| Task/DetectTarget | 3 |
| Dec/DetectResult | 3 |
| Sequence | 3 |
| Dec/IsAlive | 2 |
| Task/MoveToTarget | 2 |
| Task/UseSkill | 2 |
| Dec/IsGroupTarget | 1 |
| Dec/AggroLevel | 1 |
| Dec/CheckActorEffect | 1 |
| Task/Wait | 1 |
| Dec/Random | 1 |

## 스킬 목록
- UseSkill(CombinationToHammer|CombinationToSword|CombinationToSpear|CombinationToGunner combo=TableCommand)
- UseSkill(MeleeAttack combo=TableCommand)
