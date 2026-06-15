# M_ExoSuit_AI

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
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.6s [AbnormalTimer])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector
                │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckStance(M_ExoSuit_Default)]
                │       └── ⚔️ UseSkill(PhaseChange)
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP>70.0%) && 🛡️CheckStance(M_ExoSuit_Default) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoNerfAI ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot) && 🛡️DistanceToTarget(dist>400.0)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON)]
                │   │   ├── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️DistanceToTarget(dist>400.0) && 🛡️Random(rand(100)<=30)]
                │   │   ├── ⚔️ UseSkill(BackStepRushCounter combo=TableCommand)
                │   │   ├── ⚔️ UseSkill(BladeChaseCombo|ChaseCombo combo=TableCommand) [🛡️DistanceToTarget(dist>400.0)]
                │   │   ├── ⚔️ UseSkill(EnergyBombShot|LaserCrossFire combo=TableCommand) [🛡️DistanceToTarget(dist>800.0)]
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️DistanceToTarget(dist>400.0) && 🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(DashShockwave)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergySphereRush) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(BackMove|BackMoveCombo) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️Blackboard(DistanceIncrease)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount1)]
                │   │   │   ├── ⚔️ UseSkill(LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=1000.0)]
                │   │   │   └── 📋 Blackboard(ShotCount1)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount1)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot1|EnergyMoveShot1|EnergyBombShot|LaserCrossFire) [🛡️DistanceToTarget(dist>=1000.0)]
                │   │       └── 📋 Blackboard(ShotCount1)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️Blackboard(DistanceIncrease)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount2)]
                │   │   │   ├── ⚔️ UseSkill(LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=1000.0)]
                │   │   │   └── 📋 Blackboard(ShotCount2)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount2)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot1|EnergyMoveShot1|EnergyBombShot|LaserCrossFire) [🛡️DistanceToTarget(dist>=1000.0)]
                │   │       └── 📋 Blackboard(ShotCount2)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   │   ├── ⚔️ UseSkill(BladeChaseCombo) [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence [🛡️UseableTime]
                │   │       ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   ├── ⚔️ UseSkill(BladeCombo|Grab) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_ChanceGroupCheck ON)]
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   │   ├── ⚔️ UseSkill(BackStepRush) [🛡️Blackboard(DistanceIncrease) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardCheck ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence [🛡️UseableTime]
                │   │       ├── ⚔️ UseSkill(BackStepCombo1|BackStepCombo2) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardGroupCheck ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   ├── ⚔️ UseSkill(SmashCombo)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   ├── 📋 Blackboard(DistanceIncrease)
                │   │   └── ⚔️ UseSkill(BackStepRush) [🛡️UseableTime]
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(Swing|SwingChain)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   ├── 📋 Blackboard(DistanceIncrease)
                │   │   └── ⚔️ UseSkill(BackStepRush) [🛡️UseableTime]
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP>70.0%) && 🛡️CheckStance(M_ExoSuit_Default) && 🛡️CheckActorEffect(Self.M_ExoSuit_NoNerfAI ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON)]
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(DashShockwave)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergySphereRush) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(BackMoveCombo|BackMove) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️Blackboard(DistanceIncrease)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount1)]
                │   │   │   ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount1)]
                │   │   │   ├── ⚔️ UseSkill(LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=800.0)]
                │   │   │   └── 📋 Blackboard(ShotCount1)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount1)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot1|EnergyMoveShot1|EnergyMoveShot3|EnergyBombShot|LaserCrossFire) [🛡️DistanceToTarget(dist>=800.0)]
                │   │       └── 📋 Blackboard(ShotCount1)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️Blackboard(DistanceIncrease)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount2)]
                │   │   │   ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount2)]
                │   │   │   ├── ⚔️ UseSkill(LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=800.0)]
                │   │   │   └── 📋 Blackboard(ShotCount2)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount2)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot1|EnergyMoveShot1|EnergyMoveShot3|EnergyBombShot|LaserCrossFire) [🛡️DistanceToTarget(dist>=800.0)]
                │   │       └── 📋 Blackboard(ShotCount2)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   │   ├── ⚔️ UseSkill(BladeChaseCombo) [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence [🛡️UseableTime]
                │   │       ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   ├── ⚔️ UseSkill(BladeCombo|Grab) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_ChanceGroupCheck ON)]
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   │   ├── ⚔️ UseSkill(BackStepRush) [🛡️Blackboard(DistanceIncrease) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardCheck ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence [🛡️UseableTime]
                │   │       ├── ⚔️ UseSkill(BackStepCombo1|BackStepCombo2|MoveCombo) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardGroupCheck ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   ├── ⚔️ UseSkill(SmashCombo)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   ├── 📋 Blackboard(DistanceIncrease)
                │   │   └── ⚔️ UseSkill(BackStepRush) [🛡️UseableTime]
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(Swing|SwingChain)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   ├── 📋 Blackboard(DistanceIncrease)
                │   │   └── ⚔️ UseSkill(BackStepRush) [🛡️UseableTime]
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorStat(ActorStatType_HP>30.0%) && 🛡️CheckStance(M_ExoSuit_Phase2) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoNerfAI ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB2)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB2)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON)]
                │   │   ├── ⚔️ UseSkill(EvadeLeft|EvadeRight|LaserChargeShot combo=TableCommand) [🛡️DistanceToTarget(dist>400.0) && 🛡️Random(rand(100)<=30)]
                │   │   ├── ⚔️ UseSkill(BackStepRushCounter combo=TableCommand)
                │   │   ├── ⚔️ UseSkill(BladeChaseCombo|ChaseCombo combo=TableCommand) [🛡️DistanceToTarget(dist>400.0)]
                │   │   ├── ⚔️ UseSkill(EnergyBombShot|LaserCrossFire|LaserChargeShot combo=TableCommand) [🛡️DistanceToTarget(dist>800.0)]
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️DistanceToTarget(dist>400.0) && 🛡️Random(rand(100)<=50)]
                │   ├── ❓ Selector [🛡️AimMe && 🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                │   │   ├── ⚔️ UseSkill(EvadeLeft|EvadeRight|LaserChargeShot combo=TableCommand) [🛡️DistanceToTarget(dist>400.0) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(BackStepRushCounter combo=TableCommand)
                │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                │   │   ├── ⚔️ UseSkill(AreaExplosion)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(FullBurst) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_SpecialGroupCheck ON) && 🛡️DistanceToTarget(dist>=1000.0)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(DashShockwave)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergySphereRush) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(BackMoveCombo|BackMoveMissile) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️Blackboard(DistanceIncrease)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(FirstLaser)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot) [🛡️DistanceToTarget(dist>=1000.0)]
                │   │   │   ├── 📋 Blackboard(FirstLaser)
                │   │   │   └── 📋 Blackboard(ShotCount1)
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount1) && 🛡️Blackboard(FirstLaser)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot|LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=1000.0)]
                │   │   │   └── 📋 Blackboard(ShotCount1)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount1) && 🛡️Blackboard(FirstLaser)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot) [🛡️DistanceToTarget(dist>=1000.0)]
                │   │       └── 📋 Blackboard(ShotCount1)
                │   ├── ❓ Selector [🛡️Blackboard(DistanceIncrease)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(FirstLaser)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot) [🛡️DistanceToTarget(dist>=1000.0)]
                │   │   │   ├── 📋 Blackboard(FirstLaser)
                │   │   │   └── 📋 Blackboard(ShotCount2)
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount2) && 🛡️Blackboard(FirstLaser)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot|LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=1000.0)]
                │   │   │   └── 📋 Blackboard(ShotCount2)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount2) && 🛡️Blackboard(FirstLaser)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot) [🛡️DistanceToTarget(dist>=1000.0)]
                │   │       └── 📋 Blackboard(ShotCount2)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(BladeChaseCombo) [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardCheck ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️Blackboard(SecondTime)]
                │   │   ├── ⚔️ UseSkill(BladeGrabCombo) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_ChanceGroupCheck ON)]
                │   │   ├── 📋 Blackboard(SecondTime)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️Blackboard(SecondTime)]
                │   │   ├── ⚔️ UseSkill(BladeGrabCombo|Grab) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_ChanceGroupCheck ON)]
                │   │   ├── 📋 Blackboard(ShotCount)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️Blackboard(SecondTime2)]
                │   │   ├── ⚔️ UseSkill(EnergyShotCombo)
                │   │   ├── 📋 Blackboard(SecondTime2)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️Blackboard(SecondTime2)]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergyExplosion|BackStepRush) [🛡️Blackboard(DistanceIncrease) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(BackStepCombo2|BackStepCombo1|EnergyShotCombo|MoveCombo) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardGroupCheck ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(Swing|SwingChain)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   ├── 📋 Blackboard(DistanceIncrease)
                │   │   └── ⚔️ UseSkill(BackStepRush)
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorStat(ActorStatType_HP>30.0%) && 🛡️CheckStance(M_ExoSuit_Phase2) && 🛡️CheckActorEffect(Self.M_ExoSuit_NoNerfAI ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB2)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB2)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON)]
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
                │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                │   │   ├── ⚔️ UseSkill(FullBurst) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_SpecialGroupCheck ON)]
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(ShockWaveCombo) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(ShockWaveCombo2) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(DashShockwave)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergySphereRush) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(BackMoveCombo|BackMoveMissile) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️Blackboard(DistanceIncrease)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount1)]
                │   │   │   ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   ├── ➡️ Sequence [🛡️Blackboard(FirstLaser)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot) [🛡️DistanceToTarget(dist>=800.0)]
                │   │   │   ├── 📋 Blackboard(FirstLaser)
                │   │   │   └── 📋 Blackboard(ShotCount1)
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount1) && 🛡️Blackboard(FirstLaser)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot|LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=800.0)]
                │   │   │   └── 📋 Blackboard(ShotCount1)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount1) && 🛡️Blackboard(FirstLaser)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot) [🛡️DistanceToTarget(dist>=800.0)]
                │   │       └── 📋 Blackboard(ShotCount1)
                │   ├── ❓ Selector [🛡️Blackboard(DistanceIncrease)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount2)]
                │   │   │   ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   ├── ➡️ Sequence [🛡️Blackboard(FirstLaser)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot) [🛡️DistanceToTarget(dist>=800.0)]
                │   │   │   ├── 📋 Blackboard(FirstLaser)
                │   │   │   └── 📋 Blackboard(ShotCount2)
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount2) && 🛡️Blackboard(FirstLaser)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot|LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=800.0)]
                │   │   │   └── 📋 Blackboard(ShotCount2)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount2) && 🛡️Blackboard(FirstLaser)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot) [🛡️DistanceToTarget(dist>=800.0)]
                │   │       └── 📋 Blackboard(ShotCount2)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(MissileChaseCombo) [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_MissileCheck ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(BladeChaseCombo) [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardCheck ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️Blackboard(SecondTime)]
                │   │   ├── ⚔️ UseSkill(BladeGrabCombo) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_ChanceGroupCheck ON)]
                │   │   ├── 📋 Blackboard(SecondTime)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️Blackboard(SecondTime)]
                │   │   ├── ⚔️ UseSkill(BladeGrabCombo|Grab) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_ChanceGroupCheck ON)]
                │   │   ├── 📋 Blackboard(ShotCount)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence [🛡️Blackboard(SecondTime2)]
                │   │   ├── ⚔️ UseSkill(SwingChainCombo|RapidCombo) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardGroupCheck ON)]
                │   │   ├── 📋 Blackboard(SecondTime2)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   └── 📋 Blackboard(DistanceIncrease)
                │   ├── ❓ Selector [🛡️Blackboard(SecondTime2)]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergyExplosion|BackStepRush) [🛡️Blackboard(DistanceIncrease) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergyShotCombo|SwingChainCombo|RapidCombo) [🛡️Random(rand(100)<=30) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardGroupCheck ON)]
                │   │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   │   └── 📋 Blackboard(DistanceIncrease)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(BackStepCombo2|BackStepCombo1|EnergyShotCombo|SwingChainCombo|RapidCombo|MoveCombo) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardGroupCheck ON)]
                │   │       ├── 📋 Blackboard(ShotCount1)
                │   │       ├── 📋 Blackboard(ShotCount2)
                │   │       └── 📋 Blackboard(DistanceIncrease)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(Swing|SwingChain)
                │   │   ├── 📋 Blackboard(ShotCount1)
                │   │   ├── 📋 Blackboard(ShotCount2)
                │   │   ├── 📋 Blackboard(DistanceIncrease)
                │   │   └── ⚔️ UseSkill(BackStepRush)
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=30.0%) && 🛡️CheckStance(M_ExoSuit_Phase3) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoNerfAI ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB3)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB3)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(DashShockwave)
                │   │   └── 📋 Blackboard(ShotCount)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergySphereRush) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   └── 📋 Blackboard(ShotCount)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(BackMoveCombo|BackMoveMissile) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │       └── 📋 Blackboard(ShotCount)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount)]
                │   │   │   ├── ⚔️ UseSkill(LaserChargeShot|LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=1000.0)]
                │   │   │   └── 📋 Blackboard(ShotCount)
                │   │   └── ➡️ Sequence [🛡️Blackboard(ShotCount)]
                │   │       ├── ⚔️ UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot) [🛡️DistanceToTarget(dist>=1000.0)]
                │   │       └── 📋 Blackboard(ShotCount)
                │   ├── ❓ Selector
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                │   │       └── 📋 Blackboard(ShotCount)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(EnergyExplosion) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   │   └── 📋 Blackboard(ShotCount)
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(BackStepCombo1|BackStepCombo2|MoveCombo) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardGroupCheck ON)]
                │   │       └── 📋 Blackboard(ShotCount)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(SmashCombo)
                │   │   └── 📋 Blackboard(ShotCount)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(Swing|SwingChain)
                │   │   └── 📋 Blackboard(ShotCount)
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=30.0%) && 🛡️CheckStance(M_ExoSuit_Phase3) && 🛡️CheckActorEffect(Self.M_ExoSuit_NoNerfAI ON)]
                    ├── ➡️ Sequence [🛡️Blackboard(BB3)]
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(BB3)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                    │   ├── 📋 Blackboard(FirstShot)
                    │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON)]
                    │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                    │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(DashShockwave)
                    │   └── 📋 Blackboard(ShotCount)
                    ├── ❓ Selector [🛡️UseableTime]
                    │   ├── ➡️ Sequence
                    │   │   ├── ⚔️ UseSkill(EnergySphereRush) [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   │   └── 📋 Blackboard(ShotCount)
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(BackMoveCombo|BackMoveMissile) [🛡️CheckActorEffect(NOT Self.? ON)]
                    │       └── 📋 Blackboard(ShotCount)
                    ├── ❓ Selector
                    │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount)]
                    │   │   ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                    │   │   └── 📋 Blackboard(ShotCount)
                    │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount)]
                    │   │   ├── ⚔️ UseSkill(LaserChargeShot|LaserCrossFire) [🛡️Random(rand(100)<=30) && 🛡️DistanceToTarget(dist>=800.0)]
                    │   │   └── 📋 Blackboard(ShotCount)
                    │   └── ➡️ Sequence [🛡️Blackboard(ShotCount)]
                    │       ├── ⚔️ UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot) [🛡️DistanceToTarget(dist>=800.0)]
                    │       └── 📋 Blackboard(ShotCount)
                    ├── ❓ Selector
                    │   ├── ➡️ Sequence
                    │   │   ├── ⚔️ UseSkill(MissileChaseCombo) [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(NOT Self.M_ExoSuit_MissileCheck ON)]
                    │   │   └── 📋 Blackboard(ShotCount)
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(ChaseCombo) [🛡️DistanceToTarget(dist>=400.0)]
                    │       └── 📋 Blackboard(ShotCount)
                    ├── ❓ Selector
                    │   ├── ➡️ Sequence
                    │   │   ├── ⚔️ UseSkill(EnergyExplosion) [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   │   └── 📋 Blackboard(ShotCount)
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(BackStepCombo1|BackStepCombo2|SwingChainCombo|RapidCombo|MoveCombo) [🛡️CheckActorEffect(NOT Self.M_ExoSuit_NoGuardGroupCheck ON)]
                    │       └── 📋 Blackboard(ShotCount)
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(SmashCombo)
                    │   └── 📋 Blackboard(ShotCount)
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(Swing|SwingChain)
                    │   └── 📋 Blackboard(ShotCount)
                    └── ❓ Selector
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/Blackboard | 218 |
| Task/UseSkill | 124 |
| Sequence | 112 |
| Dec/Blackboard | 68 |
| Dec/CheckActorEffect | 55 |
| Selector | 52 |
| Dec/DistanceToTarget | 52 |
| Dec/UseableTime | 28 |
| Dec/Random | 23 |
| Task/UseableTimeReset | 18 |
| Dec/CheckActorStat | 14 |
| Dec/AimMe | 8 |
| Dec/CheckStance | 7 |
| Task/MoveToTarget | 6 |
| Dec/IsAlive | 3 |
| Task/Wait | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/CautionToTarget | 1 |
| Dec/TimeLimit | 1 |
| Dec/AggroLevel | 1 |

## 스킬 목록
- UseSkill(PhaseChange)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(BackStepRushCounter combo=TableCommand)
- UseSkill(BladeChaseCombo|ChaseCombo combo=TableCommand)
- UseSkill(EnergyBombShot|LaserCrossFire combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(DashShockwave)
- UseSkill(EnergySphereRush)
- UseSkill(BackMove|BackMoveCombo)
- UseSkill(LaserCrossFire)
- UseSkill(MissileMoveShot1|EnergyMoveShot1|EnergyBombShot|LaserCrossFire)
- UseSkill(LaserCrossFire)
- UseSkill(MissileMoveShot1|EnergyMoveShot1|EnergyBombShot|LaserCrossFire)
- UseSkill(BladeChaseCombo)
- UseSkill(ChaseCombo)
- UseSkill(BladeCombo|Grab)
- UseSkill(BackStepRush)
- UseSkill(BackStepCombo1|BackStepCombo2)
- UseSkill(SmashCombo)
- UseSkill(BackStepRush)
- UseSkill(Swing|SwingChain)
- UseSkill(BackStepRush)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(DashShockwave)
- UseSkill(EnergySphereRush)
- UseSkill(BackMoveCombo|BackMove)
- UseSkill(ChaseCombo)
- UseSkill(LaserCrossFire)
- UseSkill(MissileMoveShot1|EnergyMoveShot1|EnergyMoveShot3|EnergyBombShot|LaserCrossFire)
- UseSkill(ChaseCombo)
- UseSkill(LaserCrossFire)
- UseSkill(MissileMoveShot1|EnergyMoveShot1|EnergyMoveShot3|EnergyBombShot|LaserCrossFire)
- UseSkill(BladeChaseCombo)
- UseSkill(ChaseCombo)
- UseSkill(BladeCombo|Grab)
- UseSkill(BackStepRush)
- UseSkill(BackStepCombo1|BackStepCombo2|MoveCombo)
- UseSkill(SmashCombo)
- UseSkill(BackStepRush)
- UseSkill(Swing|SwingChain)
- UseSkill(BackStepRush)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight|LaserChargeShot combo=TableCommand)
- UseSkill(BackStepRushCounter combo=TableCommand)
- UseSkill(BladeChaseCombo|ChaseCombo combo=TableCommand)
- UseSkill(EnergyBombShot|LaserCrossFire|LaserChargeShot combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight|LaserChargeShot combo=TableCommand)
- UseSkill(BackStepRushCounter combo=TableCommand)
- UseSkill(AreaExplosion)
- UseSkill(FullBurst)
- UseSkill(DashShockwave)
- UseSkill(EnergySphereRush)
- UseSkill(BackMoveCombo|BackMoveMissile)
- UseSkill(LaserChargeShot)
- UseSkill(LaserChargeShot|LaserCrossFire)
- UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot)
- UseSkill(LaserChargeShot)
- UseSkill(LaserChargeShot|LaserCrossFire)
- UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot)
- UseSkill(BladeChaseCombo)
- UseSkill(ChaseCombo)
- UseSkill(BladeGrabCombo)
- UseSkill(BladeGrabCombo|Grab)
- UseSkill(EnergyShotCombo)
- UseSkill(EnergyExplosion|BackStepRush)
- UseSkill(BackStepCombo2|BackStepCombo1|EnergyShotCombo|MoveCombo)
- UseSkill(Swing|SwingChain)
- UseSkill(BackStepRush)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(FullBurst)
- UseSkill(ShockWaveCombo)
- UseSkill(ShockWaveCombo2)
- UseSkill(DashShockwave)
- UseSkill(EnergySphereRush)
- UseSkill(BackMoveCombo|BackMoveMissile)
- UseSkill(ChaseCombo)
- UseSkill(LaserChargeShot)
- UseSkill(LaserChargeShot|LaserCrossFire)
- UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot)
- UseSkill(ChaseCombo)
- UseSkill(LaserChargeShot)
- UseSkill(LaserChargeShot|LaserCrossFire)
- UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot)
- UseSkill(MissileChaseCombo)
- UseSkill(BladeChaseCombo)
- UseSkill(ChaseCombo)
- UseSkill(BladeGrabCombo)
- UseSkill(BladeGrabCombo|Grab)
- UseSkill(SwingChainCombo|RapidCombo)
- UseSkill(EnergyExplosion|BackStepRush)
- UseSkill(EnergyShotCombo|SwingChainCombo|RapidCombo)
- UseSkill(BackStepCombo2|BackStepCombo1|EnergyShotCombo|SwingChainCombo|RapidCombo|MoveCombo)
- UseSkill(Swing|SwingChain)
- UseSkill(BackStepRush)
- UseSkill(DashShockwave)
- UseSkill(EnergySphereRush)
- UseSkill(BackMoveCombo|BackMoveMissile)
- UseSkill(LaserChargeShot|LaserCrossFire)
- UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot)
- UseSkill(ChaseCombo)
- UseSkill(EnergyExplosion)
- UseSkill(BackStepCombo1|BackStepCombo2|MoveCombo)
- UseSkill(SmashCombo)
- UseSkill(Swing|SwingChain)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(DashShockwave)
- UseSkill(EnergySphereRush)
- UseSkill(BackMoveCombo|BackMoveMissile)
- UseSkill(ChaseCombo)
- UseSkill(LaserChargeShot|LaserCrossFire)
- UseSkill(MissileMoveShot2|EnergyMoveShot2|EnergyBombShot|LaserCrossFire|LaserChargeShot)
- UseSkill(MissileChaseCombo)
- UseSkill(ChaseCombo)
- UseSkill(EnergyExplosion)
- UseSkill(BackStepCombo1|BackStepCombo2|SwingChainCombo|RapidCombo|MoveCombo)
- UseSkill(SmashCombo)
- UseSkill(Swing|SwingChain)
