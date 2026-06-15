# M_SawsharkWasteland_AI

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
            │           └── ⚔️ UseSkill(S07_LengthShockWaves2)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── 🎬 PlayShow [🛡️CheckActorEffect(Target.? ON)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Self.M_SawsharkWasteland_PhaseChange ON) && 🛡️CheckActorStat(ActorStatType_HP>60.0%)]
                │   ├── ➡️ Sequence [🛡️Blackboard(Timer1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(Timer1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   ├── ⚔️ UseSkill(S13_TurnB|S14_TurnL|S15_TurnR) [🛡️DistanceToTarget(dist>=250.0)]
                │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckShot ON)]
                │   │   ├── 📋 Blackboard(DistanceCheck)
                │   │   └── ⚔️ UseSkill(S07_LengthShockWaves2)
                │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                │   │   └── 📋 Blackboard(DistanceCheck)
                │   ├── ❓ Selector
                │   │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=75.0%) && 🛡️CheckActorEffect(NOT Target.? ON)]
                │   │   │   └── ⚔️ UseSkill(Guard)
                │   │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
                │   │       └── ⚔️ UseSkill(LS01_DoubleGrab)
                │   ├── ⚔️ UseSkill(SwingRampage) [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
                │   ├── ➡️ Sequence [🛡️TimeLimit(10.0s [TimerBack]) && 🛡️DistanceToTarget(dist>=150.0)]
                │   │   └── ⚔️ UseSkill(MoveBack)
                │   ├── ❓ Selector [🛡️CheckActorEffect(Target.Down)]
                │   │   ├── ⚔️ UseSkill(S12_DashGrinder) [🛡️DistanceToTarget(dist>=800.0)]
                │   │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=98.0%)]
                │   │       ├── ⚔️ UseSkill(SawSwing)
                │   │       └── ⚔️ UseSkill(S13_TurnB|S14_TurnL|S15_TurnR) [🛡️DistanceToTarget(dist>=250.0)]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(DoubleSwingSaw) [🛡️CheckActorStat(ActorStatType_HP<=90.0%) && 🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckBehindSkill ON)]
                │   │   └── ⚔️ UseSkill(SlashChain)
                │   ├── ⚔️ UseSkill(SwingSlash)
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(S11_CrazySwing)
                │   │   ├── ⚔️ UseSkill(CS01_ComboAttack) [🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
                │   │   └── ⚔️ UseSkill(S04_PentaSwing) [🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckBehindSkill ON)]
                │   ├── ❓ Selector
                │   │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckBehindSkill ON)]
                │   │   │   └── ⚔️ UseSkill(JumpGrab)
                │   │   └── ⚔️ UseSkill(RoundShot2) [🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckShot ON)]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(SwingClaw) [🛡️UseableTime]
                │   │   ├── ⚔️ UseSkill(S05_DashAndSwing)
                │   │   └── ⚔️ UseSkill(S02_DoubleSwing)
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer2]) && 🛡️DistanceToTarget(dist<=600.0)]
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer1]) && 🛡️DistanceToTarget(dist>=600.0)]
                │   └── ❓ Selector
                │       ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<=800.0) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(2.0s)]
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>800.0)]
                │           └── ⏳ Wait(0.5s)
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_SawsharkWasteland_PhaseChange ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(Timer2)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(Timer2)
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
                │   │   └── ⚔️ UseSkill(ClawShot)
                │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                │   │   └── 📋 Blackboard(DistanceCheck)
                │   ├── ⚔️ UseSkill(SwingRampage) [🛡️CheckActorStat(ActorStatType_HP<=30.0%)]
                │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=40.0%) && 🛡️CheckActorEffect(NOT Target.? ON)]
                │   │   └── ⚔️ UseSkill(Guard)
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(CS01_ComboAttack) [🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                │   │   └── ❓ Selector
                │   │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist<=600.0)]
                │   │       │   └── ⚔️ UseSkill(LS01_DoubleGrab)
                │   │       └── ➡️ Sequence
                │   │           └── ⚔️ UseSkill(ClawShot combo=TableCommand)
                │   ├── ❓ Selector [🛡️CheckActorEffect(Target.Down)]
                │   │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                │   │       ├── ⚔️ UseSkill(SawSwing)
                │   │       └── ⚔️ UseSkill(S13_TurnB|S14_TurnL|S15_TurnR) [🛡️DistanceToTarget(dist>=250.0)]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(SwingClaw) [🛡️DistanceToTarget(dist>=300.0)]
                │   │   └── ⚔️ UseSkill(S05_DashAndSwing)
                │   ├── ❓ Selector
                │   │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckBehindSkill ON)]
                │   │       └── ⚔️ UseSkill(JumpGrab)
                │   ├── ➡️ Sequence
                │   │   └── ⚔️ UseSkill(MoveBack) [🛡️DistanceToTarget(dist>=150.0)]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(SwingSlash)
                │   │   └── ⚔️ UseSkill(SlashChain)
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(S11_CrazySwing)
                │   │   └── ⚔️ UseSkill(S04_PentaSwing) [🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckBehindSkill ON)]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(DoubleSwingSaw) [🛡️CheckActorEffect(NOT Self.M_Sawshark_CheckBehindSkill ON)]
                │   │   └── ⚔️ UseSkill(S02_DoubleSwing)
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer2]) && 🛡️DistanceToTarget(dist<=600.0)]
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer1]) && 🛡️DistanceToTarget(dist>=600.0)]
                │   └── ❓ Selector
                │       ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<=800.0) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(2.0s)]
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>800.0)]
                │           └── ⏳ Wait(0.5s)
                └── ❓ Selector
                    └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_SawsharkWasteland_Default) && 🛡️CheckActorEffect(NOT Self.M_Sawshark_PhaseChange ON)]
                        ├── ⚔️ UseSkill(PhaseChange combo=TableCommand)
                        ├── ⚔️ UseSkill(S12_DashGrinder) [🛡️DistanceToTarget(dist>=800.0)]
                        └── ⚔️ UseSkill(ClawShot)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 48 |
