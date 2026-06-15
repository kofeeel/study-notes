# M_WeaponMasterA_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   └── ➡️ Sequence [🛡️Blackboard(BattleStart)]
        │       ├── 📋 Blackboard(BattleStart)
        │       ├── 🚶 MoveToTarget [🛡️TimeLimit(4.3s [StartTimer1])]
        │       ├── ⚔️ UseSkill(GreatSwordSwingCombo)
        │       └── ✨ UseEffect(['M_WeaponMasterA_CheckRun'])
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.5s [DownCautionTimer1])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️CheckStance(M_WeaponMasterA_Phase2) && 🛡️CheckActorEffect(Self.M_WeaponMasterA_Phase2 ON) && 🛡️CheckActorStat(ActorStatType_HP<=1.0%)]
                │   │   └── ⚔️ UseSkill(PhaseChange3 combo=TableCommand)
                │   └── ➡️ Sequence [🛡️CheckStance(M_WeaponMasterA_Default) && 🛡️CheckActorEffect(NOT Self.M_WeaponMasterA_Phase2 ON) && 🛡️CheckActorStat(ActorStatType_HP<=1.0%)]
                │       └── ⚔️ UseSkill(PhaseChange2 combo=TableCommand)
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorStat(ActorStatType_HP>1.0%) && 🛡️CheckActorEffect(NOT Self.M_WeaponMasterA_Phase2 ON) && 🛡️CheckStance(NOT M_WeaponMasterA_Phase3)]
                │   ├── ➡️ Sequence [🛡️Blackboard(TS1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(TS1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP>=1.0%)]
                │   │   └── ❓ Selector
                │   │       ├── ⚔️ UseSkill(ParrySkill) [🛡️CheckActorStat(ActorStatType_HP<=98.0%)]
                │   │       ├── ⚔️ UseSkill(CounterSkill) [🛡️CheckActorEffect(Self.M_WeaponMasterA_HitResult ON)]
                │   │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                │   │       ├── ❓ Selector
                │   │       │   ├── ⚔️ UseSkill(GreatSwordStamp) [🛡️DistanceToTarget(dist<=500.0) && 🛡️UseableTime && 🛡️CheckStance(NOT M_WeaponMasterA_Phase3)]
                │   │       │   ├── ⚔️ UseSkill(GreatSwordSwing)
                │   │       │   └── ⚔️ UseSkill(MoveBack|TwinSwordSpin|TwinSwordSwing combo=TableCommand)
                │   │       ├── ⚔️ UseSkill(GreatSwordSwingCombo) [🛡️UseableTime]
                │   │       ├── ⚔️ UseSkill(TwinSwordCombo) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │       └── ❓ Selector
                │   │           ├── ⚔️ UseSkill(TwinSwordSpin|MoveLeft|MoveRight combo=TableCommand)
                │   │           ├── ❓ Selector
                │   │           │   ├── ⚔️ UseSkill(TwinSwordRushChain combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                │   │           │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                │   │           └── ⚔️ UseSkill(TwinSwordRush)
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(TwinSwordSpin combo=TableCommand)
                │   │   ├── ⚔️ UseSkill(TwinSwordSwing|MoveBack combo=TableCommand)
                │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Self.M_WeaponMasterA_CheckRun) && 🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=150.0)]
                │   └── ➡️ Sequence
                │       ├── ❓ Selector
                │       │   └── 🚶 MoveToTarget
                │       └── ✨ UseEffect(['M_WeaponMasterA_CheckRun'])
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_WeaponMasterA_Phase2 ON) && 🛡️CheckActorStat(ActorStatType_HP>1.0%)]
                │   ├── ➡️ Sequence [🛡️Blackboard(TS1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(TS1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP>=1.0%)]
                │   │   ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
                │   │   │   ├── ➡️ Sequence
                │   │   │   │   ├── ⚔️ UseSkill(Grab)
                │   │   │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                │   │   │   ├── ❓ Selector
                │   │   │   │   ├── ⚔️ UseSkill(TwinSwordCombo_2) [🛡️CheckActorEffect(Self.? ON)]
                │   │   │   │   └── ⚔️ UseSkill(TwinSwordCombo)
                │   │   │   └── ⚔️ UseSkill(TwinSwordRushChain combo=TableCommand) [🛡️TimeLimit(-1.0s [SkillTimer1]) && 🛡️CheckActorEffect(Self.? ON)]
                │   │   └── ❓ Selector
                │   │       ├── ⚔️ UseSkill(ParrySkill) [🛡️CheckActorStat(ActorStatType_HP<=98.0%)]
                │   │       ├── ⚔️ UseSkill(CounterSkill) [🛡️CheckActorEffect(Self.M_WeaponMasterA_HitResult ON)]
                │   │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                │   │       ├── ❓ Selector
                │   │       │   ├── ⚔️ UseSkill(GreatSwordStamp) [🛡️DistanceToTarget(dist<=500.0) && 🛡️UseableTime && 🛡️CheckStance(NOT M_WeaponMasterA_Phase3)]
                │   │       │   ├── ⚔️ UseSkill(GreatSwordSwing)
                │   │       │   └── ⚔️ UseSkill(MoveBack|TwinSwordSpin|TwinSwordSwing combo=TableCommand)
                │   │       ├── ⚔️ UseSkill(GreatSwordSwingCombo) [🛡️UseableTime]
                │   │       └── ❓ Selector
                │   │           ├── ⚔️ UseSkill(TwinSwordSpin|MoveLeft|MoveRight combo=TableCommand)
                │   │           ├── ❓ Selector
                │   │           │   ├── ⚔️ UseSkill(TwinSwordRushChain combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                │   │           │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                │   │           └── ⚔️ UseSkill(TwinSwordRush)
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(TwinSwordSpin combo=TableCommand)
                │   │   ├── ⚔️ UseSkill(TwinSwordSwing|MoveBack combo=TableCommand)
                │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Self.M_WeaponMasterA_CheckRun) && 🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=150.0)]
                │   └── ➡️ Sequence
                │       ├── ❓ Selector
                │       │   └── 🚶 MoveToTarget
                │       └── ✨ UseEffect(['M_WeaponMasterA_CheckRun'])
                └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckStance(M_WeaponMasterA_Phase3)]
                    ├── ➡️ Sequence [🛡️Blackboard(TS1)]
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(TS1)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                    │   ├── 📋 Blackboard(FirstShot)
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                    ├── ❓ Selector
                    │   ├── ❓ Selector [🛡️CheckStance(M_WeaponMasterA_Phase3)]
                    │   │   ├── ⚔️ UseSkill(GreatSwordSlash)
                    │   │   └── ⚔️ UseSkill(GreatSwordSwingChain)
                    │   ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
                    │   │   ├── ➡️ Sequence
                    │   │   │   ├── ❓ Selector
                    │   │   │   │   ├── ⚔️ UseSkill(GreatSwordStamp) [🛡️CheckStance(M_WeaponMasterA_Phase3) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                    │   │   │   │   └── ⚔️ UseSkill(Grab)
                    │   │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                    │   │   ├── ❓ Selector
                    │   │   │   ├── ⚔️ UseSkill(TwinSwordCombo_2) [🛡️CheckActorEffect(Self.? ON) && 🛡️TimeLimit(-1.0s [SkillTimer1])]
                    │   │   │   └── ⚔️ UseSkill(TwinSwordCombo)
                    │   │   └── ⚔️ UseSkill(TwinSwordRushChain combo=TableCommand) [🛡️TimeLimit(-1.0s [SkillTimer1]) && 🛡️CheckActorEffect(Self.? ON)]
                    │   └── ❓ Selector
                    │       ├── ⚔️ UseSkill(ParrySkill) [🛡️CheckActorStat(ActorStatType_HP<=98.0%)]
                    │       ├── ⚔️ UseSkill(CounterSkill) [🛡️CheckActorEffect(Self.M_WeaponMasterA_HitResult ON)]
                    │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                    │       ├── ❓ Selector
                    │       │   ├── ⚔️ UseSkill(GreatSwordSwing)
                    │       │   └── ⚔️ UseSkill(MoveBack|TwinSwordSpin|TwinSwordSwing combo=TableCommand)
                    │       ├── ⚔️ UseSkill(GreatSwordSwingCombo) [🛡️UseableTime]
                    │       ├── ⚔️ UseSkill(TwinSwordCombo) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │       └── ❓ Selector
                    │           ├── ⚔️ UseSkill(TwinSwordSpin|MoveLeft|MoveRight combo=TableCommand)
                    │           ├── ❓ Selector
                    │           │   ├── ⚔️ UseSkill(TwinSwordRushChain combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                    │           │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0)]
                    │           └── ⚔️ UseSkill(TwinSwordRush)
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(TwinSwordSpin combo=TableCommand)
                    │   ├── ⚔️ UseSkill(TwinSwordSwing|MoveBack combo=TableCommand)
                    │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Self.M_WeaponMasterA_CheckRun) && 🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=150.0)]
                    └── ➡️ Sequence
                        ├── ❓ Selector
                        │   └── 🚶 MoveToTarget
                        └── ✨ UseEffect(['M_WeaponMasterA_CheckRun'])
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 57 |
| Selector | 38 |
| Dec/CheckActorEffect | 26 |
| Dec/DistanceToTarget | 25 |
| Sequence | 20 |
| Dec/TimeLimit | 16 |
| Dec/CheckActorStat | 16 |
| Task/CautionToTarget | 12 |
| Task/UseableTimeReset | 12 |
| Dec/Random | 9 |
| Dec/CheckStance | 8 |
| Dec/Blackboard | 7 |
| Task/Blackboard | 7 |
| Dec/UseableTime | 7 |
| Dec/AimMe | 6 |
| Task/MoveToTarget | 4 |
| Task/UseEffect | 4 |
| Dec/IsAlive | 3 |
| Dec/AggroLevel | 2 |
| Task/Wait | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |

