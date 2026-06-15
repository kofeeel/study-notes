# M_Marionette_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.8s [AbnormalTimer])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector
                │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_Marionette_Default)]
                │       └── ⚔️ UseSkill(SoundBombPhaseChange)
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Marionette_Phase2 ON) && 🛡️Blackboard(StartLaser) && 🛡️CheckActorStat(ActorStatType_HP>60.0%)]
                │   ├── ➡️ Sequence [🛡️CheckStance(M_Marionette_Default) && 🛡️Blackboard(StartLaser)]
                │   │   ├── ⚔️ UseSkill(Laser360Spin)
                │   │   └── 📋 Blackboard(StartLaser)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(SlashChain|DoubleScratchDown)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Marionette_Phase2 ON) && 🛡️Blackboard(StartLaser) && 🛡️CheckActorStat(ActorStatType_HP>60.0%)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB1)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckSummonedCount(count<?)]
                │   │   └── ⚔️ UseSkill(SummonDollHead)
                │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckSummonedCount(count<?) && 🛡️UseableTime]
                │   │   └── ⚔️ UseSkill(BackStep combo=TableCommand)
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_SummonChainCheck ON)]
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.8s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=400.0)]
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_SummonChainCheck ON)]
                │   │   └── ⚔️ UseSkill(SlashChain)
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Marionette_SummonChainCheck ON)]
                │   │   └── ⚔️ UseSkill(FaceLaser)
                │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Marionette_SummonChainCheck ON) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️UseableTime]
                │   │   ├── ⚔️ UseSkill(JumpAttack) [🛡️CheckActorEffect(NOT Self.M_Marionette_NoGuardCheck ON)]
                │   │   └── ⚔️ UseSkill(ScratchRush)
                │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Marionette_SummonChainCheck ON)]
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.8s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(1Phase Laser) && 🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                │   │   ├── ⚔️ UseSkill(Laser360Spin)
                │   │   └── 📋 Blackboard(1Phase Laser)
                │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Marionette_NoGuardCheck ON)]
                │   │   └── ⚔️ UseSkill(Grab)
                │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(Swing) [🛡️CheckActorStat(ActorStatType_HP<=95.0%)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Marionette_SummonChainCheck ON)]
                │   │   ├── ⚔️ UseSkill(SlashChain|DoubleScratchDown|OffbeatSlash)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Marionette_SummonChainCheck ON)]
                │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=200.0)]
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_Phase2 ON) && 🛡️CheckActorStat(ActorStatType_HP<=60.0%)]
                    ├── ➡️ Sequence [🛡️Blackboard(BB2)]
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(BB2)
                    ├── ⚔️ UseSkill(LaserRampage) [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                    ├── ➡️ Sequence [🛡️CheckSummonedCount(count>=?) && 🛡️CheckActorEffect(Self.M_Marionette_LasertoBombSignal ON)]
                    │   ├── ⚔️ UseSkill(SoundBomb)
                    │   └── ✨ UseEffect(['M_Marionette_LasertoBombSignal_Dispel'])
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_LasertoBombSignal ON) && 🛡️CheckSummonedCount(count<?)]
                    │   └── ✨ UseEffect(['M_Marionette_LasertoBombSignal_Dispel'])
                    ├── ❓ Selector [🛡️CheckSummonedCount(count<?) && 🛡️UseableTime && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(SummonDollHead|SummonDollHead2)
                    ├── ❓ Selector [🛡️CheckSummonedCount(count<?) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(BackStep2)
                    ├── ❓ Selector [🛡️CheckSummonedCount(count<?) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(BackStep)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_BackStepChain ON) && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(SummonDollHead|SummonDollHead2)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(Self.M_Marionette_DollHeadBombCheck ON) && 🛡️CheckActorEffect(NOT Self.M_Marionette_SoundBombChain1 ON)]
                    │   └── ⚔️ UseSkill(Laser360Spin combo=TableCommand)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(Self.M_Marionette_DollHeadBombCheck ON) && 🛡️CheckActorEffect(NOT Self.M_Marionette_SoundBombChain1 ON) && 🛡️DistanceToTarget(dist<=300.0)]
                    │   └── ⚔️ UseSkill(SoundBomb)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.8s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=400.0)]
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
                    │   └── ⚔️ UseSkill(FaceLaser)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
                    │   └── ⚔️ UseSkill(SlashChain)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_DollHeadBombCheck ON)]
                    │   └── 🚶 MoveToTarget
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_DollHeadLaserCheck ON) && 🛡️CheckActorEffect(NOT Self.M_Marionette_SoundBombChain2 ON)]
                    │   └── ⚔️ UseSkill(JumpAttack combo=TableCommand)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_DollHeadLaserCheck ON) && 🛡️CheckActorEffect(NOT Self.M_Marionette_SoundBombChain2 ON) && 🛡️DistanceToTarget(dist<=300.0)]
                    │   └── ⚔️ UseSkill(SoundBomb2)
                    ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorEffect(Self.? ON)]
                    │   └── ⚔️ UseSkill(ScratchRush)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
                    │   └── ⚔️ UseSkill(SlashChain)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_DollHeadLaserCheck ON)]
                    │   └── 🚶 MoveToTarget
                    ├── ➡️ Sequence [🛡️CheckSummonedCount(count>=?) && 🛡️CheckActorEffect(Self.M_Marionette_LasertoBombSignal ON)]
                    │   ├── ⚔️ UseSkill(SoundBomb)
                    │   └── ✨ UseEffect(['M_Marionette_LasertoBombSignal_Dispel'])
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Marionette_LasertoBombSignal ON) && 🛡️CheckSummonedCount(count<?)]
                    │   └── ✨ UseEffect(['M_Marionette_LasertoBombSignal_Dispel'])
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️UseableTime]
                    │   ├── ⚔️ UseSkill(JumpAttack) [🛡️CheckActorEffect(NOT Self.M_Marionette_NoGuardCheck2 ON)]
                    │   └── ⚔️ UseSkill(ScratchRush)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.8s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorStat(ActorStatType_HP<=55.0%)]
                    │   └── ⚔️ UseSkill(ScratchLaserCombo)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(Grab)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(Swing)
                    ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                    │   └── ⚔️ UseSkill(Laser360Spin)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.8s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   ├── ⚔️ UseSkill(SlashChain|DoubleScratchDown|OffbeatSlash)
                    │   └── ⏳ WaitTimeRandom
                    └── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                        ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=200.0)]
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 53 |
| Selector | 43 |
| Task/UseSkill | 35 |
| Dec/DistanceToTarget | 19 |
| Sequence | 13 |
| Dec/UseableTime | 10 |
| Dec/CheckActorStat | 9 |
| Dec/CheckSummonedCount | 9 |
| Task/CautionToTarget | 8 |
| Dec/TimeLimit | 8 |
| Dec/Blackboard | 6 |
| Task/MoveToTarget | 5 |
| Task/UseableTimeReset | 5 |
| Task/Blackboard | 4 |
| Task/UseEffect | 4 |
| Dec/IsAlive | 3 |
| Task/WaitTimeRandom | 3 |
| Task/Wait | 2 |
| Dec/CheckStance | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/AggroLevel | 1 |

