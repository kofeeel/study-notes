# M_Tachy_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   └── ➡️ Sequence [🛡️Blackboard(BattleStart)]
        │       ├── 📋 Blackboard(BattleStart)
        │       └── 🚶 MoveToTarget [🛡️TimeLimit(3.5s [StartTimer1])]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [DownCautionTimer1])]
            ├── ❓ Selector
            │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=45.0%) && 🛡️CheckStance(NOT M_Tachy_Phase3) && 🛡️CheckStance(NOT M_Tachy_Finish)]
            │   │   └── ⚔️ UseSkill(PhaseChange3 combo=TableCommand)
            │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=75.0%) && 🛡️CheckActorEffect(Self.M_Tachy_Phase1 ON) && 🛡️CheckStance(M_Tachy_Default)]
            │       ├── ⚔️ UseSkill(PhaseChange2)
            │       └── ❓ Selector
            │           ├── ⚔️ UseSkill(MoveBackNear) [🛡️DistanceToTarget(dist<=500.0)]
            │           └── ⚔️ UseSkill(SwordAura) [🛡️DistanceToTarget(dist>=500.0)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorStat(ActorStatType_HP>75.0%)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(BB1)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   ├── ⚔️ UseSkill(BlinkStageMiddle1 combo=TableCommand) [🛡️DistanceToTarget(dist<=500.0) && 🛡️CheckActorEffect(Self.M_Tachy_BlinkCheck)]
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(Blink2 combo=TableCommand) [🛡️CheckActorEffect(Self.M_Tachy_BlinkCheck)]
            │   │   └── ⚔️ UseSkill(DashSlash_2 combo=TableCommand)
            │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Tachy_BlinkCheck)]
            │   │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck2) && 🛡️DistanceToTarget(dist>=1800.0)]
            │   │   │   ├── 📋 Blackboard(DistanceCheck2)
            │   │   │   └── ⚔️ UseSkill(Blink|Blink2 combo=TableCommand)
            │   │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=1500.0) && 🛡️TimeLimit(-1.0s [TimerRangeFar])]
            │   │       └── 📋 Blackboard(DistanceCheck2)
            │   ├── ❓ Selector [🛡️TimeLimit(15.0s [Timer2]) && 🛡️CheckActorEffect(NOT Self.M_Tachy_CheckBackSkill ON)]
            │   │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer3]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=300.0)]
            │   │   └── ⚔️ UseSkill(MoveBackNear combo=TableCommand)
            │   ├── ❓ Selector
            │   │   ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON) && 🛡️CheckActorEffect(NOT Self.M_Tachy_CheckBehindSkill ON)]
            │   │   │   └── ⚔️ UseSkill(BackDashChain)
            │   │   ├── ➡️ Sequence
            │   │   │   ├── ⚔️ UseSkill(MoveLeft|MoveRight combo=TableCommand)
            │   │   │   └── ❓ Selector
            │   │   │       ├── ⚔️ UseSkill(SideWalkChain)
            │   │   │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=300.0)]
            │   │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.Down ON)]
            │   │   │   ├── ⚔️ UseSkill(DashSlash) [🛡️UseableTime]
            │   │   │   └── ⚔️ UseSkill(Dash) [🛡️DistanceToTarget(dist>=400.0)]
            │   │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_Tachy_BlinkCheck)]
            │   │       ├── ⚔️ UseSkill(Blink combo=TableCommand) [🛡️DistanceToTarget(dist>=600.0)]
            │   │       └── ⚔️ UseSkill(StrongSlash)
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=800.0) && 🛡️DistanceToTarget(dist>=250.0)]
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(MoveBackNear_2|Swing)
            │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=100.0)]
            │   └── ❓ Selector
            │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=300.0)]
            │       └── 🚶 MoveToTarget
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Tachy_Phase2 ON) && 🛡️CheckActorStat(ActorStatType_HP>45.0%)]
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Tachy_BlinkCheck)]
            │   │   ├── ⚔️ UseSkill(BlinkStageMiddle1 combo=TableCommand) [🛡️DistanceToTarget(dist<=500.0)]
            │   │   └── ⚔️ UseSkill(SwordAura) [🛡️DistanceToTarget(dist>=700.0) && 🛡️DistanceToTarget(dist<=2000.0)]
            │   ├── ❓ Selector
            │   │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck2) && 🛡️DistanceToTarget(dist>=1800.0)]
            │   │   │   ├── 📋 Blackboard(DistanceCheck2)
            │   │   │   └── ❓ Selector
            │   │   │       ├── ⚔️ UseSkill(ChainSpin_2)
            │   │   │       └── ⚔️ UseSkill(Blink combo=TableCommand) [🛡️CheckActorEffect(NOT Self.? ON)]
            │   │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=1500.0) && 🛡️TimeLimit(-1.0s [TimerRangeFar])]
            │   │       └── 📋 Blackboard(DistanceCheck2)
            │   ├── ⚔️ UseSkill(ParrySlash)
            │   ├── ❓ Selector
            │   │   ├── ❓ Selector
            │   │   │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=700.0)]
            │   │   │   │   ├── 📋 Blackboard(DistanceCheck1)
            │   │   │   │   └── ❓ Selector
            │   │   │   │       └── ⚔️ UseSkill(SwordAura|Dash) [🛡️DistanceToTarget(dist>=700.0) && 🛡️DistanceToTarget(dist<=2000.0)]
            │   │   │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=700.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
            │   │   │       └── 📋 Blackboard(DistanceCheck1)
            │   │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Tachy_CheckBehindSkill ON)]
            │   │       └── ⚔️ UseSkill(ChainSpin)
            │   ├── ❓ Selector [🛡️TimeLimit(10.0s [Timer2]) && 🛡️CheckActorEffect(NOT Self.M_Tachy_CheckBackSkill ON)]
            │   │   ├── ➡️ Sequence
            │   │   │   ├── ⚔️ UseSkill(MoveBackFar combo=TableCommand) [🛡️DistanceToTarget(dist<=400.0)]
            │   │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer3]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=300.0)]
            │   │   └── ➡️ Sequence
            │   │       ├── ⚔️ UseSkill(MoveBackNear combo=TableCommand)
            │   │       └── ❓ Selector [🛡️Random(rand(100)<=90)]
            │   │           └── ⚔️ UseSkill(MoveBackFar combo=TableCommand)
            │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.Down ON)]
            │   │   ├── ⚔️ UseSkill(DashSlash)
            │   │   └── ⚔️ UseSkill(Dash) [🛡️DistanceToTarget(dist>=400.0)]
            │   ├── ➡️ Sequence
            │   │   ├── ⚔️ UseSkill(MoveLeft|MoveRight combo=TableCommand)
            │   │   └── ❓ Selector
            │   │       ├── ⚔️ UseSkill(SideWalkChain)
            │   │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=300.0)]
            │   ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON) && 🛡️CheckActorEffect(NOT Self.M_Tachy_CheckBehindSkill ON)]
            │   │   └── ⚔️ UseSkill(BackDashChain)
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(SwingRushChain)
            │   │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
            │   │       ├── ⚔️ UseSkill(Blink combo=TableCommand) [🛡️DistanceToTarget(dist>=600.0)]
            │   │       └── ⚔️ UseSkill(StrongSlash)
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=800.0) && 🛡️DistanceToTarget(dist>=250.0)]
            │   ├── ❓ Selector
            │   │   ├── ⚔️ UseSkill(MoveBackNear_2|Swing)
            │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=100.0)]
            │   └── ❓ Selector
            │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=300.0)]
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Tachy_Phase3 ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Tachy_BlinkCheck)]
                │   ├── ⚔️ UseSkill(BlinkStageMiddle2 combo=TableCommand) [🛡️DistanceToTarget(dist<=500.0)]
                │   └── ❓ Selector
                │       ├── ⚔️ UseSkill(SwordAura) [🛡️DistanceToTarget(dist>=700.0) && 🛡️DistanceToTarget(dist<=2000.0)]
                │       └── ⚔️ UseSkill(FlyStamp_2) [🛡️DistanceToTarget(dist>700.0)]
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck2) && 🛡️DistanceToTarget(dist>=1800.0)]
                │   │   ├── 📋 Blackboard(DistanceCheck2)
                │   │   └── ❓ Selector
                │   │       ├── ⚔️ UseSkill(SwordAuraDash) [🛡️DistanceToTarget(dist>=700.0)]
                │   │       └── ⚔️ UseSkill(Blink combo=TableCommand) [🛡️CheckActorEffect(Self.M_Tachy_BlinkCheck)]
                │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=1500.0) && 🛡️TimeLimit(-1.0s [TimerRangeFar])]
                │       └── 📋 Blackboard(DistanceCheck2)
                ├── ⚔️ UseSkill(ParrySlash)
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=500.0)]
                │   │   ├── 📋 Blackboard(DistanceCheck1)
                │   │   └── ❓ Selector
                │   │       ├── ⚔️ UseSkill(FlyStamp) [🛡️CheckActorStat(ActorStatType_HP<=45.0%)]
                │   │       ├── ⚔️ UseSkill(SwordAuraDash) [🛡️DistanceToTarget(dist>=1000.0)]
                │   │       └── ⚔️ UseSkill(SwordAura) [🛡️DistanceToTarget(dist>=700.0) && 🛡️DistanceToTarget(dist<=2000.0)]
                │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=500.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                │       └── 📋 Blackboard(DistanceCheck1)
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Tachy_CheckBehindSkill ON)]
                │   ├── ⚔️ UseSkill(ComboAttack)
                │   └── ⚔️ UseSkill(SwingRushChain)
                ├── ❓ Selector
                │   └── ⚔️ UseSkill(ChainSpin)
                ├── ❓ Selector [🛡️TimeLimit(15.0s [Timer2]) && 🛡️CheckActorEffect(NOT Self.M_Tachy_CheckBackSkill ON)]
                │   ├── ⚔️ UseSkill(MoveBackFly combo=TableCommand) [🛡️DistanceToTarget(dist<=400.0)]
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer3]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=300.0)]
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(MoveBackNear combo=TableCommand)
                │       └── ❓ Selector [🛡️Random(rand(100)<=90) && 🛡️CheckActorEffect(Self.? ON)]
                │           └── ⚔️ UseSkill(MoveBackFly|MoveBackFar combo=TableCommand)
                ├── ❓ Selector
                │   ├── ⚔️ UseSkill(Grab) [🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer4]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=200.0)]
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(MoveLeft|MoveRight combo=TableCommand)
                │   └── ❓ Selector
                │       ├── ⚔️ UseSkill(SideWalkChain)
                │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=300.0)]
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.Down ON)]
                │   ├── ⚔️ UseSkill(Dash) [🛡️DistanceToTarget(dist>=600.0)]
                │   └── ⚔️ UseSkill(DashSlash) [🛡️DistanceToTarget(dist>=400.0)]
                ├── ❓ Selector
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON) && 🛡️CheckActorEffect(NOT Self.M_Tachy_CheckBehindSkill ON)]
                │   │   └── ⚔️ UseSkill(BackDashChain)
                │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                │       ├── ⚔️ UseSkill(Blink combo=TableCommand) [🛡️DistanceToTarget(dist>=600.0)]
                │       └── ⚔️ UseSkill(StrongSlash)
                ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=800.0) && 🛡️DistanceToTarget(dist>=250.0)]
                ├── ❓ Selector
                │   ├── ⚔️ UseSkill(MoveBackNear_2|Swing)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=100.0)]
                └── ❓ Selector
                    ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=300.0)]
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 69 |
| Dec/DistanceToTarget | 67 |
| Selector | 53 |
| Dec/CheckActorEffect | 33 |
| Sequence | 32 |
| Dec/TimeLimit | 26 |
| Task/Blackboard | 18 |
| Task/CautionToTarget | 17 |
| Dec/Blackboard | 13 |
| Dec/Random | 11 |
| Dec/CheckActorStat | 7 |
| Dec/AimMe | 6 |
| Dec/AggroLevel | 4 |
| Task/MoveToTarget | 4 |
| Dec/IsAlive | 3 |
| Dec/CheckStance | 3 |
| Task/Wait | 2 |
| Task/UseableTimeReset | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/UseableTime | 1 |

