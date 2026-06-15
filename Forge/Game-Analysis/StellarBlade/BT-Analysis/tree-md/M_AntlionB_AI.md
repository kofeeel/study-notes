# M_AntlionB_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ❓ Selector
        │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Antlion_MonsterSpawnEvent) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
        │   │   ├── ⏳ WaitTimeRandom [🛡️DetectResult(==)]
        │   │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_SpawnEvent) && 🛡️Blackboard(BattleState)]
            │       ├── 📋 Blackboard(BattleState)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(SuicideRun combo=TableCommand)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 10 |
| Selector | 9 |
| Sequence | 6 |
| Dec/AggroLevel | 5 |
| Dec/DetectResult | 4 |
| Dec/IsAlive | 3 |
| Task/WaitTimeRandom | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Task/DetectTarget | 2 |
| Task/Wait | 2 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/CautionToTarget | 1 |
| Dec/TimeLimit | 1 |
| Task/UseSkill | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(SuicideRun combo=TableCommand)