## 스킬 목록
- UseSkill(SoundBombPhaseChange)
- UseSkill(Laser360Spin)
- UseSkill(SlashChain|DoubleScratchDown)
- UseSkill(SummonDollHead)
- UseSkill(BackStep combo=TableCommand)
- UseSkill(SlashChain)
- UseSkill(FaceLaser)
- UseSkill(JumpAttack)
- UseSkill(ScratchRush)
- UseSkill(Laser360Spin)
- UseSkill(Grab)
- UseSkill(Swing)
- UseSkill(SlashChain|DoubleScratchDown|OffbeatSlash)
- UseSkill(LaserRampage)
- UseSkill(SoundBomb)
- UseSkill(SummonDollHead|SummonDollHead2)
- UseSkill(BackStep2)
- UseSkill(BackStep)
- UseSkill(SummonDollHead|SummonDollHead2)
- UseSkill(Laser360Spin combo=TableCommand)
- UseSkill(SoundBomb)
- UseSkill(FaceLaser)
- UseSkill(SlashChain)
- UseSkill(JumpAttack combo=TableCommand)
- UseSkill(SoundBomb2)
- UseSkill(ScratchRush)
- UseSkill(SlashChain)
- UseSkill(SoundBomb)
- UseSkill(JumpAttack)
- UseSkill(ScratchRush)
- UseSkill(ScratchLaserCombo)
- UseSkill(Grab)
- UseSkill(Swing)
- UseSkill(Laser360Spin)
- UseSkill(SlashChain|DoubleScratchDown|OffbeatSlash)
