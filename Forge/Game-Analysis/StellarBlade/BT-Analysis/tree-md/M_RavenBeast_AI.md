# M_RavenBeast_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️IsAlive(Target) && 🛡️CheckStance(?) && 🛡️CheckActorEffect(Self.? ON)]
        │   ├── ⏳ Wait(1.0s)
        │   └── ⚔️ UseSkill(FlyRoutine2 combo=TableCommand)
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart)]
            │       ├── 📋 Blackboard(BattleStart)
            │       ├── ✨ UseEffect(['M_RavenBeast_CheckRun'])
            │       └── 🚶 MoveToTarget [🛡️TimeLimit(4.5s [StartTimer1])]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [DownCautionTimer1])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_RavenBeast_TestAI ON)]
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_Phase1 ON) && 🛡️CheckActorEffect(Target.?)]
                │   │   └── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP>85.0%)]
                │   │       ├── ➡️ Sequence [🛡️Blackboard(TS1)]
                │   │       │   ├── 🔄 UseableTimeReset
                │   │       │   ├── 🔄 UseableTimeReset
                │   │       │   └── 📋 Blackboard(TS1)
                │   │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │       │   ├── 📋 Blackboard(FirstShot)
                │   │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON)]
                │   │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   │       ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_RavenBeast_BlinkCheck ON)]
                │   │       │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=1800.0)]
                │   │       │   │   ├── 📋 Blackboard(DistanceCheck1)
                │   │       │   │   └── ❓ Selector
                │   │       │   │       ├── ⚔️ UseSkill(FlyStamp) [🛡️DistanceToTarget(dist<=2000.0)]
                │   │       │   │       └── ⚔️ UseSkill(Blink combo=TableCommand)
                │   │       │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=1000.0) && 🛡️TimeLimit(-1.0s [TimerDistance])]
                │   │       │       └── 📋 Blackboard(DistanceCheck1)
                │   │       ├── ❓ Selector
                │   │       │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_HitResult ON)]
                │   │       │       ├── ⚔️ UseSkill(Counter combo=TableCommand)
                │   │       │       └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_RavenBeast_ParryStart ON)]
                │   │       │           ├── ⚔️ UseSkill(MoveBackEscape combo=TableCommand)
                │   │       │           └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=900.0) && 🛡️CheckActorEffect(Self.M_RavenBeast_BlinkCheck ON)]
                │   │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=1200.0) && 🛡️UseableTime]
                │   │       │   ├── ⚔️ UseSkill(ColonyGuard)
                │   │       │   └── ⚔️ UseSkill(FlyStamp)
                │   │       ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_MaxHP<=98.0%)]
                │   │       │   ├── ⚔️ UseSkill(MoveBackFar)
                │   │       │   └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                │   │       │       └── ⚔️ UseSkill(ChainSpin)
                │   │       ├── ❓ Selector
                │   │       │   ├── ❓ Selector
                │   │       │   │   ├── ⚔️ UseSkill(SideMoveL)
                │   │       │   │   └── ⚔️ UseSkill(SideMoveR)
                │   │       │   └── ⚔️ UseSkill(ColonyComboChain)
                │   │       ├── ❓ Selector
                │   │       │   ├── ⚔️ UseSkill(SideRush) [🛡️DistanceToTarget(dist<=1000.0)]
                │   │       │   └── ⚔️ UseSkill(SlashCombo) [🛡️UseableTime]
                │   │       ├── ❓ Selector
                │   │       │   ├── ⚔️ UseSkill(MoveBackNear) [🛡️CheckActorEffect(NOT Self.M_RavenBeast_CheckRun ON)]
                │   │       │   ├── ⚔️ UseSkill(SwingDouble)
                │   │       │   └── ⚔️ UseSkill(Swing)
                │   │       └── ➡️ Sequence
                │   │           ├── ❓ Selector
                │   │           │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=400.0)]
                │   │           │   └── 🚶 MoveToTarget
                │   │           └── ✨ UseEffect(['M_RavenBeast_CheckRun'])
                │   ├── ❓ Selector [🛡️CheckActorEffect(Target.?) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                │   │       ├── ➡️ Sequence [🛡️Blackboard(SC1) && 🛡️CheckActorEffect(Self.M_RavenBeast_Phase3 ON)]
                │   │       │   ├── 🔄 UseableTimeReset
                │   │       │   ├── 🔄 UseableTimeReset
                │   │       │   └── 📋 Blackboard(SC1)
                │   │       ├── ❓ Selector
                │   │       │   ├── ➡️ Sequence [🛡️CheckStance(M_RavenBeast_FlyPhase3) && 🛡️CheckActorEffect(Self.M_RavenBeast_FlyRoutine2Start ON)]
                │   │       │   │   └── ⚔️ UseSkill(FlyRoutine2 combo=TableCommand)
                │   │       │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RavenBeast_StanceFlyPhase3 ON) && 🛡️CheckStance(M_RavenBeast_FlyPhase3) && 🛡️CheckActorEffect(Self.M_RavenBeast_FlyRoutine1_HitCheck ON)]
                │   │       │   │   ├── ⏳ Wait(1.0s)
                │   │       │   │   └── ⚔️ UseSkill(FlyRoutine2)
                │   │       │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_MaxHP<=25.0%)]
                │   │       │   │   └── ⚔️ UseSkill(FlyRoutine1Phase3 combo=TableCommand)
                │   │       │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_StanceFlyPhase3 ON)]
                │   │       │       ├── ➡️ Sequence [🛡️Blackboard(Fly2)]
                │   │       │       │   ├── 📋 Blackboard(Fly2)
                │   │       │       │   ├── ⚔️ UseSkill(ColonyMeteor2 combo=TableCommand)
                │   │       │       │   ├── ⏳ Wait(1.5s)
                │   │       │       │   ├── ⚔️ UseSkill(ColonyShotChain)
                │   │       │       │   ├── ⏳ Wait(1.5s)
                │   │       │       │   └── ⚔️ UseSkill(FlyRoutine2 combo=TableCommand)
                │   │       │       └── ➡️ Sequence [🛡️Blackboard(Fly2)]
                │   │       │           ├── 📋 Blackboard(Fly2)
                │   │       │           ├── ⚔️ UseSkill(ColonyShotChain)
                │   │       │           └── ⏳ Wait(1.2s)
                │   │       ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │       │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RavenBeast_StanceFlyFinish ON)]
                │   │       │   │   └── ⚔️ UseSkill(StanceChangeAttack)
                │   │       │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_MaxHP<=35.0%)]
                │   │       │   │   └── ⚔️ UseSkill(StanceChangeFly)
                │   │       │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_StanceFlyPhase3 ON)]
                │   │       │       └── ➡️ Sequence
                │   │       │           ├── ⚔️ UseSkill(ColonyShotChain)
                │   │       │           └── ⚔️ UseSkill(StanceChangeAttack)
                │   │       ├── ➡️ Sequence [🛡️AimMe && 🛡️CheckStance(NOT M_RavenBeast_FlyPhase3) && 🛡️Blackboard(FirstShot)]
                │   │       │   ├── 📋 Blackboard(FirstShot)
                │   │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️CheckStance(NOT M_RavenBeast_FlyPhase3)]
                │   │       │   └── ❓ Selector
                │   │       │       ├── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   │       │       └── ⚔️ UseSkill(Dash)
                │   │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%) && 🛡️CheckStance(NOT M_RavenBeast_FlyPhase3)]
                │   │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   │       └── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_Phase3 ON)]
                │   │           ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_RavenBeast_BlinkCheck ON)]
                │   │           │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=1800.0)]
                │   │           │   │   ├── 📋 Blackboard(DistanceCheck1)
                │   │           │   │   └── ❓ Selector
                │   │           │   │       ├── ⚔️ UseSkill(FlyStamp) [🛡️DistanceToTarget(dist<=2000.0)]
                │   │           │   │       ├── ➡️ Sequence
                │   │           │   │       │   ├── ⚔️ UseSkill(Blink combo=TableCommand)
                │   │           │   │       │   └── ⚔️ UseSkill(SideRush|SwingMoveChain combo=TableCommand) [🛡️CheckActorEffect(Self.M_RavenBeast_BlinkCheck ON)]
                │   │           │   │       └── ⚔️ UseSkill(StanceChangeFly) [🛡️CheckActorStat(ActorStatType_MaxHP<=35.0%)]
                │   │           │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=1000.0) && 🛡️TimeLimit(-1.0s [TimerDistance])]
                │   │           │       └── 📋 Blackboard(DistanceCheck1)
                │   │           ├── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_HitResult ON)]
                │   │           │   ├── ⚔️ UseSkill(Counter)
                │   │           │   └── ➡️ Sequence
                │   │           │       ├── ⚔️ UseSkill(MoveBackEscape combo=TableCommand)
                │   │           │       └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=900.0) && 🛡️CheckActorEffect(Self.M_RavenBeast_BlinkCheck ON)]
                │   │           ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │           │   ├── ➡️ Sequence
                │   │           │   │   ├── ⚔️ UseSkill(GrabSpecial combo=TableCommand)
                │   │           │   │   └── ❓ Selector
                │   │           │   │       └── ⚔️ UseSkill(MoveBackEscape|GrabSpecial2 combo=TableCommand)
                │   │           │   └── ⚔️ UseSkill(Grab)
                │   │           ├── ➡️ Sequence
                │   │           │   ├── ⚔️ UseSkill(ColonyGuardChain)
                │   │           │   └── ❓ Selector
                │   │           │       ├── ⚔️ UseSkill(Dash) [🛡️DistanceToTarget(dist<1200.0)]
                │   │           │       └── ⚔️ UseSkill(FlyStamp) [🛡️DistanceToTarget(dist>=1200.0)]
                │   │           ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_RavenBeast_BlinkCheck ON)]
                │   │           │   ├── ⚔️ UseSkill(MoveBackFar)
                │   │           │   └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                │   │           │       ├── ➡️ Sequence
                │   │           │       │   ├── ⚔️ UseSkill(ChainSpin2 combo=TableCommand)
                │   │           │       │   └── ⚔️ UseSkill(MoveBackAttack combo=TableCommand) [🛡️CheckActorEffect(NOT Target.? ON)]
                │   │           │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=800.0)]
                │   │           ├── ❓ Selector
                │   │           │   ├── ⚔️ UseSkill(SwingMoveChain)
                │   │           │   └── ⚔️ UseSkill(SwingChain)
                │   │           ├── ❓ Selector
                │   │           │   ├── ❓ Selector
                │   │           │   │   ├── ⚔️ UseSkill(SideMoveL)
                │   │           │   │   └── ⚔️ UseSkill(SideMoveR)
                │   │           │   └── ⚔️ UseSkill(ColonyCombo) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │           ├── ❓ Selector
                │   │           │   ├── ⚔️ UseSkill(RushCombo)
                │   │           │   └── ❓ Selector
                │   │           │       ├── ⚔️ UseSkill(ColonyStampLarge) [🛡️DistanceToTarget(dist<=500.0)]
                │   │           │       └── ❓ Selector [🛡️DistanceToTarget(dist>=500.0)]
                │   │           │           ├── ⚔️ UseSkill(ColonyStamp2)
                │   │           │           └── ⚔️ UseSkill(ColonyStamp)
                │   │           ├── ➡️ Sequence
                │   │           │   ├── ⚔️ UseSkill(MoveBackNear)
                │   │           │   └── ⚔️ UseSkill(GroundMissile)
                │   │           ├── ❓ Selector
                │   │           │   ├── ⚔️ UseSkill(SideRush) [🛡️DistanceToTarget(dist<=1000.0)]
                │   │           │   └── ⚔️ UseSkill(ColonySlash)
                │   │           ├── ❓ Selector
                │   │           │   ├── ⚔️ UseSkill(MoveBackNear) [🛡️CheckActorEffect(NOT Self.M_RavenBeast_CheckRun ON)]
                │   │           │   ├── ⚔️ UseSkill(SwingDouble)
                │   │           │   └── ⚔️ UseSkill(MoveCombo)
                │   │           ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RavenBeast_Phase2 ON)]
                │   │           │   ├── ❓ Selector
                │   │           │   │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=400.0)]
                │   │           │   │   └── 🚶 MoveToTarget
                │   │           │   └── ✨ UseEffect(['M_RavenBeast_CheckRun'])
                │   │           ├── ❓ Selector
                │   │           │   ├── ⚔️ UseSkill(MoveCombo)
                │   │           │   ├── ⚔️ UseSkill(Counter)
                │   │           │   └── ⚔️ UseSkill(MoveBackNear) [🛡️CheckActorEffect(NOT Self.M_RavenBeast_CheckRun ON)]
                │   │           └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RavenBeast_Phase3 ON)]
                │   │               ├── ❓ Selector
                │   │               │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=400.0)]
                │   │               │   └── 🚶 MoveToTarget
                │   │               └── ✨ UseEffect(['M_RavenBeast_CheckRun'])
                │   └── ❓ Selector [🛡️CheckActorEffect(Target.?) && 🛡️CheckActorEffect(Self.?)]
                │       └── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<85.0%) && 🛡️CheckActorStat(ActorStatType_HP>=40.0%)]
                │           ├── ➡️ Sequence [🛡️Blackboard(SC1) && 🛡️CheckActorEffect(Self.M_RavenBeast_Phase2 ON) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │           │   ├── 🔄 UseableTimeReset
                │           │   ├── 🔄 UseableTimeReset
                │           │   └── 📋 Blackboard(SC1)
                │           ├── ❓ Selector
                │           │   ├── ➡️ Sequence [🛡️CheckStance(M_RavenBeast_FlyPhase2) && 🛡️CheckActorEffect(Self.M_RavenBeast_FlyRoutine2Start ON)]
                │           │   │   └── ⚔️ UseSkill(FlyRoutine2 combo=TableCommand)
                │           │   ├── ➡️ Sequence [🛡️CheckStance(M_RavenBeast_FlyPhase2) && 🛡️CheckActorEffect(Self.M_RavenBeast_StanceFlyPhase2 ON) && 🛡️CheckActorEffect(Self.M_RavenBeast_FlyRoutine1_HitCheck ON)]
                │           │   │   ├── ⏳ Wait(1.0s)
                │           │   │   └── ⚔️ UseSkill(FlyRoutine2)
                │           │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_MaxHP<=60.0%)]
                │           │   │   └── ⚔️ UseSkill(FlyRoutine1Phase2 combo=TableCommand)
                │           │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_StanceFlyPhase2 ON)]
                │           │       ├── ➡️ Sequence [🛡️Blackboard(Fly1)]
                │           │       │   ├── 📋 Blackboard(Fly1)
                │           │       │   ├── ⚔️ UseSkill(ColonyMeteor2 combo=TableCommand)
                │           │       │   ├── ⏳ Wait(1.5s)
                │           │       │   ├── ⚔️ UseSkill(ColonyShot)
                │           │       │   ├── ⏳ Wait(1.5s)
                │           │       │   └── ⚔️ UseSkill(FlyRoutine2 combo=TableCommand)
                │           │       └── ➡️ Sequence [🛡️Blackboard(Fly1)]
                │           │           ├── 📋 Blackboard(Fly1)
                │           │           ├── ⚔️ UseSkill(ColonyShot)
                │           │           └── ⏳ Wait(1.2s)
                │           ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_MaxHP<=75.0%) && 🛡️CheckActorStat(ActorStatType_MaxHP>=42.0%) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │           │   ├── ❓ Selector
                │           │   │   └── ⚔️ UseSkill(StanceChangeFly)
                │           │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RavenBeast_StanceFlyFinish ON)]
                │           │   │   └── ⚔️ UseSkill(StanceChangeAttack)
                │           │   └── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
                │           │       └── ➡️ Sequence
                │           │           ├── ⚔️ UseSkill(ColonyShot)
                │           │           └── ⚔️ UseSkill(StanceChangeAttack)
                │           ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot) && 🛡️CheckStance(NOT M_RavenBeast_FlyPhase2)]
                │           │   ├── 📋 Blackboard(FirstShot)
                │           │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │           ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️CheckStance(NOT M_RavenBeast_FlyPhase2)]
                │           │   └── ❓ Selector
                │           │       ├── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │           │       └── ⚔️ UseSkill(Dash)
                │           └── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_Phase2 ON)]
                │               ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_RavenBeast_BlinkCheck ON)]
                │               │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=1800.0)]
                │               │   │   ├── 📋 Blackboard(DistanceCheck1)
                │               │   │   └── ❓ Selector
                │               │   │       ├── ⚔️ UseSkill(FlyStamp) [🛡️DistanceToTarget(dist<=2000.0)]
                │               │   │       └── ⚔️ UseSkill(Blink combo=TableCommand)
                │               │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=1000.0) && 🛡️TimeLimit(-1.0s [TimerDistance])]
                │               │       └── 📋 Blackboard(DistanceCheck1)
                │               ├── ❓ Selector [🛡️CheckActorEffect(Self.M_RavenBeast_HitResult ON)]
                │               │   ├── ⚔️ UseSkill(Counter)
                │               │   └── ➡️ Sequence
                │               │       ├── ⚔️ UseSkill(MoveBackEscape combo=TableCommand)
                │               │       └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=900.0) && 🛡️CheckActorEffect(Self.M_RavenBeast_BlinkCheck ON)]
                │               ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON) && 🛡️UseableTime]
                │               │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_MaxHP<=80.0%)]
                │               │   │   ├── ⚔️ UseSkill(GrabSpecial combo=TableCommand)
                │               │   │   └── ❓ Selector
                │               │   │       └── ⚔️ UseSkill(GrabSpecial2|MoveBackEscape combo=TableCommand)
                │               │   └── ⚔️ UseSkill(Grab combo=TableCommand)
                │               ├── ➡️ Sequence
                │               │   ├── ⚔️ UseSkill(ColonyGuard)
                │               │   └── ❓ Selector
                │               │       ├── ⚔️ UseSkill(Dash) [🛡️DistanceToTarget(dist<1200.0)]
                │               │       └── ⚔️ UseSkill(FlyStamp) [🛡️DistanceToTarget(dist>=1200.0)]
                │               ├── ❓ Selector
                │               │   ├── ⚔️ UseSkill(MoveBackFar)
                │               │   └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                │               │       ├── ➡️ Sequence
                │               │       │   ├── ⚔️ UseSkill(ChainSpin2 combo=TableCommand)
                │               │       │   └── ⚔️ UseSkill(MoveBackAttack combo=TableCommand) [🛡️CheckActorEffect(NOT Target.? ON)]
                │               │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=800.0)]
                │               ├── ⚔️ UseSkill(SwingMoveChain)
                │               ├── ❓ Selector
                │               │   ├── ❓ Selector
                │               │   │   ├── ⚔️ UseSkill(SideMoveL)
                │               │   │   └── ⚔️ UseSkill(SideMoveR)
                │               │   └── ⚔️ UseSkill(ColonyCombo) [🛡️CheckActorEffect(NOT Self.? ON)]
                │               ├── ❓ Selector
                │               │   ├── ⚔️ UseSkill(ColonyStamp) [🛡️DistanceToTarget(dist>=500.0)]
                │               │   └── ⚔️ UseSkill(RushCombo) [🛡️CheckActorStat(ActorStatType_MaxHP<=70.0%)]
                │               ├── ➡️ Sequence
                │               │   ├── ⚔️ UseSkill(MoveBackNear)
                │               │   └── ⚔️ UseSkill(GroundMissile)
                │               ├── ❓ Selector
                │               │   ├── ⚔️ UseSkill(SideRush) [🛡️DistanceToTarget(dist<=1000.0)]
                │               │   └── ⚔️ UseSkill(SlashCombo)
                │               ├── ❓ Selector
                │               │   ├── ⚔️ UseSkill(MoveBackNear) [🛡️CheckActorEffect(NOT Self.M_RavenBeast_CheckRun ON)]
                │               │   ├── ⚔️ UseSkill(SwingDouble)
                │               │   └── ⚔️ UseSkill(MoveCombo)
                │               └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_RavenBeast_Phase2 ON)]
                │                   ├── ❓ Selector
                │                   │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=400.0)]
                │                   │   └── 🚶 MoveToTarget
                │                   └── ✨ UseEffect(['M_RavenBeast_CheckRun'])
                └── ❓ Selector
                    ├── ➡️ Sequence [🛡️CheckStance(M_RavenBeast_Phase2) && 🛡️CheckActorEffect(Self.M_RavenBeast_Phase2 ON) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                    │   ├── ⚔️ UseSkill(PhaseChange3)
                    │   └── ❓ Selector
                    │       └── ⚔️ UseSkill(SwingChain)
                    └── ➡️ Sequence [🛡️CheckStance(M_RavenBeast_Default) && 🛡️CheckActorEffect(Self.M_RavenBeast_Phase1 ON) && 🛡️CheckActorStat(ActorStatType_HP<=85.0%)]
                        ├── ⚔️ UseSkill(PhaseChange2 combo=TableCommand)
                        └── ❓ Selector
                            ├── ⚔️ UseSkill(Grab) [🛡️DistanceToTarget(dist<=500.0)]
                            └── ⚔️ UseSkill(ColonyStamp) [🛡️DistanceToTarget(dist>=500.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 113 |
| Selector | 75 |
| Dec/CheckActorEffect | 63 |
| Sequence | 51 |
| Dec/DistanceToTarget | 40 |
| Task/Blackboard | 17 |
| Dec/CheckActorStat | 16 |
| Dec/Blackboard | 14 |
| Dec/TimeLimit | 14 |
| Dec/CheckStance | 12 |
| Task/Wait | 11 |
| Task/CautionToTarget | 10 |
| Task/UseableTimeReset | 6 |
| Task/UseEffect | 5 |
| Task/MoveToTarget | 5 |
| Dec/IsAlive | 4 |
| Dec/AimMe | 4 |
| Dec/Random | 4 |
| Dec/UseableTime | 3 |
| Dec/AggroLevel | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |

## 스킬 목록
- UseSkill(FlyRoutine2 combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(FlyStamp)
- UseSkill(Blink combo=TableCommand)
- UseSkill(Counter combo=TableCommand)
- UseSkill(MoveBackEscape combo=TableCommand)
- UseSkill(ColonyGuard)
- UseSkill(FlyStamp)
- UseSkill(MoveBackFar)
- UseSkill(ChainSpin)
- UseSkill(SideMoveL)
- UseSkill(SideMoveR)
- UseSkill(ColonyComboChain)
- UseSkill(SideRush)
- UseSkill(SlashCombo)
- UseSkill(MoveBackNear)
- UseSkill(SwingDouble)
- UseSkill(Swing)
- UseSkill(FlyRoutine2 combo=TableCommand)
- UseSkill(FlyRoutine2)
- UseSkill(FlyRoutine1Phase3 combo=TableCommand)
- UseSkill(ColonyMeteor2 combo=TableCommand)
- UseSkill(ColonyShotChain)
- UseSkill(FlyRoutine2 combo=TableCommand)
- UseSkill(ColonyShotChain)
- UseSkill(StanceChangeAttack)
- UseSkill(StanceChangeFly)
- UseSkill(ColonyShotChain)
- UseSkill(StanceChangeAttack)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Dash)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(FlyStamp)
- UseSkill(Blink combo=TableCommand)
- UseSkill(SideRush|SwingMoveChain combo=TableCommand)
- UseSkill(StanceChangeFly)
- UseSkill(Counter)
- UseSkill(MoveBackEscape combo=TableCommand)
- UseSkill(GrabSpecial combo=TableCommand)
- UseSkill(MoveBackEscape|GrabSpecial2 combo=TableCommand)
- UseSkill(Grab)
- UseSkill(ColonyGuardChain)
- UseSkill(Dash)
- UseSkill(FlyStamp)
- UseSkill(MoveBackFar)
- UseSkill(ChainSpin2 combo=TableCommand)
- UseSkill(MoveBackAttack combo=TableCommand)
- UseSkill(SwingMoveChain)
- UseSkill(SwingChain)
- UseSkill(SideMoveL)
- UseSkill(SideMoveR)
- UseSkill(ColonyCombo)
- UseSkill(RushCombo)
- UseSkill(ColonyStampLarge)
- UseSkill(ColonyStamp2)
- UseSkill(ColonyStamp)
- UseSkill(MoveBackNear)
- UseSkill(GroundMissile)
- UseSkill(SideRush)
- UseSkill(ColonySlash)
- UseSkill(MoveBackNear)
- UseSkill(SwingDouble)
- UseSkill(MoveCombo)
- UseSkill(MoveCombo)
- UseSkill(Counter)
- UseSkill(MoveBackNear)
- UseSkill(FlyRoutine2 combo=TableCommand)
- UseSkill(FlyRoutine2)
- UseSkill(FlyRoutine1Phase2 combo=TableCommand)
- UseSkill(ColonyMeteor2 combo=TableCommand)
- UseSkill(ColonyShot)
- UseSkill(FlyRoutine2 combo=TableCommand)
- UseSkill(ColonyShot)
- UseSkill(StanceChangeFly)
- UseSkill(StanceChangeAttack)
- UseSkill(ColonyShot)
- UseSkill(StanceChangeAttack)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Dash)
- UseSkill(FlyStamp)
- UseSkill(Blink combo=TableCommand)
- UseSkill(Counter)
- UseSkill(MoveBackEscape combo=TableCommand)
- UseSkill(GrabSpecial combo=TableCommand)
- UseSkill(GrabSpecial2|MoveBackEscape combo=TableCommand)
- UseSkill(Grab combo=TableCommand)
- UseSkill(ColonyGuard)
- UseSkill(Dash)
- UseSkill(FlyStamp)
- UseSkill(MoveBackFar)
- UseSkill(ChainSpin2 combo=TableCommand)
- UseSkill(MoveBackAttack combo=TableCommand)
- UseSkill(SwingMoveChain)
- UseSkill(SideMoveL)
- UseSkill(SideMoveR)
- UseSkill(ColonyCombo)
- UseSkill(ColonyStamp)
- UseSkill(RushCombo)
- UseSkill(MoveBackNear)
- UseSkill(GroundMissile)
- UseSkill(SideRush)
- UseSkill(SlashCombo)
- UseSkill(MoveBackNear)
- UseSkill(SwingDouble)
- UseSkill(MoveCombo)
- UseSkill(PhaseChange3)
- UseSkill(SwingChain)
- UseSkill(PhaseChange2 combo=TableCommand)
- UseSkill(Grab)
- UseSkill(ColonyStamp)
