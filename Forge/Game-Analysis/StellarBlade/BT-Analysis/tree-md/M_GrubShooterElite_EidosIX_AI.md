# M_GrubShooterElite_EidosIX_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       ├── 🎬 PlayShow
            │       └── ⏳ Wait(1.2s)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ➡️ Sequence [🛡️CheckActorEffect(Target.? ON)]
            │   ├── 🎬 PlayShow
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB2)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB2)
            │       ├── ➡️ Sequence [🛡️Blackboard(UseSkill)]
            │       │   ├── 🎬 PlayShow
            │       │   ├── ⏳ WaitTimeRandom
            │       │   └── 📋 Blackboard(UseSkill)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │       │   ├── 📋 Blackboard(FirstShot)
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️UseableTime]
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(ThornThrust)
            │       │       └── ❓ Selector
            │       │           └── ⚔️ UseSkill(ThornThrust)
            │       ├── ⚔️ UseSkill(JumpStand_1)
            │       ├── ⚔️ UseSkill(JumpBack) [🛡️UseableTime]
            │       ├── ➡️ Sequence [🛡️Blackboard(StartShot)]
            │       │   ├── ⚔️ UseSkill(SpitMassAttack_1)
            │       │   ├── 📋 Blackboard(StartShot)
            │       │   └── 📋 Blackboard(UseSkill)
            │       ├── ➡️ Sequence [🛡️Blackboard(ShotCount)]
            │       │   ├── ⚔️ UseSkill(SpitMassAttack_1)
            │       │   └── 📋 Blackboard(UseSkill)
            │       ├── ➡️ Sequence
            │       │   ├── 📋 Blackboard(ShotCount)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence
            │       │       │   ├── ⚔️ UseSkill(SpitDozenAttack_1 combo=TableCommand) [🛡️DistanceToTarget(dist<900.0)]
            │       │       │   └── 📋 Blackboard(UseSkill)
            │       │       ├── ➡️ Sequence
            │       │       │   ├── ⚔️ UseSkill(SpitAttack_1 combo=TableCommand)
            │       │       │   └── 📋 Blackboard(UseSkill)
            │       │       └── ➡️ Sequence
            │       │           ├── ⚔️ UseSkill(SpitTrainAttack_1 combo=TableCommand)
            │       │           └── 📋 Blackboard(UseSkill)
            │       └── ❓ Selector
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️IsGroupTarget(NOT)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ❓ Selector [🛡️DistanceToTarget(dist>=800.0) && 🛡️DistanceToTarget(dist<2000.0)]
                │   ├── ➡️ Sequence [🛡️Blackboard(ShotCount)]
                │   │   ├── ⚔️ UseSkill(RushAttack1)
                │   │   └── 📋 Blackboard(ShotCount)
                │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_KnockbackAttackCheck ON)]
                │   │   └── ⚔️ UseSkill(SpitDozenAttack_1 combo=TableCommand)
                │   └── ➡️ Sequence [🛡️Blackboard(ShotCount)]
                │       ├── ⚔️ UseSkill(SpitTrainAttack_1|SpitAttack_1|SpitMassAttack_1)
                │       ├── 📋 Blackboard(ShotCount)
                │       └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=90.0%) && 🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_NoGuardCheck ON)]
                │   ├── 📋 Blackboard(ShotCount)
                │   ├── ⚔️ UseSkill(ShockWaveAttack)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_NoGuardCheck ON) && 🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
                │   ├── 📋 Blackboard(ShotCount)
                │   └── ⚔️ UseSkill(ShockWaveAttack)
                ├── ⚔️ UseSkill(JumpStand_1)
                ├── ➡️ Sequence
                │   ├── 📋 Blackboard(ShotCount) [🛡️DistanceToTarget(dist<=500.0)]
                │   └── ❓ Selector
                │       ├── ➡️ Sequence
                │       │   ├── ⚔️ UseSkill(ThornThrust) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_NoGuardCheck ON)]
                │       │   └── ⏳ WaitTimeRandom
                │       └── ❓ Selector
                │           ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_KnockbackAttackCheck ON) && 🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                │           │   └── ⚔️ UseSkill(RandomSpit combo=TableCommand)
                │           ├── ➡️ Sequence
                │           │   ├── ⚔️ UseSkill(ComboAttack)
                │           │   └── ❓ Selector
                │           │       ├── ⚔️ UseSkill(JumpBack) [🛡️UseableTime]
                │           │       └── ⏳ WaitTimeRandom
                │           └── ➡️ Sequence
                │               ├── ⚔️ UseSkill(HeadAttack)
                │               └── ❓ Selector
                │                   ├── ⚔️ UseSkill(JumpBack) [🛡️UseableTime]
                │                   └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 28 |
| Task/UseSkill | 27 |
| Selector | 19 |
| Task/Blackboard | 18 |
| Dec/CheckActorEffect | 12 |
| Dec/Blackboard | 10 |
| Task/WaitTimeRandom | 7 |
| Dec/Random | 6 |
| Task/UseableTimeReset | 5 |
| Dec/CheckActorStat | 5 |
| Dec/UseableTime | 5 |
| Dec/AggroLevel | 4 |
| Dec/AimMe | 4 |
| Dec/DistanceToTarget | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Task/PlayShow | 3 |
| Task/MoveToTarget | 3 |
| Dec/IsGroupTarget | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |

## 스킬 목록
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(ThornThrust)
- UseSkill(ThornThrust)
- UseSkill(JumpStand_1)
- UseSkill(JumpBack)
- UseSkill(SpitMassAttack_1)
- UseSkill(SpitMassAttack_1)
- UseSkill(SpitDozenAttack_1 combo=TableCommand)
- UseSkill(SpitAttack_1 combo=TableCommand)
- UseSkill(SpitTrainAttack_1 combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(RushAttack1)
- UseSkill(SpitDozenAttack_1 combo=TableCommand)
- UseSkill(SpitTrainAttack_1|SpitAttack_1|SpitMassAttack_1)
- UseSkill(ShockWaveAttack)
- UseSkill(ShockWaveAttack)
- UseSkill(JumpStand_1)
- UseSkill(ThornThrust)
- UseSkill(RandomSpit combo=TableCommand)
- UseSkill(ComboAttack)
- UseSkill(JumpBack)
- UseSkill(HeadAttack)
- UseSkill(JumpBack)
