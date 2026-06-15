# M_ClriketA_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction)]
        │   ├── 🏠 MoveToHome [🛡️DetectResult(==)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️CheckStance(M_ClriketA_GroundStance) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ⚔️ UseSkill(GroundToNormal_1 combo=TableCommand)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(NOT M_ClriketA_GroundStance)]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
                ├── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                │   ├── ➡️ Sequence
                │   │   ├── 📋 Blackboard(Shoot)
                │   │   └── ⚔️ UseSkill(SpitAttack_1 combo=TableCommand)
                │   └── ➡️ Sequence
                │       ├── 📋 Blackboard(Shoot)
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ⚔️ UseSkill(SpitAttack_1 combo=TableCommand)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 11 |
| Dec/CheckActorEffect | 11 |
| Dec/AggroLevel | 6 |
| Sequence | 4 |
| Dec/IsAlive | 3 |
| Task/Blackboard | 3 |
| Task/UseSkill | 3 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/CheckStance | 2 |
| Task/CautionToTarget | 2 |
| Dec/TimeLimit | 2 |
| Dec/IsGroupTarget | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(GroundToNormal_1 combo=TableCommand)
- UseSkill(SpitAttack_1 combo=TableCommand)
- UseSkill(SpitAttack_1 combo=TableCommand)
