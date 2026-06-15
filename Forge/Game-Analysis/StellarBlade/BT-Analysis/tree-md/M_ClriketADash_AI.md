# M_ClriketADash_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle)]
        │   └── 📋 Blackboard(BB1) [🛡️Blackboard(BB1)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ❓ Selector
        │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                    │   ├── 🎬 PlayShow
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(BB1)
                    ├── ⚔️ UseSkill(SuicideRun_2 combo=TableCommand) [🛡️UseableTime]
                    ├── ⚔️ UseSkill(SuicideRun_1 combo=TableCommand)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 9 |
| Dec/CheckActorEffect | 7 |
| Dec/AggroLevel | 5 |
| Sequence | 4 |
| Dec/IsAlive | 3 |
| Task/Blackboard | 2 |
| Dec/Blackboard | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Task/CautionToTarget | 2 |
| Dec/TimeLimit | 2 |
| Task/UseSkill | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/WaitTimeRandom | 1 |
| Task/PlayShow | 1 |
| Task/UseableTimeReset | 1 |
| Dec/UseableTime | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(SuicideRun_2 combo=TableCommand)
- UseSkill(SuicideRun_1 combo=TableCommand)
