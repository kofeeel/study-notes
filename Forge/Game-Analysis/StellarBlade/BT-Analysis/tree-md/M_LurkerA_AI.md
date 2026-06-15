# M_LurkerA_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.M_LurkerA_BurrowStart ON) && 🛡️CheckStance(NOT M_LurkerA_Underground) && 🛡️IsActiveSkill(NOT)]
        │   └── ⚔️ UseSkill(BattleEndBurrow) [🛡️Blackboard(BattleStart)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.M_LurkerA_BurrowStart ON) && 🛡️CheckStance(NOT M_LurkerA_Underground) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   ├── 📋 Blackboard(BattleStart)
        │   └── ⚔️ UseSkill(BattleEndBurrow)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️CheckActorEffect(NOT Self.M_Lurker_GimmickCheck ON) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Lurker_GimmickCheck ON)]
        │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️CheckActorEffect(Self.M_Lurker_GimmickCheck ON)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Self.M_Lurker_GimmickCheck ON)]
            │   ├── ❓ Selector [🛡️CheckStance(M_LurkerA_Underground) && 🛡️Blackboard(BattleStart)]
            │   │   ├── ➡️ Sequence [🛡️Blackboard(BattleStart)]
            │   │   │   ├── ⚔️ UseSkill(SuddenlyAppear)
            │   │   │   ├── 📋 Blackboard(BattleStart)
            │   │   │   └── 📋 Blackboard(BattleStartSkill)
            │   │   └── 🚶 MoveToTarget
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckStance(NOT M_LurkerA_Underground)]
            │       └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.94s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(2.94s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(NOT Self.M_Lurker_GimmickCheck ON) && 🛡️CheckStance(NOT M_LurkerA_Underground)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=60) && 🛡️TimeLimit(2.94s [Timer_LinkWait]) && 🛡️DistanceToTarget(dist>250.0)]
            │       │       ├── ⚔️ UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
            │       │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0)]
            │       │       │   └── ⚔️ UseSkill(UndergroundRush1|UndergroundRush2)
            │       │       └── ❓ Selector [🛡️DistanceToTarget(dist>=250.0)]
            │       │           └── ⚔️ UseSkill(UndergroundRush2)
            │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorEffect(NOT Self.M_LurkerA_UnderGroundCoolTime ON)]
            │       │   └── ⚔️ UseSkill(UndergroundRush1|UndergroundRush2)
            │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=200.0) && 🛡️CheckActorEffect(NOT Self.M_LurkerA_NoGuardCheck ON)]
            │       │   └── ⚔️ UseSkill(UndergroundRush2)
            │       ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_LurkerA_NoGuardCheck ON)]
            │       │   └── ⚔️ UseSkill(Grab)
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
            │       │   └── ❓ Selector
            │       │       ├── ⚔️ UseSkill(StepCombo) [🛡️UseableTime]
            │       │       └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(NOT Self.M_Lurker_GimmickCheck ON) && 🛡️CheckStance(NOT M_LurkerA_Underground)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=60) && 🛡️TimeLimit(2.94s [Timer_BattleStartSkill]) && 🛡️DistanceToTarget(dist>250.0)]
                │       ├── ⚔️ UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
                │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0)]
                │       │   └── ⚔️ UseSkill(UndergroundRush1|UndergroundRush2)
                │       └── ❓ Selector [🛡️DistanceToTarget(dist>=250.0)]
                │           └── ⚔️ UseSkill(UndergroundRush2)
                ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorEffect(NOT Self.M_LurkerA_UnderGroundCoolTime ON)]
                │   └── ⚔️ UseSkill(UndergroundRush1|UndergroundRush2)
                ├── ❓ Selector [🛡️DistanceToTarget(dist>=250.0) && 🛡️CheckActorEffect(NOT Self.M_LurkerA_NoGuardCheck ON) && 🛡️CheckActorEffect(NOT Self.M_LurkerA_UnderGroundCoolTime ON)]
                │   └── ⚔️ UseSkill(UndergroundRush2)
                ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_LurkerA_NoGuardCheck ON)]
                │   └── ⚔️ UseSkill(Grab)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
                │   └── ❓ Selector
                │       ├── ⚔️ UseSkill(StepCombo) [🛡️UseableTime]
                │       └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 30 |
| Dec/CheckActorEffect | 29 |
| Task/UseSkill | 19 |
| Sequence | 14 |
| Dec/AggroLevel | 10 |
| Dec/Blackboard | 10 |
| Dec/DistanceToTarget | 10 |
| Task/Blackboard | 9 |
| Dec/CheckStance | 6 |
| Task/UseableTimeReset | 6 |
| Task/CautionToTarget | 5 |
| Dec/IsActiveSkill | 4 |
| Task/MoveToTarget | 4 |
| Dec/TimeLimit | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Task/PlayShow | 3 |
| Dec/DetectResult | 3 |
| Task/WaitTimeRandom | 3 |
| Task/MoveToHome | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/Random | 2 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(BattleEndBurrow)
- UseSkill(BattleEndBurrow)
- UseSkill(SuddenlyAppear)
- UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
- UseSkill(UndergroundRush1|UndergroundRush2)
- UseSkill(UndergroundRush2)
- UseSkill(UndergroundRush1|UndergroundRush2)
- UseSkill(UndergroundRush2)
- UseSkill(Grab)
- UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
- UseSkill(StepCombo)
- UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
- UseSkill(UndergroundRush1|UndergroundRush2)
- UseSkill(UndergroundRush2)
- UseSkill(UndergroundRush1|UndergroundRush2)
- UseSkill(UndergroundRush2)
- UseSkill(Grab)
- UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
- UseSkill(StepCombo)