## 스킬 목록
- UseSkill(GreatSwordSwingCombo)
- UseSkill(PhaseChange3 combo=TableCommand)
- UseSkill(PhaseChange2 combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(ParrySkill)
- UseSkill(CounterSkill)
- UseSkill(GreatSwordStamp)
- UseSkill(GreatSwordSwing)
- UseSkill(MoveBack|TwinSwordSpin|TwinSwordSwing combo=TableCommand)
- UseSkill(GreatSwordSwingCombo)
- UseSkill(TwinSwordCombo)
- UseSkill(TwinSwordSpin|MoveLeft|MoveRight combo=TableCommand)
- UseSkill(TwinSwordRushChain combo=TableCommand)
- UseSkill(TwinSwordRush)
- UseSkill(TwinSwordSpin combo=TableCommand)
- UseSkill(TwinSwordSwing|MoveBack combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Grab)
- UseSkill(TwinSwordCombo_2)
- UseSkill(TwinSwordCombo)
- UseSkill(TwinSwordRushChain combo=TableCommand)
- UseSkill(ParrySkill)
- UseSkill(CounterSkill)
- UseSkill(GreatSwordStamp)
- UseSkill(GreatSwordSwing)
- UseSkill(MoveBack|TwinSwordSpin|TwinSwordSwing combo=TableCommand)
- UseSkill(GreatSwordSwingCombo)
- UseSkill(TwinSwordSpin|MoveLeft|MoveRight combo=TableCommand)
- UseSkill(TwinSwordRushChain combo=TableCommand)
- UseSkill(TwinSwordRush)
- UseSkill(TwinSwordSpin combo=TableCommand)
- UseSkill(TwinSwordSwing|MoveBack combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(GreatSwordSlash)
- UseSkill(GreatSwordSwingChain)
- UseSkill(GreatSwordStamp)
- UseSkill(Grab)
- UseSkill(TwinSwordCombo_2)
- UseSkill(TwinSwordCombo)
- UseSkill(TwinSwordRushChain combo=TableCommand)
- UseSkill(ParrySkill)
- UseSkill(CounterSkill)
- UseSkill(GreatSwordSwing)
- UseSkill(MoveBack|TwinSwordSpin|TwinSwordSwing combo=TableCommand)
- UseSkill(GreatSwordSwingCombo)
- UseSkill(TwinSwordCombo)
- UseSkill(TwinSwordSpin|MoveLeft|MoveRight combo=TableCommand)
- UseSkill(TwinSwordRushChain combo=TableCommand)
- UseSkill(TwinSwordRush)
- UseSkill(TwinSwordSpin combo=TableCommand)
- UseSkill(TwinSwordSwing|MoveBack combo=TableCommand)
