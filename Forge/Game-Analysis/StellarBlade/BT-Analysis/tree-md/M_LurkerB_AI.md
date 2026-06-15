# M_LurkerB_AI

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
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckStance(NOT M_LurkerB_Underground)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=20) && 🛡️TimeLimit(2.94s [Timer_LinkWait]) && 🛡️DistanceToTarget(dist>250.0)]
            │       │       ├── ⚔️ UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
            │       │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=70)]
            │       │       │   └── ⚔️ UseSkill(ShotThrow)
            │       │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0)]
            │       │       │   └── ⚔️ UseSkill(UndergroundRush1|UndergroundRush2)
            │       │       └── ❓ Selector [🛡️DistanceToTarget(dist>=250.0)]
            │       │           └── ⚔️ UseSkill(UndergroundRush2)
            │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0)]
            │       │   └── ⚔️ UseSkill(ShotThrow)
            │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorEffect(NOT Self.M_LurkerB_UnderGroundCoolTime ON)]
            │       │   └── ⚔️ UseSkill(UndergroundRush1|UndergroundRush2)
            │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=250.0) && 🛡️CheckActorEffect(NOT Self.M_LurkerB_NoGuardCheck ON) && 🛡️CheckActorEffect(NOT Self.M_LurkerB_UnderGroundCoolTime ON)]
            │       │   └── ⚔️ UseSkill(UndergroundRush2)
            │       ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_LurkerB_NoGuardCheck ON)]
            │       │   └── ⚔️ UseSkill(Vomit)
            │       ├── ❓ Selector [🛡️UseableTime]
            │       │   └── ⚔️ UseSkill(UndergroundBackStep)
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
            │       │   └── ❓ Selector
            │       │       ├── ⚔️ UseSkill(StepCombo) [🛡️UseableTime]
            │       │       └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckStance(NOT M_LurkerB_Underground)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=20) && 🛡️TimeLimit(2.94s [Timer_BattleStartSkill]) && 🛡️DistanceToTarget(dist>250.0)]
                │       ├── ⚔️ UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
                │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=70)]
                │       │   └── ⚔️ UseSkill(ShotThrow)
                │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0)]
                │       │   └── ⚔️ UseSkill(UndergroundRush1|UndergroundRush2)
                │       └── ❓ Selector [🛡️DistanceToTarget(dist>=250.0)]
                │           └── ⚔️ UseSkill(UndergroundRush2)
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=300.0)]
                │   ├── ⚔️ UseSkill(ShotThrow)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1])]
                ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorEffect(NOT Self.M_LurkerB_UnderGroundCoolTime ON)]
                │   └── ⚔️ UseSkill(UndergroundRush1|UndergroundRush2)
                ├── ❓ Selector [🛡️DistanceToTarget(dist>=250.0) && 🛡️CheckActorEffect(NOT Self.M_LurkerB_NoGuardCheck ON) && 🛡️CheckActorEffect(NOT Self.M_LurkerB_UnderGroundCoolTime ON)]
                │   └── ⚔️ UseSkill(UndergroundRush2)
                ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_LurkerB_NoGuardCheck ON)]
                │   └── ⚔️ UseSkill(Vomit)
                ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_LurkerB_UnderGroundCoolTime ON)]
                │   ├── ⚔️ UseSkill(UndergroundBackStep)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️DistanceToTarget(dist<2000.0)]
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
| Selector | 35 |
| Task/UseSkill | 25 |
| Dec/CheckActorEffect | 24 |
| Dec/DistanceToTarget | 16 |
| Sequence | 14 |
| Dec/Blackboard | 10 |
| Dec/AggroLevel | 9 |
| Task/Blackboard | 9 |
| Task/UseableTimeReset | 8 |
| Task/CautionToTarget | 7 |
| Dec/CheckStance | 6 |
| Dec/TimeLimit | 6 |
| Dec/UseableTime | 6 |
| Dec/IsActiveSkill | 4 |
| Task/MoveToTarget | 4 |
| Dec/Random | 4 |
| Dec/IsAlive | 3 |
| Task/PlayShow | 3 |
| Task/WaitTimeRandom | 3 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(BattleEndBurrow)
- UseSkill(BattleEndBurrow)
- UseSkill(SuddenlyAppear)
- UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
- UseSkill(ShotThrow)
- UseSkill(UndergroundRush1|UndergroundRush2)
- UseSkill(UndergroundRush2)
- UseSkill(ShotThrow)
- UseSkill(UndergroundRush1|UndergroundRush2)
- UseSkill(UndergroundRush2)
- UseSkill(Vomit)
- UseSkill(UndergroundBackStep)
- UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
- UseSkill(StepCombo)
- UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
- UseSkill(ShotThrow)
- UseSkill(UndergroundRush1|UndergroundRush2)
- UseSkill(UndergroundRush2)
- UseSkill(ShotThrow)
- UseSkill(UndergroundRush1|UndergroundRush2)
- UseSkill(UndergroundRush2)
- UseSkill(Vomit)
- UseSkill(UndergroundBackStep)
- UseSkill(ScratchCombo|SpinCombo combo=TableCommand)
- UseSkill(StepCombo)
