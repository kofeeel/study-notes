# M_Crawler_AI

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
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [AbnormalTimer])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                │   │   └── ⚔️ UseSkill(2PhaseShift) [🛡️CheckStance(M_Crawler_Phase2)]
                │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                │       └── ⚔️ UseSkill(1PhaseShift) [🛡️CheckStance(M_Crawler_Default)]
                ├── ❓ Selector [🛡️CheckStance(M_Crawler_Default) && 🛡️CheckActorEffect(Self.M_Crawler_Phase1 ON) && 🛡️CheckActorStat(ActorStatType_HP>80.0%)]
                │   ├── ➡️ Sequence [🛡️Blackboard(Phase1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(Phase1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvasionLeft1|EvasionRight1 combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(EvasionLeft1|EvasionRight1 combo=TableCommand)
                │   ├── ❓ Selector [🛡️Blackboard(FirstProjectile) && 🛡️UseableTime && 🛡️DistanceToTarget(dist>=1000.0)]
                │   │   └── 📋 Blackboard(FirstProjectile)
                │   ├── ➡️ Sequence [🛡️Blackboard(FirstProjectile)]
                │   │   ├── ⚔️ UseSkill(FairySwarm1)
                │   │   └── 📋 Blackboard(FirstProjectile)
                │   ├── ❓ Selector [🛡️Blackboard(FirstProjectile) && 🛡️UseableTime && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(FairySwarm1|FairySwarm1Extension)
                │   │   │   └── 🚶 MoveToTarget
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(LightningCore1)
                │   │       └── 🚶 MoveToTarget
                │   ├── ➡️ Sequence [🛡️Blackboard(FirstProjectile) && 🛡️UseableTime]
                │   │   ├── ⚔️ UseSkill(BackMove1)
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=4000.0) && 🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckStance(M_Crawler_Default) && 🛡️CheckActorEffect(Self.M_Crawler_Phase1 ON)]
                │   ├── ❓ Selector [🛡️Blackboard(FirstProjectile) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(SnatchGrab) [🛡️DistanceToTarget(dist>=400.0)]
                │   ├── ❓ Selector [🛡️Blackboard(FirstProjectile) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck ON)]
                │   │   └── ⚔️ UseSkill(JumpSmash1)
                │   ├── ❓ Selector [🛡️Blackboard(FirstProjectile) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck ON)]
                │   │   └── ⚔️ UseSkill(ScratchDownCombo)
                │   ├── ❓ Selector [🛡️Blackboard(FirstProjectile) && 🛡️Blackboard(FirstAttack)]
                │   │   └── 📋 Blackboard(FirstAttack)
                │   ├── ➡️ Sequence [🛡️Blackboard(FirstAttack)]
                │   │   ├── ⚔️ UseSkill(DoubleScratch|OffbeatAttack)
                │   │   └── 📋 Blackboard(FirstAttack)
                │   ├── ❓ Selector [🛡️Blackboard(FirstProjectile) && 🛡️Blackboard(FirstAttack)]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(BackStepAttack|OffbeatAttack)
                │   │   │   └── ⏳ WaitTimeRandom
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(DoubleScratch|Smash)
                │   │       └── ⏳ WaitTimeRandom
                │   ├── ❓ Selector [🛡️Blackboard(FirstProjectile) && 🛡️UseableTime]
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckStance(M_Crawler_Default) && 🛡️CheckActorEffect(Self.M_Crawler_Phase1 ON)]
                │   └── ❓ Selector [🛡️Blackboard(FirstProjectile)]
                │       └── 🚶 MoveToTarget
                ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Crawler_Phase2 ON) && 🛡️CheckActorStat(ActorStatType_HP<=80.0%) && 🛡️CheckActorStat(ActorStatType_HP>40.0%)]
                │   ├── ➡️ Sequence [🛡️Blackboard(Phase2)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(Phase2)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
                │   ├── ❓ Selector
                │   │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=400.0)]
                │   │   │   └── ⚔️ UseSkill(PlasmaCannonRotation)
                │   │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0)]
                │   │       ├── ⚔️ UseSkill(PlasmaCannon1)
                │   │       └── 🚶 MoveToTarget
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(PlasmaMineSet)
                │   │   │   └── 🚶 MoveToTarget
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(FairySwarm2|FairySwarm2Extension)
                │   │   │   └── 🚶 MoveToTarget
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(LightningCore2)
                │   │       └── 🚶 MoveToTarget
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=4000.0) && 🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckStance(M_Crawler_Phase2) && 🛡️CheckActorEffect(Self.M_Crawler_Phase2 ON)]
                │   ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_WarpCheck ON)]
                │   │   ├── ⚔️ UseSkill(BackMove2)
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=4000.0) && 🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckStance(M_Crawler_Phase2) && 🛡️CheckActorEffect(Self.M_Crawler_Phase2 ON)]
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(WarpLongRange)
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️DistanceToTarget(dist>=1500.0)]
                │   │   └── ⚔️ UseSkill(WarpSlashDown)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck ON) && 🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
                │   │   └── ⚔️ UseSkill(GrabFail)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck ON) && 🛡️CheckActorStat(ActorStatType_HP<=78.0%)]
                │   │   └── ⚔️ UseSkill(HexaSlash|ComboGrab)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck ON)]
                │   │   └── ⚔️ UseSkill(JumpSmash2)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck ON)]
                │   │   └── ⚔️ UseSkill(SlashDownCombo)
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(TripleSlash)
                │   │   │   └── ⏳ WaitTimeRandom
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(DoubleSlash|SmashDouble)
                │   │       └── ⏳ WaitTimeRandom
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(1.73s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckStance(M_Crawler_Phase2) && 🛡️CheckActorEffect(Self.M_Crawler_Phase2 ON)]
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckActorEffect(Self.M_Crawler_Phase3 ON) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                    ├── ➡️ Sequence [🛡️Blackboard(Phase3)]
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(Phase3)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                    │   ├── 📋 Blackboard(FirstShot)
                    │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand) [🛡️Random(rand(100)<=50)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                    │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                    │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=1500.0)]
                    │   │   └── ⚔️ UseSkill(LightningSmash)
                    │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   │   └── ⚔️ UseSkill(WarpLongRange combo=TableCommand)
                    │   └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON)]
                    │       └── ⚔️ UseSkill(LightningDoubleSlash)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                    │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=700.0)]
                    │   │   └── ⚔️ UseSkill(PlasmaCannonCircle)
                    │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=400.0)]
                    │   │   └── ⚔️ UseSkill(PlasmaCannonRotation)
                    │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0)]
                    │       ├── ⚔️ UseSkill(PlasmaCannon2)
                    │       └── 🚶 MoveToTarget
                    ├── ❓ Selector [🛡️UseableTime && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    │   ├── ➡️ Sequence
                    │   │   ├── ⚔️ UseSkill(PlasmaMineSet)
                    │   │   └── 🚶 MoveToTarget
                    │   ├── ➡️ Sequence
                    │   │   ├── ⚔️ UseSkill(FairySwarm2Extension)
                    │   │   └── 🚶 MoveToTarget
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(LightningCore2)
                    │       └── 🚶 MoveToTarget
                    ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=4000.0) && 🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckStance(M_Crawler_Phase2) && 🛡️CheckActorEffect(Self.M_Crawler_Phase2 ON)]
                    ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_WarpCheck ON)]
                    │   ├── ⚔️ UseSkill(BackMove2)
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=4000.0) && 🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckStance(M_Crawler_Phase3) && 🛡️CheckActorEffect(Self.M_Crawler_Phase3 ON)]
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(WarpLongRange)
                    ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️DistanceToTarget(dist>=1500.0) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    │   └── ⚔️ UseSkill(WarpSlashDown)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck2 ON) && 🛡️CheckActorStat(ActorStatType_HP<=30.0%)]
                    │   └── ⚔️ UseSkill(GrabFail)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck2 ON) && 🛡️CheckActorStat(ActorStatType_HP<=38.0%)]
                    │   └── ⚔️ UseSkill(HexaSlash|ComboGrab)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck2 ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    │   └── ⚔️ UseSkill(JumpSmash3)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Crawler_NoGuardCheck2 ON)]
                    │   └── ⚔️ UseSkill(SlashDownCombo)
                    ├── ❓ Selector
                    │   ├── ➡️ Sequence
                    │   │   ├── ⚔️ UseSkill(TripleSlash)
                    │   │   └── ⏳ WaitTimeRandom
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(DoubleSlash|SmashDouble)
                    │       └── ⏳ WaitTimeRandom
                    └── ❓ Selector
                        ├── ⚠️ CautionToTarget [🛡️TimeLimit(1.73s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckStance(M_Crawler_Phase3) && 🛡️CheckActorEffect(Self.M_Crawler_Phase3 ON)]
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 51 |
| Dec/CheckActorEffect | 44 |
| Selector | 43 |
| Sequence | 36 |
| Dec/DistanceToTarget | 29 |
| Dec/UseableTime | 23 |
| Dec/Blackboard | 20 |
| Task/MoveToTarget | 13 |
| Dec/CheckActorStat | 11 |
| Dec/CheckStance | 11 |
| Task/UseableTimeReset | 11 |
| Task/Blackboard | 10 |
| Task/CautionToTarget | 9 |
| Dec/TimeLimit | 9 |
| Dec/Random | 7 |
| Task/WaitTimeRandom | 6 |
| Dec/AimMe | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/AggroLevel | 1 |

## 스킬 목록
- UseSkill(2PhaseShift)
- UseSkill(1PhaseShift)
- UseSkill(EvasionLeft1|EvasionRight1 combo=TableCommand)
- UseSkill(EvasionLeft1|EvasionRight1 combo=TableCommand)
- UseSkill(FairySwarm1)
- UseSkill(FairySwarm1|FairySwarm1Extension)
- UseSkill(LightningCore1)
- UseSkill(BackMove1)
- UseSkill(SnatchGrab)
- UseSkill(JumpSmash1)
- UseSkill(ScratchDownCombo)
- UseSkill(DoubleScratch|OffbeatAttack)
- UseSkill(BackStepAttack|OffbeatAttack)
- UseSkill(DoubleScratch|Smash)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(PlasmaCannonRotation)
- UseSkill(PlasmaCannon1)
- UseSkill(PlasmaMineSet)
- UseSkill(FairySwarm2|FairySwarm2Extension)
- UseSkill(LightningCore2)
- UseSkill(BackMove2)
- UseSkill(WarpLongRange)
- UseSkill(WarpSlashDown)
- UseSkill(GrabFail)
- UseSkill(HexaSlash|ComboGrab)
- UseSkill(JumpSmash2)
- UseSkill(SlashDownCombo)
- UseSkill(TripleSlash)
- UseSkill(DoubleSlash|SmashDouble)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(LightningSmash)
- UseSkill(WarpLongRange combo=TableCommand)
- UseSkill(LightningDoubleSlash)
- UseSkill(PlasmaCannonCircle)
- UseSkill(PlasmaCannonRotation)
- UseSkill(PlasmaCannon2)
- UseSkill(PlasmaMineSet)
- UseSkill(FairySwarm2Extension)
- UseSkill(LightningCore2)
- UseSkill(BackMove2)
- UseSkill(WarpLongRange)
- UseSkill(WarpSlashDown)
- UseSkill(GrabFail)
- UseSkill(HexaSlash|ComboGrab)
- UseSkill(JumpSmash3)
- UseSkill(SlashDownCombo)
- UseSkill(TripleSlash)
- UseSkill(DoubleSlash|SmashDouble)
