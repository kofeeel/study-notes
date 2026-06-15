# M_ClriketA_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector
        │   └── 🏠 MoveToHome [🛡️CheckActorEffect(Self.M_Common_Nikke_PathWay ON)]
        └── ❓ Selector [🛡️IsAlive(Target) && 🛡️CheckActorEffect(Self.M_Common_Nikke_PathWay)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️CheckStance(M_ClriketA_GroundStance) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ⚔️ UseSkill(GroundToNormal_1 combo=TableCommand)
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ➡️ Sequence
                        ├── ⏳ WaitTimeRandom
                        └── ⚔️ UseSkill(Nikke_SpitAttack combo=TableCommand subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 7 |
| Dec/CheckActorEffect | 5 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Dec/AggroLevel | 2 |
| Task/UseSkill | 2 |
| Sequence | 2 |
| Task/MoveToHome | 1 |
| Dec/CheckStance | 1 |
| Dec/IsGroupTarget | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(GroundToNormal_1 combo=TableCommand)
- UseSkill(Nikke_SpitAttack combo=TableCommand subTarget)
