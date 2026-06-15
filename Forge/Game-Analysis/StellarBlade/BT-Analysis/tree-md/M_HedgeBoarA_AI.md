# M_HedgeBoarA_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ❓ Selector [🛡️Random(rand(100)<=20) && 🛡️DistanceToTarget(dist>=500.0)]
            │       │       │   ├── ⚔️ UseSkill(ChainAxeRush)
            │       │       │   └── ➡️ Sequence
            │       │       │       ├── 🚶 MoveToTarget
            │       │       │       └── ⚔️ UseSkill(ChainAxeRush)
            │       │       ├── ❓ Selector [🛡️Random(rand(100)<=20) && 🛡️DistanceToTarget(dist>=300.0)]
            │       │       │   ├── ⚔️ UseSkill(ChainAxeCombo)
            │       │       │   └── ➡️ Sequence
            │       │       │       ├── 🚶 MoveToTarget
            │       │       │       └── ⚔️ UseSkill(ChainAxeCombo)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(Smash|RushCombo)
            │       ├── ❓ Selector [🛡️UseableTime]
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(ChainAxeCombo) [🛡️DistanceToTarget(dist>=300.0)]
            │       │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=350.0)]
            │       │   ├── ⚔️ UseSkill(ChainAxeRush) [🛡️DistanceToTarget(dist>=300.0) && 🛡️CheckActorEffect(NOT Self.M_HedgeBoarA_NoChainAxeCheck ON)]
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(ChainAxeBackStep) [🛡️CheckActorEffect(NOT Self.M_HedgeBoarA_NoChainAxeCheck ON)]
            │       │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=350.0)]
            │       ├── ❓ Selector
            │       │   └── ⚔️ UseSkill(SpinCombo) [🛡️UseableTime]
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(Smash|RushCombo combo=TableCommand)
            │       │   └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ❓ Selector [🛡️Random(rand(100)<=20) && 🛡️DistanceToTarget(dist>=500.0)]
                │       │   ├── ⚔️ UseSkill(ChainAxeRush)
                │       │   └── ➡️ Sequence
                │       │       ├── 🚶 MoveToTarget
                │       │       └── ⚔️ UseSkill(ChainAxeRush)
                │       ├── ❓ Selector [🛡️Random(rand(100)<=20) && 🛡️DistanceToTarget(dist>=300.0)]
                │       │   ├── ⚔️ UseSkill(ChainAxeCombo)
                │       │   └── ➡️ Sequence
                │       │       ├── 🚶 MoveToTarget
                │       │       └── ⚔️ UseSkill(ChainAxeCombo)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(Smash|RushCombo)
                ├── ❓ Selector [🛡️UseableTime]
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(ChainAxeCombo) [🛡️DistanceToTarget(dist>=300.0)]
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=350.0)]
                │   ├── ⚔️ UseSkill(ChainAxeRush) [🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_HedgeBoarA_NoChainAxeCheck ON)]
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(ChainAxeBackStep) [🛡️CheckActorEffect(NOT Self.M_HedgeBoarA_NoChainAxeCheck ON)]
                │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=350.0)]
                ├── ❓ Selector
                │   └── ⚔️ UseSkill(SpinCombo) [🛡️UseableTime]
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(Smash|RushCombo combo=TableCommand)
                │   └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 23 |
| Sequence | 20 |
| Task/UseSkill | 20 |
| Dec/CheckActorEffect | 17 |
| Dec/DistanceToTarget | 12 |
| Task/MoveToTarget | 9 |
| Dec/TimeLimit | 8 |
| Dec/AggroLevel | 7 |
| Task/CautionToTarget | 7 |
| Task/UseableTimeReset | 7 |
| Dec/Blackboard | 6 |
| Task/Blackboard | 6 |
| Dec/Random | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Task/WaitTimeRandom | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(ChainAxeRush)
- UseSkill(ChainAxeRush)
- UseSkill(ChainAxeCombo)
- UseSkill(ChainAxeCombo)
- UseSkill(Smash|RushCombo)
- UseSkill(ChainAxeCombo)
- UseSkill(ChainAxeRush)
- UseSkill(ChainAxeBackStep)
- UseSkill(SpinCombo)
- UseSkill(Smash|RushCombo combo=TableCommand)
- UseSkill(ChainAxeRush)
- UseSkill(ChainAxeRush)
- UseSkill(ChainAxeCombo)
- UseSkill(ChainAxeCombo)
- UseSkill(Smash|RushCombo)
- UseSkill(ChainAxeCombo)
- UseSkill(ChainAxeRush)
- UseSkill(ChainAxeBackStep)
- UseSkill(SpinCombo)
- UseSkill(Smash|RushCombo combo=TableCommand)
