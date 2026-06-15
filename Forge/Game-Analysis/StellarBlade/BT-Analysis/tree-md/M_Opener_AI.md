# M_Opener_AI

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
            │           └── ⚔️ UseSkill(Stinger combo=TableCommand)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [DownCautionTimer1])]
            ├── ❓ Selector
            │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_Opener_Default) && 🛡️CheckActorEffect(NOT Self.M_Opener_PhaseChange ON)]
            │       ├── ⚔️ UseSkill(PhaseChange combo=TableCommand)
            │       └── ⚔️ UseSkill(AuraMine)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Self.M_Opener_PhaseChange ON)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(BB1)
            │   ├── ➡️ Sequence [🛡️CheckActorTag && 🛡️UseableTime]
            │   │   ├── ✨ UseEffect(['M_Opener_ResetCoolTime_Stamp'])
            │   │   └── ⚔️ UseSkill(StampSmall|StampLarge combo=TableCommand)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorStat(ActorStatType_HP>60.0%)]
            │       ├── ❓ Selector
            │       │   ├── ⚔️ UseSkill(Stinger) [🛡️DistanceToTarget(dist>=600.0)]
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Opener_CheckRun ON)]
            │       │   │   │   ├── ⚔️ UseSkill(MoveBack_2 combo=TableCommand) [🛡️Random(rand(100)<=70)]
            │       │   │   │   └── ⚔️ UseSkill(MoveBack combo=TableCommand) [🛡️DistanceToTarget(dist<=400.0)]
            │       │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [Timer1]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=1500.0)]
            │       │   ├── ❓ Selector [🛡️UseableTime && 🛡️DistanceToTarget(dist>=600.0)]
            │       │   │   └── ⚔️ UseSkill(StampAttack combo=TableCommand)
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(SwingDouble combo=TableCommand)
            │       │   │   └── ❓ Selector
            │       │   │       ├── ⚔️ UseSkill(Stinger|StampSmall combo=TableCommand) [🛡️Random(rand(100)<=80)]
            │       │   │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer2]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=1800.0)]
            │       │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer2]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️CheckActorEffect(NOT Self.M_Opener_CheckRun ON)]
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(SwingCombo_2)
            │       │       └── ⚔️ UseSkill(SwingDouble)
            │       ├── ❓ Selector
            │       │   ├── ⚔️ UseSkill(SwingDouble)
            │       │   └── ⚔️ UseSkill(Swing)
            │       └── ❓ Selector
            │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [WalkTimer1]) && 🛡️DistanceToTarget(dist<=500.0)]
            │           └── ➡️ Sequence
            │               ├── ✨ UseEffect(['M_Opener_CheckRun'])
            │               └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Opener_PhaseChange ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️CheckActorTag && 🛡️UseableTime]
                │   ├── ✨ UseEffect(['M_Opener_ResetCoolTime_Stamp'])
                │   └── ⚔️ UseSkill(StampSmall|StampLarge combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ⚔️ UseSkill(AuraMine combo=TableCommand) [🛡️DistanceToTarget(dist>=400.0)]
                    ├── ⚔️ UseSkill(Stinger|StampLarge combo=TableCommand) [🛡️DistanceToTarget(dist>=600.0)]
                    ├── ⚔️ UseSkill(LaserAura combo=TableCommand)
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(SwingChain_2 combo=TableCommand)
                    │   └── ❓ Selector
                    │       ├── ⚔️ UseSkill(StampLarge|LaserAura combo=TableCommand) [🛡️Random(rand(100)<=70)]
                    │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer2]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=1800.0)]
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(SwingCombo combo=TableCommand)
                    │   └── ⚔️ UseSkill(SwingTriple)
                    ├── ⚔️ UseSkill(Stinger) [🛡️DistanceToTarget(dist>=600.0)]
                    ├── ➡️ Sequence
                    │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Opener_CheckRun ON)]
                    │   │   ├── ⚔️ UseSkill(MoveBack_2 combo=TableCommand) [🛡️Random(rand(100)<=70)]
                    │   │   └── ⚔️ UseSkill(MoveBack combo=TableCommand) [🛡️DistanceToTarget(dist<=400.0)]
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [Timer1]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=1500.0)]
                    ├── ❓ Selector [🛡️UseableTime && 🛡️DistanceToTarget(dist>=600.0)]
                    │   └── ⚔️ UseSkill(StampAttack combo=TableCommand)
                    ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer2]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️CheckActorEffect(NOT Self.M_Opener_CheckRun ON)]
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(SwingTriple)
                    │   └── ⚔️ UseSkill(Swing)
                    └── ❓ Selector
                        ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [WalkTimer1]) && 🛡️DistanceToTarget(dist<=500.0)]
                        └── ➡️ Sequence
                            ├── ✨ UseEffect(['M_Opener_CheckRun'])
                            └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 33 |
| Dec/DistanceToTarget | 24 |
| Selector | 23 |
| Sequence | 19 |
| Dec/CheckActorEffect | 13 |
| Task/CautionToTarget | 9 |
| Dec/TimeLimit | 9 |
| Dec/Random | 9 |
| Task/UseableTimeReset | 6 |
| Dec/UseableTime | 6 |
| Dec/Blackboard | 5 |
| Task/Blackboard | 5 |
| Task/UseEffect | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Dec/AggroLevel | 3 |
| Dec/CheckActorStat | 3 |
| Dec/AimMe | 3 |
| Dec/CheckActorTag | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/CheckStance | 1 |

## 스킬 목록
- UseSkill(Stinger combo=TableCommand)
- UseSkill(PhaseChange combo=TableCommand)
- UseSkill(AuraMine)
- UseSkill(StampSmall|StampLarge combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Stinger)
- UseSkill(MoveBack_2 combo=TableCommand)
- UseSkill(MoveBack combo=TableCommand)
- UseSkill(StampAttack combo=TableCommand)
- UseSkill(SwingDouble combo=TableCommand)
- UseSkill(Stinger|StampSmall combo=TableCommand)
- UseSkill(SwingCombo_2)
- UseSkill(SwingDouble)
- UseSkill(SwingDouble)
- UseSkill(Swing)
- UseSkill(StampSmall|StampLarge combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(AuraMine combo=TableCommand)
- UseSkill(Stinger|StampLarge combo=TableCommand)
- UseSkill(LaserAura combo=TableCommand)
- UseSkill(SwingChain_2 combo=TableCommand)
- UseSkill(StampLarge|LaserAura combo=TableCommand)
- UseSkill(SwingCombo combo=TableCommand)
- UseSkill(SwingTriple)
- UseSkill(Stinger)
- UseSkill(MoveBack_2 combo=TableCommand)
- UseSkill(MoveBack combo=TableCommand)
- UseSkill(StampAttack combo=TableCommand)
- UseSkill(SwingTriple)
- UseSkill(Swing)
