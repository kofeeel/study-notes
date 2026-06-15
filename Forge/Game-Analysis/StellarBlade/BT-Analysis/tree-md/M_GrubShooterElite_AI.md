# M_GrubShooterElite_AI

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
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(StartShot)]
                │   ├── ⚔️ UseSkill(SpitMassAttack_1)
                │   ├── 📋 Blackboard(StartShot)
                │   └── ⏳ WaitTimeRandom
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
                │   ├── ❓ Selector [🛡️Blackboard(Phase2Check) && 🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_KnockbackAttackCheck ON)]
                │   │   └── ⚔️ UseSkill(SpitDozenAttack_1 combo=TableCommand)
                │   └── ➡️ Sequence [🛡️Blackboard(ShotCount)]
                │       ├── ⚔️ UseSkill(SpitTrainAttack_1|SpitAttack_1|SpitMassAttack_1)
                │       ├── 📋 Blackboard(ShotCount)
                │       └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=80.0%) && 🛡️Blackboard(Phase2Check) && 🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_NoGuardCheck ON)]
                │   ├── 📋 Blackboard(ShotCount)
                │   ├── 📋 Blackboard(Phase2Check)
                │   ├── ⚔️ UseSkill(ShockWaveAttack)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️Blackboard(Phase2Check) && 🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_NoGuardCheck ON)]
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
                │           ├── ❓ Selector [🛡️Blackboard(Phase2Check) && 🛡️CheckActorEffect(NOT Self.M_GrubShooterElite_KnockbackAttackCheck ON)]
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
| Sequence | 16 |
| Task/UseSkill | 16 |
| Selector | 13 |
| Dec/Blackboard | 10 |
| Dec/CheckActorEffect | 10 |
| Task/Blackboard | 10 |
| Task/WaitTimeRandom | 7 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Task/UseableTimeReset | 3 |
| Dec/Random | 3 |
| Dec/DistanceToTarget | 3 |
| Dec/UseableTime | 3 |
| Dec/AggroLevel | 2 |
| Task/PlayShow | 2 |
| Dec/AimMe | 2 |
| Dec/CheckActorStat | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(SpitMassAttack_1)
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
