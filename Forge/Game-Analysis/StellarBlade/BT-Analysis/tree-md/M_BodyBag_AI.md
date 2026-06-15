# M_BodyBag_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BattleState) && 🛡️CheckActorEffect(Self.M_BodyBag_SummonEffectDispel ON)]
            │   │   ├── 📋 Blackboard(BattleState)
            │   │   └── ⏳ WaitTimeRandom
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_BodyBag_SummonEffectDispel)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(SuicideBomb)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 10 |
| Selector | 8 |
| Sequence | 6 |
| Dec/AggroLevel | 5 |
| Task/WaitTimeRandom | 4 |
| Dec/IsAlive | 3 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/UseSkill | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(SuicideBomb)