## 스킬 목록
- UseSkill(PhaseChange3 combo=TableCommand)
- UseSkill(PhaseChange2)
- UseSkill(MoveBackNear)
- UseSkill(SwordAura)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(BlinkStageMiddle1 combo=TableCommand)
- UseSkill(Blink2 combo=TableCommand)
- UseSkill(DashSlash_2 combo=TableCommand)
- UseSkill(Blink|Blink2 combo=TableCommand)
- UseSkill(MoveBackNear combo=TableCommand)
- UseSkill(BackDashChain)
- UseSkill(MoveLeft|MoveRight combo=TableCommand)
- UseSkill(SideWalkChain)
- UseSkill(DashSlash)
- UseSkill(Dash)
- UseSkill(Blink combo=TableCommand)
- UseSkill(StrongSlash)
- UseSkill(MoveBackNear_2|Swing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(BlinkStageMiddle1 combo=TableCommand)
- UseSkill(SwordAura)
- UseSkill(ChainSpin_2)
- UseSkill(Blink combo=TableCommand)
- UseSkill(ParrySlash)
- UseSkill(SwordAura|Dash)
- UseSkill(ChainSpin)
- UseSkill(MoveBackFar combo=TableCommand)
- UseSkill(MoveBackNear combo=TableCommand)
- UseSkill(MoveBackFar combo=TableCommand)
- UseSkill(DashSlash)
- UseSkill(Dash)
- UseSkill(MoveLeft|MoveRight combo=TableCommand)
- UseSkill(SideWalkChain)
- UseSkill(BackDashChain)
- UseSkill(SwingRushChain)
- UseSkill(Blink combo=TableCommand)
- UseSkill(StrongSlash)
- UseSkill(MoveBackNear_2|Swing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(BlinkStageMiddle2 combo=TableCommand)
- UseSkill(SwordAura)
- UseSkill(FlyStamp_2)
- UseSkill(SwordAuraDash)
- UseSkill(Blink combo=TableCommand)
- UseSkill(ParrySlash)
- UseSkill(FlyStamp)
- UseSkill(SwordAuraDash)
- UseSkill(SwordAura)
- UseSkill(ComboAttack)
- UseSkill(SwingRushChain)
- UseSkill(ChainSpin)
- UseSkill(MoveBackFly combo=TableCommand)
- UseSkill(MoveBackNear combo=TableCommand)
- UseSkill(MoveBackFly|MoveBackFar combo=TableCommand)
- UseSkill(Grab)
- UseSkill(MoveLeft|MoveRight combo=TableCommand)
- UseSkill(SideWalkChain)
- UseSkill(Dash)
- UseSkill(DashSlash)
- UseSkill(BackDashChain)
- UseSkill(Blink combo=TableCommand)
- UseSkill(StrongSlash)
- UseSkill(MoveBackNear_2|Swing)
