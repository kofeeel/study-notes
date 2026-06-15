# M_RoyalGuardFemale_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent ON)]
        │   │   ├── 📋 Blackboard(BattleStart)
        │   │   ├── ✨ UseEffect(['M_RoyalGuardFemale_FX_Check'])
        │   │   └── 🚶 MoveToTarget [🛡️TimeLimit(3.5s [StartTimer1])]
        │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
        │       ├── 📋 Blackboard(BattleStart)
        │       ├── ✨ UseEffect(['M_RoyalGuardFemale_FX_Check'])
        │       ├── ⚔️ UseSkill(BattleStart)
        │       ├── ⏳ WaitTimeRandom
        │       └── 🚶 MoveToTarget [🛡️TimeLimit(3.5s [StartTimer1])]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️IsActiveSkill(NOT)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RoyalGuardFemale_BattleEnd2 ON)]
            │   │   └── ➡️ Sequence
            │   │       ├── 🎬 PlayShow
            │   │       └── ✨ UseEffect(['M_RoyalGuardFemale_FX_Check'])
            │   └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RoyalGuardFemale_BattleEnd ON)]
            │       ├── 🎬 PlayShow
            │       └── ✨ UseEffect(['M_RoyalGuardFemale_FX_Check'])
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(NOT M_RoyalGuardFemale_Finish) && 🛡️CheckActorEffect(Self.M_RoyalGuardFemale_CheckAppearanceArea ON)]
            │   ├── ➡️ Sequence [🛡️Blackboard(MBS)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(MBS)
            │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_MaxHP<=50.0%) && 🛡️CheckStance(NOT M_RoyalGuardFemale_Phase2)]
            │   │   └── ⚔️ UseSkill(PhaseChange)
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RoyalGuardFemale_GroggyCheck ON)]
            │   │   └── ⚔️ UseSkill(MoveBackFar|MoveBackNear) [🛡️DistanceToTarget(dist<=500.0)]
            │   ├── ➡️ Sequence [🛡️Blackboard(SC) && 🛡️CheckStance(M_RoyalGuardFemale_Phase2)]
            │   │   ├── ⚔️ UseSkill(MoveBackFar|MoveBackNear) [🛡️DistanceToTarget(dist<=500.0)]
            │   │   └── 📋 Blackboard(SC)
            │   ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   │   ├── ⚔️ UseSkill(MoveBackFar|MoveBackNear) [🛡️DistanceToTarget(dist<=500.0)]
            │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [DownCautionTimer1])]
            │   ├── ❓ Selector
            │   │   ├── ➡️ Sequence
            │   │   │   ├── ⚔️ UseSkill(MoveLeft2|MoveRight2|MoveBackNear2)
            │   │   │   └── ⚔️ UseSkill(MoveBackFar|MoveBackNear) [🛡️CheckActorEffect(NOT Self.M_RoyalGuardFemale_CheckRun ON)]
            │   │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_RoyalGuardFemale_CheckRun ON)]
            │   │       ├── ➡️ Sequence
            │   │       │   ├── ⚔️ UseSkill(MoveBackFar) [🛡️UseableTime]
            │   │       │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=150.0)]
            │   │       └── ⚔️ UseSkill(MoveBackNear|MoveLeft|MoveRight) [🛡️UseableTime]
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(Recovery) [🛡️CheckActorStat(ActorStatType_MaxStamina<=99.0%)]
            │   │   └── ⚔️ UseSkill(Recovery) [🛡️CheckActorStat(ActorStatType_MaxShield<=50.0%)]
            │   ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=100.0)]
            │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_MaxStamina>=90.0%)]
            │   │   ├── ⚔️ UseSkill(SwingChain2 combo=TableCommand) [🛡️CheckActorEffect(Self.M_RoyalGuardFemale_Phase2 ON)]
            │   │   └── ⚔️ UseSkill(Combo combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_RoyalGuardFemale_Phase2 ON)]
            │   └── ➡️ Sequence
            │       ├── ✨ UseEffect(['M_RoyalGuardFemale_CheckRun'])
            │       └── ❓ Selector
            │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=150.0)]
            │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer3]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=500.0)]
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_RoyalGuardFemale_CheckAppearanceArea ON) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(NOT M_RoyalGuardFemale_Finish)]
                ├── ➡️ Sequence [🛡️Blackboard(MBS)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(MBS)
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RoyalGuardFemale_GroggyCheck ON)]
                │   └── ⚔️ UseSkill(MoveBackFar|MoveBackNear) [🛡️DistanceToTarget(dist<=500.0)]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
                │   ├── ⚔️ UseSkill(MoveBackFar|MoveBackNear) [🛡️DistanceToTarget(dist<=500.0)]
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [DownCautionTimer1])]
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(MoveLeft2|MoveRight2|MoveBackNear2)
                │   │   └── ⚔️ UseSkill(MoveBackFar|MoveBackNear) [🛡️CheckActorEffect(NOT Self.M_RoyalGuardFemale_CheckRun ON)]
                │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_RoyalGuardFemale_CheckRun ON)]
                │       ├── ➡️ Sequence
                │       │   ├── ⚔️ UseSkill(MoveBackFar) [🛡️UseableTime]
                │       │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=150.0)]
                │       └── ⚔️ UseSkill(MoveBackNear|MoveLeft|MoveRight) [🛡️UseableTime]
                ├── ❓ Selector
                │   ├── ⚔️ UseSkill(Recovery) [🛡️CheckActorStat(ActorStatType_MaxStamina<=99.0%)]
                │   └── ⚔️ UseSkill(Recovery) [🛡️CheckActorStat(ActorStatType_MaxShield<=50.0%)]
                ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=100.0)]
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_MaxStamina>=90.0%)]
                │   └── ⚔️ UseSkill(Combo combo=TableCommand)
                └── ➡️ Sequence
                    ├── ✨ UseEffect(['M_RoyalGuardFemale_CheckRun'])
                    └── ❓ Selector
                        ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=150.0)]
                        ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer3]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=500.0)]
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 22 |
| Selector | 21 |
| Dec/DistanceToTarget | 21 |
| Dec/CheckActorEffect | 17 |
| Sequence | 16 |
| Dec/TimeLimit | 12 |
| Task/CautionToTarget | 10 |
| Dec/CheckActorStat | 7 |
| Dec/Blackboard | 6 |
| Task/UseEffect | 6 |
| Task/UseableTimeReset | 6 |
| Dec/UseableTime | 6 |
| Task/Blackboard | 5 |
| Task/MoveToTarget | 4 |
| Dec/CheckStance | 4 |
| Dec/IsAlive | 3 |
| Dec/AggroLevel | 3 |
| Task/Wait | 2 |
| Task/PlayShow | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/WaitTimeRandom | 1 |
| Dec/IsActiveSkill | 1 |

## 스킬 목록
- UseSkill(BattleStart)
- UseSkill(PhaseChange)
- UseSkill(MoveBackFar|MoveBackNear)
- UseSkill(MoveBackFar|MoveBackNear)
- UseSkill(MoveBackFar|MoveBackNear)
- UseSkill(MoveLeft2|MoveRight2|MoveBackNear2)
- UseSkill(MoveBackFar|MoveBackNear)
- UseSkill(MoveBackFar)
- UseSkill(MoveBackNear|MoveLeft|MoveRight)
- UseSkill(Recovery)
- UseSkill(Recovery)
- UseSkill(SwingChain2 combo=TableCommand)
- UseSkill(Combo combo=TableCommand)
- UseSkill(MoveBackFar|MoveBackNear)
- UseSkill(MoveBackFar|MoveBackNear)
- UseSkill(MoveLeft2|MoveRight2|MoveBackNear2)
- UseSkill(MoveBackFar|MoveBackNear)
- UseSkill(MoveBackFar)
- UseSkill(MoveBackNear|MoveLeft|MoveRight)
- UseSkill(Recovery)
- UseSkill(Recovery)
- UseSkill(Combo combo=TableCommand)