| Selector | 30 |
| Dec/DistanceToTarget | 24 |
| Sequence | 22 |
| Dec/CheckActorEffect | 20 |
| Dec/CheckActorStat | 13 |
| Task/Blackboard | 9 |
| Dec/TimeLimit | 9 |
| Dec/Blackboard | 7 |
| Task/Wait | 5 |
| Dec/Random | 5 |
| Dec/AggroLevel | 4 |
| Task/CautionToTarget | 4 |
| Task/MoveToTarget | 4 |
| Dec/IsAlive | 3 |
| Task/UseableTimeReset | 3 |
| Dec/AimMe | 3 |
| Dec/UseableTime | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/PlayShow | 1 |
| Dec/CheckStance | 1 |

## 스킬 목록
- UseSkill(S07_LengthShockWaves2)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S13_TurnB|S14_TurnL|S15_TurnR)
- UseSkill(S07_LengthShockWaves2)
- UseSkill(Guard)
- UseSkill(LS01_DoubleGrab)
- UseSkill(SwingRampage)
- UseSkill(MoveBack)
- UseSkill(S12_DashGrinder)
- UseSkill(SawSwing)
- UseSkill(S13_TurnB|S14_TurnL|S15_TurnR)
- UseSkill(DoubleSwingSaw)
- UseSkill(SlashChain)
- UseSkill(SwingSlash)
- UseSkill(S11_CrazySwing)
- UseSkill(CS01_ComboAttack)
- UseSkill(S04_PentaSwing)
- UseSkill(JumpGrab)
- UseSkill(RoundShot2)
- UseSkill(SwingClaw)
- UseSkill(S05_DashAndSwing)
- UseSkill(S02_DoubleSwing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S13_TurnB|S14_TurnL|S15_TurnR)
- UseSkill(ClawShot)
- UseSkill(SwingRampage)
- UseSkill(Guard)
- UseSkill(CS01_ComboAttack)
- UseSkill(LS01_DoubleGrab)
- UseSkill(ClawShot combo=TableCommand)
- UseSkill(SawSwing)
- UseSkill(S13_TurnB|S14_TurnL|S15_TurnR)
- UseSkill(SwingClaw)
- UseSkill(S05_DashAndSwing)
- UseSkill(JumpGrab)
- UseSkill(MoveBack)
- UseSkill(SwingSlash)
- UseSkill(SlashChain)
- UseSkill(S11_CrazySwing)
- UseSkill(S04_PentaSwing)
- UseSkill(DoubleSwingSaw)
- UseSkill(S02_DoubleSwing)
- UseSkill(PhaseChange combo=TableCommand)
- UseSkill(S12_DashGrinder)
- UseSkill(ClawShot)
