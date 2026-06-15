# M_OpenerWasteland_AI

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
            │           ├── ⚔️ UseSkill(AuraMine combo=TableCommand)
            │           └── ⚔️ UseSkill(LaserAura combo=TableCommand)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [DownCautionTimer1])]
            ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_OpenerWasteland_Default) && 🛡️CheckActorEffect(NOT Self.M_Opener_PhaseChange ON)]
            │   ├── ⚔️ UseSkill(PhaseChange combo=TableCommand)
            │   ├── ⚔️ UseSkill(AuraMine2)
            │   └── ⚔️ UseSkill(AuraBeam)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Self.M_OpenerWasteland_PhaseChange ON)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(BB1)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorStat(ActorStatType_HP>60.0%)]
            │       ├── ⚔️ UseSkill(Grab combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=75.0%)]
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorStat(ActorStatType_HP<=85.0%)]
            │       │   ├── ⚔️ UseSkill(AuraMine combo=TableCommand)
            │       │   └── ⚔️ UseSkill(LaserAura combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️UseableTime]
            │       │   ├── ⚔️ UseSkill(MoveBack|MoveBack_2 combo=TableCommand)
            │       │   └── ❓ Selector
            │       │       ├── ⚔️ UseSkill(Dash combo=TableCommand)
            │       │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer2]) && 🛡️DistanceToTarget(dist>=400.0) && 🛡️DistanceToTarget(dist<=1800.0)]
            │       ├── ⚔️ UseSkill(LaserAura combo=TableCommand) [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorStat(ActorStatType_HP<85.0%)]
            │       ├── ❓ Selector
            │       │   ├── ⚔️ UseSkill(Stinger) [🛡️DistanceToTarget(dist<=1200.0)]
            │       │   └── ⚔️ UseSkill(StampLarge combo=TableCommand) [🛡️UseableTime && 🛡️DistanceToTarget(dist>=600.0)]
            │       ├── ❓ Selector
            │       │   ├── ⚔️ UseSkill(SwingCombo)
            │       │   └── ⚔️ UseSkill(SwingChain combo=TableCommand)
            │       ├── ⚔️ UseSkill(SwingTriple)
            │       └── ❓ Selector
            │           └── ➡️ Sequence
            │               ├── ✨ UseEffect(['M_Opener_CheckRun'])
            │               └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=500.0)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_OpenerWasteland_PhaseChange ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
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
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=1500.0)]
                │   │   ├── 📋 Blackboard(DistanceCheck1)
                │   │   └── ❓ Selector
                │   │       ├── ⚔️ UseSkill(Dash combo=TableCommand) [🛡️DistanceToTarget(dist<=1000.0)]
                │   │       └── ⚔️ UseSkill(LaserAura2)
                │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=1800.0) && 🛡️TimeLimit(-1.0s [TimerDistance])]
                │       └── 📋 Blackboard(DistanceCheck1)
                └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence [🛡️DistanceToTarget(dist<=800.0)]
                    │   ├── ⚔️ UseSkill(MoveBack combo=TableCommand)
                    │   └── ⚔️ UseSkill(AuraBeam) [🛡️DistanceToTarget(dist>=200.0)]
                    ├── ⚔️ UseSkill(Rush) [🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                    ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=65.0%)]
                    │   ├── ⚔️ UseSkill(Grab)
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer1]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=1500.0)]
                    ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0)]
                    │   ├── ⚔️ UseSkill(AuraMine2 combo=TableCommand)
                    │   └── ⚔️ UseSkill(LaserAura2)
                    ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer2]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️CheckActorEffect(NOT Self.M_Opener_CheckRun ON)]
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(Stinger) [🛡️DistanceToTarget(dist<=1200.0)]
                    │   └── ⚔️ UseSkill(ShockWave combo=TableCommand) [🛡️UseableTime && 🛡️DistanceToTarget(dist>=600.0)]
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(Dash combo=TableCommand) [🛡️TimeLimit(30.0s [DashTimer])]
                    │   └── ➡️ Sequence
                    │       ├── ❓ Selector [🛡️UseableTime]
                    │       │   ├── ⚔️ UseSkill(MoveBack_2 combo=TableCommand) [🛡️DistanceToTarget(dist<=400.0)]
                    │       │   └── ⚔️ UseSkill(MoveBack combo=TableCommand)
                    │       └── ❓ Selector
                    │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️DistanceToTarget(dist<=2500.0)]
                    │           └── ⚔️ UseSkill(Dash combo=TableCommand)
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(SwingCombo3)
                    │   └── ⚔️ UseSkill(SwingChain3)
                    ├── ⚔️ UseSkill(SwingTriple2)
                    └── ❓ Selector
                        └── ➡️ Sequence
                            ├── ✨ UseEffect(['M_Opener_CheckRun'])
                            └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 38 |
| Selector | 23 |
| Dec/DistanceToTarget | 23 |
| Sequence | 19 |
| Dec/CheckActorEffect | 10 |
| Dec/CheckActorStat | 8 |
| Task/Blackboard | 7 |
| Dec/TimeLimit | 7 |
| Dec/Blackboard | 6 |
| Task/CautionToTarget | 5 |
| Dec/Random | 5 |
| Task/UseableTimeReset | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Dec/AggroLevel | 3 |
| Dec/AimMe | 3 |
| Task/UseEffect | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/CheckStance | 1 |

## 스킬 목록
- UseSkill(AuraMine combo=TableCommand)
- UseSkill(LaserAura combo=TableCommand)
- UseSkill(PhaseChange combo=TableCommand)
- UseSkill(AuraMine2)
- UseSkill(AuraBeam)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Grab combo=TableCommand)
- UseSkill(AuraMine combo=TableCommand)
- UseSkill(LaserAura combo=TableCommand)
- UseSkill(MoveBack|MoveBack_2 combo=TableCommand)
- UseSkill(Dash combo=TableCommand)
- UseSkill(LaserAura combo=TableCommand)
- UseSkill(Stinger)
- UseSkill(StampLarge combo=TableCommand)
- UseSkill(SwingCombo)
- UseSkill(SwingChain combo=TableCommand)
- UseSkill(SwingTriple)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Dash combo=TableCommand)
- UseSkill(LaserAura2)
- UseSkill(MoveBack combo=TableCommand)
- UseSkill(AuraBeam)
- UseSkill(Rush)
- UseSkill(Grab)
- UseSkill(AuraMine2 combo=TableCommand)
- UseSkill(LaserAura2)
- UseSkill(Stinger)
- UseSkill(ShockWave combo=TableCommand)
- UseSkill(Dash combo=TableCommand)
- UseSkill(MoveBack_2 combo=TableCommand)
- UseSkill(MoveBack combo=TableCommand)
- UseSkill(Dash combo=TableCommand)
- UseSkill(SwingCombo3)
- UseSkill(SwingChain3)
- UseSkill(SwingTriple2)
