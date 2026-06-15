# M_LurkerB_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.M_LurkerB_BurrowStart ON) && 🛡️CheckStance(NOT M_LurkerB_Underground) && 🛡️IsActiveSkill(NOT)]
        │   └── ⚔️ UseSkill(BattleEndBurrow) [🛡️Blackboard(BattleStart)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.M_LurkerB_BurrowStart ON) && 🛡️CheckStance(NOT M_LurkerB_Underground) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   ├── 📋 Blackboard(BattleStart)
        │   └── ⚔️ UseSkill(BattleEndBurrow)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ❓ Selector
        │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckStance(M_LurkerB_Underground)]
            │   │   ├── ➡️ Sequence [🛡️Blackboard(BattleStart)]
            │   │   │   ├── ⚔️ UseSkill(SuddenlyAppear)
            │   │   │   ├── 📋 Blackboard(BattleStart)
            │   │   │   └── 📋 Blackboard(BattleStartSkill)
            │   │   └── 🚶 MoveToTarget
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckStance(NOT M_LurkerB_Underground)]
            │       └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckStance(NOT M_LurkerB_Underground)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    ├── ⚔️ UseSkill(Nikke_ShotThrow subTarget)
                    └── ➡️ Sequence
                        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                        └── ⚔️ UseSkill(Nikke_UndergroundMoveLeft|Nikke_UndergroundMoveRight subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 10 |
| Selector | 9 |
| Dec/AggroLevel | 7 |
| Dec/CheckActorEffect | 7 |
| Dec/Blackboard | 7 |
| Task/Blackboard | 6 |
| Dec/CheckStance | 5 |
| Task/UseSkill | 5 |
| Dec/IsActiveSkill | 4 |
| Dec/DetectResult | 4 |
| Dec/IsAlive | 3 |
| Task/PlayShow | 3 |
| Task/DetectTarget | 3 |
| Task/UseableTimeReset | 2 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Task/Wait | 1 |
| Task/MoveToTarget | 1 |
| Dec/CheckActorState | 1 |
| Dec/IsGroupTarget | 1 |

## 스킬 목록
- UseSkill(BattleEndBurrow)
- UseSkill(BattleEndBurrow)
- UseSkill(SuddenlyAppear)
- UseSkill(Nikke_ShotThrow subTarget)
- UseSkill(Nikke_UndergroundMoveLeft|Nikke_UndergroundMoveRight subTarget)
