# M_Sawshark_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️Blackboard(BattleStart)]
            │       └── ➡️ Sequence
            │           ├── ⏳ Wait(0.1s)
            │           ├── 📋 Blackboard(BattleStart)
            │           └── ⚔️ UseSkill(S07_LengthShockWaves)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── 🎬 PlayShow [🛡️CheckActorEffect(Target.? ON)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorStat(ActorStatType_HP>60.0%) && 🛡️CheckActorEffect(NOT Self.M_Sawshark_PhaseChange ON)]
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   ├── ⚔️ UseSkill(S13_TurnB|S14_TurnL|S15_TurnR) [🛡️DistanceToTarget(dist>=250.0)]
            │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=1000.0)]
            │   │   ├── 📋 Blackboard(DistanceCheck)
            │   │   └── ⚔️ UseSkill(S07_LengthShockWaves)
            │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
            │   │   └── 📋 Blackboard(DistanceCheck)
            │   ├── ❓ Selector
            │   │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=78.0%) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   │   └── ⚔️ UseSkill(Guard)
            │   │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
            │   │       └── ⚔️ UseSkill(LS01_DoubleGrab)
            │   ├── ➡️ Sequence
            │   │   └── ⚔️ UseSkill(S10_MoveBack) [🛡️DistanceToTarget(dist>=150.0)]
            │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(Target.Down)]
            │   │   └── ⚔️ UseSkill(S12_DashGrinder)
            │   ├── ⚔️ UseSkill(Jump) [🛡️CheckActorStat(ActorStatType_HP<=85.0%)]
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(SwingSlash) [🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
            │   │   └── ⚔️ UseSkill(S11_CrazySwing|S04_PentaSwing)
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(S05_DashAndSwing)
            │   │   └── ⚔️ UseSkill(S02_DoubleSwing|S01_RightSwing)
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer2]) && 🛡️DistanceToTarget(dist<=600.0)]
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer1]) && 🛡️DistanceToTarget(dist>=600.0)]
            │   └── ❓ Selector
            │       ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<=800.0) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(2.0s)]
            │       └── ➡️ Sequence
            │           ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>800.0)]
            │           └── ⏳ Wait(0.5s)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Sawshark_PhaseChange ON)]
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   ├── ⚔️ UseSkill(S13_TurnB|S14_TurnL|S15_TurnR) [🛡️DistanceToTarget(dist>=250.0)]
            │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckShot ON)]
            │   │   ├── 📋 Blackboard(DistanceCheck)
            │   │   └── ⚔️ UseSkill(S07_LengthShockWaves)
            │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
            │   │   └── 📋 Blackboard(DistanceCheck)
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(CS01_ComboAttack)
            │   │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   │   └── ⚔️ UseSkill(Guard)
            │   │   └── ➡️ Sequence
            │   │       └── ⚔️ UseSkill(LS01_DoubleGrab)
            │   ├── ⚔️ UseSkill(SwingRampage) [🛡️CheckActorStat(ActorStatType_HP<=45.0%)]
            │   ├── ➡️ Sequence
            │   │   └── ⚔️ UseSkill(S10_MoveBack) [🛡️DistanceToTarget(dist>=150.0)]
            │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(Target.Down)]
            │   │   └── ⚔️ UseSkill(S12_DashGrinder)
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(RoundShot) [🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckShot ON)]
            │   │   └── ⚔️ UseSkill(Jump)
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(SwingSlash)
            │   │   └── ⚔️ UseSkill(S11_CrazySwing|S04_PentaSwing)
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(DoubleSwingSaw)
            │   │   ├── ⚔️ UseSkill(S05_DashAndSwing)
            │   │   └── ⚔️ UseSkill(S02_DoubleSwing)
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer2]) && 🛡️DistanceToTarget(dist<=600.0)]
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer1]) && 🛡️DistanceToTarget(dist>=600.0)]
            │   └── ❓ Selector
            │       ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<=800.0) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(2.0s)]
            │       └── ➡️ Sequence
            │           ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>800.0)]
            │           └── ⏳ Wait(0.5s)
            └── ❓ Selector
                └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_Sawshark_Default) && 🛡️CheckActorEffect(NOT Self.M_Sawshark_PhaseChange ON)]
                    ├── ⚔️ UseSkill(PhaseChange combo=TableCommand)
                    ├── ⚔️ UseSkill(S12_DashGrinder) [🛡️DistanceToTarget(dist>=800.0)]
                    └── ⚔️ UseSkill(RoundShot)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 36 |
| Selector | 22 |
| Dec/DistanceToTarget | 21 |
| Sequence | 18 |
| Dec/CheckActorEffect | 13 |
| Dec/CheckActorStat | 9 |
| Dec/TimeLimit | 8 |
| Task/Blackboard | 7 |
| Dec/Random | 6 |
| Task/Wait | 5 |
| Dec/Blackboard | 5 |
| Dec/AimMe | 4 |
| Task/CautionToTarget | 4 |
| Task/MoveToTarget | 4 |
| Dec/IsAlive | 3 |
| Dec/AggroLevel | 3 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/PlayShow | 1 |
| Dec/CheckStance | 1 |

## 스킬 목록
- UseSkill(S07_LengthShockWaves)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S13_TurnB|S14_TurnL|S15_TurnR)
- UseSkill(S07_LengthShockWaves)
- UseSkill(Guard)
- UseSkill(LS01_DoubleGrab)
- UseSkill(S10_MoveBack)
- UseSkill(S12_DashGrinder)
- UseSkill(Jump)
- UseSkill(SwingSlash)
- UseSkill(S11_CrazySwing|S04_PentaSwing)
- UseSkill(S05_DashAndSwing)
- UseSkill(S02_DoubleSwing|S01_RightSwing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S13_TurnB|S14_TurnL|S15_TurnR)
- UseSkill(S07_LengthShockWaves)
- UseSkill(CS01_ComboAttack)
- UseSkill(Guard)
- UseSkill(LS01_DoubleGrab)
- UseSkill(SwingRampage)
- UseSkill(S10_MoveBack)
- UseSkill(S12_DashGrinder)
- UseSkill(RoundShot)
- UseSkill(Jump)
- UseSkill(SwingSlash)
- UseSkill(S11_CrazySwing|S04_PentaSwing)
- UseSkill(DoubleSwingSaw)
- UseSkill(S05_DashAndSwing)
- UseSkill(S02_DoubleSwing)
- UseSkill(PhaseChange combo=TableCommand)
- UseSkill(S12_DashGrinder)
- UseSkill(RoundShot)
