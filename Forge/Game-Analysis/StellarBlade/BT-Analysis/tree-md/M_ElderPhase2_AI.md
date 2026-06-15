# M_ElderPhase2_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   └── ➡️ Sequence [🛡️Blackboard(BattleStart)]
        │       ├── 📋 Blackboard(BattleStart)
        │       ├── 🚶 MoveToTarget [🛡️TimeLimit(8.0s [StartTimer1])]
        │       └── ⚔️ UseSkill(BlinkCombo combo=TableCommand)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [DownCautionTimer1])]
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
            │   ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_Phase2 ON) && 🛡️CheckActorEffect(Self.? ON)]
            │   │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
            │   │   │   └── ⚔️ UseSkill(KillRoutinePhase3 combo=TableCommand)
            │   │   └── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
            │   │       ├── ➡️ Sequence [🛡️Blackboard(AITimer)]
            │   │       │   ├── 🔄 UseableTimeReset
            │   │       │   └── 📋 Blackboard(AITimer)
            │   │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │       │   ├── 📋 Blackboard(FirstShot)
            │   │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=70)]
            │   │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   │       ├── ❓ Selector [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=2800.0)]
            │   │       │   └── ➡️ Sequence
            │   │       │       ├── 📋 Blackboard(DistanceCheck1)
            │   │       │       └── ❓ Selector
            │   │       │           ├── ⚔️ UseSkill(Explosion combo=TableCommand)
            │   │       │           ├── ⚔️ UseSkill(Shot combo=TableCommand)
            │   │       │           └── ⚔️ UseSkill(BlinkF2 combo=TableCommand)
            │   │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️TimeLimit(10.0s [Timer_FarRange2])]
            │   │       │   └── 📋 Blackboard(DistanceCheck1)
            │   │       ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
            │   │       │   ├── ❓ Selector
            │   │       │   │   ├── ⚔️ UseSkill(ShotAura combo=TableCommand) [🛡️DistanceToTarget(dist>=1800.0)]
            │   │       │   │   └── ⚔️ UseSkill(Explosion combo=TableCommand) [🛡️TimeLimit(20.0s [Timer_EX]) && 🛡️DistanceToTarget(dist>=2000.0)]
            │   │       │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck2) && 🛡️DistanceToTarget(dist>=2500.0)]
            │   │       │   │   ├── 📋 Blackboard(DistanceCheck2)
            │   │       │   │   └── ⚔️ UseSkill(BlinkF combo=TableCommand)
            │   │       │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️TimeLimit(10.0s [TimerRangeFar])]
            │   │       │       └── 📋 Blackboard(DistanceCheck2)
            │   │       ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
            │   │       │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer97]) && 🛡️DistanceToTarget(dist<=3000.0) && 🛡️DistanceToTarget(dist>=800.0)]
            │   │       │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=38.0%)]
            │   │       │   │   ├── ⚔️ UseSkill(MoveSideAttack combo=TableCommand)
            │   │       │   │   └── ❓ Selector [🛡️DistanceToTarget(dist>=600.0)]
            │   │       │   │       ├── ⚔️ UseSkill(DashStamp2 combo=TableCommand)
            │   │       │   │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=800.0)]
            │   │       │   ├── ⚔️ UseSkill(Shield) [🛡️CheckActorEffect(Self.M_ElderPhase2_HitResult ON) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_NoGuardCheck ON)]
            │   │       │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist<=600.0)]
            │   │       │   │   └── ⚔️ UseSkill(BlinkB combo=TableCommand)
            │   │       │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorStat(ActorStatType_HP>=40.0%)]
            │   │       │   │   ├── ⚔️ UseSkill(DashStamp combo=TableCommand)
            │   │       │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=800.0)]
            │   │       │   ├── ⚔️ UseSkill(Jump combo=TableCommand) [🛡️DistanceToTarget(dist>=1000.0)]
            │   │       │   ├── ❓ Selector
            │   │       │   │   ├── ➡️ Sequence
            │   │       │   │   │   ├── ⚔️ UseSkill(BlinkCenterShot combo=TableCommand) [🛡️DistanceToTarget(dist>=900.0)]
            │   │       │   │   │   └── ⏳ Wait(3.0s)
            │   │       │   │   └── ❓ Selector
            │   │       │   │       ├── ⚔️ UseSkill(WingSwing combo=TableCommand)
            │   │       │   │       └── ➡️ Sequence
            │   │       │   │           ├── ✨ UseEffect(['M_ElderPhase2_CautionCheck'])
            │   │       │   │           └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=2500.0) && 🛡️DistanceToTarget(dist>=800.0)]
            │   │       │   ├── ➡️ Sequence
            │   │       │   │   ├── ⚔️ UseSkill(BlinkR combo=TableCommand)
            │   │       │   │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=200.0) && 🛡️TimeLimit(2.8s [CautionTimer98]) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_CautionCheck ON)]
            │   │       │   ├── ⚔️ UseSkill(RushCombo combo=TableCommand)
            │   │       │   ├── ❓ Selector
            │   │       │   │   ├── ⚔️ UseSkill(SwingBack2 combo=TableCommand)
            │   │       │   │   ├── ⚔️ UseSkill(SwingBack1)
            │   │       │   │   └── ⚔️ UseSkill(CounterCombo combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_ElderPhase2_BehindCheck ON)]
            │   │       │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist<=600.0) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_BlinkCheck ON)]
            │   │       │   │   └── ⚔️ UseSkill(BlinkB)
            │   │       │   ├── ❓ Selector
            │   │       │   │   ├── ➡️ Sequence
            │   │       │   │   │   ├── ⚔️ UseSkill(Shot combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_ElderPhase2_ProjectileCheck ON) && 🛡️DistanceToTarget(dist>800.0)]
            │   │       │   │   │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist<=3000.0) && 🛡️DistanceToTarget(dist>=200.0) && 🛡️TimeLimit(2.5s [CautionTimer2])]
            │   │       │   │   └── ➡️ Sequence
            │   │       │   │       ├── ✨ UseEffect(['M_ElderPhase2_CautionCheck'])
            │   │       │   │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=800.0)]
            │   │       │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_ElderPhase2_NoGuardCheck ON)]
            │   │       │   │   ├── ⚔️ UseSkill(SwingBig combo=TableCommand)
            │   │       │   │   └── ⚔️ UseSkill(BlinkDash combo=TableCommand)
            │   │       │   ├── ⚔️ UseSkill(BlinkCombo combo=TableCommand) [🛡️CheckActorEffect(NOT Target.? ON)]
            │   │       │   └── ❓ Selector
            │   │       │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist<=600.0) && 🛡️DistanceToTarget(dist>=200.0) && 🛡️TimeLimit(2.8s [CautionTimer99])]
            │   │       │       ├── ⚔️ UseSkill(MoveBackSwing combo=TableCommand)
            │   │       │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [NearWalk]) && 🛡️DistanceToTarget(dist<=400.0) && 🛡️DistanceToTarget(dist>=100.0)]
            │   │       └── ❓ Selector
            │   │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1700.0) && 🛡️DistanceToTarget(dist>=300.0) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_CautionCheck ON)]
            │   │           └── 🚶 MoveToTarget
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorStat(ActorStatType_HP>60.0%) && 🛡️CheckActorEffect(Self.? ON)]
            │       ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
            │       │   ├── ⚔️ UseSkill(KillRoutinePhase2 combo=TableCommand)
            │       │   └── ❓ Selector
            │       │       ├── ⚔️ UseSkill(RushCombo combo=TableCommand)
            │       │       └── ⚔️ UseSkill(DashStamp combo=TableCommand) [🛡️DistanceToTarget(dist>=1000.0)]
            │       └── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
            │           ├── ➡️ Sequence [🛡️Blackboard(AITimer)]
            │           │   ├── 🔄 UseableTimeReset
            │           │   ├── 🔄 UseableTimeReset
            │           │   ├── 🔄 UseableTimeReset
            │           │   ├── 🔄 UseableTimeReset
            │           │   └── 📋 Blackboard(AITimer)
            │           ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │           │   ├── 📋 Blackboard(FirstShot)
            │           │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │           ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │           │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │           ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=70)]
            │           │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │           ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
            │           │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck2) && 🛡️DistanceToTarget(dist>=2500.0)]
            │           │   │   ├── 📋 Blackboard(DistanceCheck2)
            │           │   │   └── ⚔️ UseSkill(BlinkF combo=TableCommand)
            │           │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️TimeLimit(10.0s [TimerRangeFar])]
            │           │       └── 📋 Blackboard(DistanceCheck2)
            │           ├── ❓ Selector [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=2800.0)]
            │           │   └── ➡️ Sequence
            │           │       ├── 📋 Blackboard(DistanceCheck1)
            │           │       └── ❓ Selector
            │           │           ├── ⚔️ UseSkill(Explosion combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=85.0%)]
            │           │           ├── ⚔️ UseSkill(Shot combo=TableCommand)
            │           │           └── ⚔️ UseSkill(BlinkF2 combo=TableCommand)
            │           ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️TimeLimit(10.0s [Timer_FarRange2])]
            │           │   └── 📋 Blackboard(DistanceCheck1)
            │           ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
            │           │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=79.0%)]
            │           │   │   ├── ⚔️ UseSkill(RushCombo combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=65.0%)]
            │           │   │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist<=600.0)]
            │           │   │   │   └── ⚔️ UseSkill(BlinkB)
            │           │   │   └── ❓ Selector [🛡️DistanceToTarget(dist>=600.0)]
            │           │   │       ├── ⚔️ UseSkill(DashStamp combo=TableCommand)
            │           │   │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=800.0)]
            │           │   ├── ⚔️ UseSkill(Shield) [🛡️CheckActorEffect(Self.M_ElderPhase2_HitResult ON)]
            │           │   ├── ⚔️ UseSkill(Jump combo=TableCommand) [🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorStat(ActorStatType_HP<=92.0%)]
            │           │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=93.0%)]
            │           │   │   ├── ➡️ Sequence
            │           │   │   │   ├── ⚔️ UseSkill(BlinkCenterShot combo=TableCommand) [🛡️DistanceToTarget(dist>=800.0)]
            │           │   │   │   └── ⏳ Wait(3.0s)
            │           │   │   └── ❓ Selector
            │           │   │       ├── ⚔️ UseSkill(WingSwing combo=TableCommand)
            │           │   │       └── ➡️ Sequence
            │           │   │           ├── ✨ UseEffect(['M_ElderPhase2_CautionCheck'])
            │           │   │           └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=2500.0) && 🛡️DistanceToTarget(dist>=800.0)]
            │           │   ├── ➡️ Sequence
            │           │   │   ├── ⚔️ UseSkill(BlinkR)
            │           │   │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=200.0) && 🛡️TimeLimit(2.8s [CautionTimer98]) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_CautionCheck ON)]
            │           │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=95.0%)]
            │           │   │   ├── ⚔️ UseSkill(SwingBack1)
            │           │   │   └── ⚔️ UseSkill(CounterCombo combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=92.0%) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_BehindCheck ON)]
            │           │   ├── ➡️ Sequence [🛡️UseableTime && 🛡️DistanceToTarget(dist<=600.0) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_BlinkCheck ON)]
            │           │   │   └── ⚔️ UseSkill(BlinkB)
            │           │   ├── ❓ Selector
            │           │   │   ├── ➡️ Sequence
            │           │   │   │   ├── ⚔️ UseSkill(Shot combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=98.0%) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_ProjectileCheck ON) && 🛡️DistanceToTarget(dist>800.0)]
            │           │   │   │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist<=3000.0) && 🛡️DistanceToTarget(dist>=200.0) && 🛡️TimeLimit(2.5s [CautionTimer2])]
            │           │   │   └── ➡️ Sequence
            │           │   │       ├── ✨ UseEffect(['M_ElderPhase2_CautionCheck'])
            │           │   │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=800.0)]
            │           │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_NoGuardCheck ON)]
            │           │   │   ├── ⚔️ UseSkill(SwingBig combo=TableCommand)
            │           │   │   └── ⚔️ UseSkill(BlinkShortDash combo=TableCommand)
            │           │   ├── ⚔️ UseSkill(BlinkCombo combo=TableCommand) [🛡️CheckActorEffect(NOT Target.? ON)]
            │           │   ├── ⚔️ UseSkill(MoveBackSwing combo=TableCommand)
            │           │   └── ❓ Selector
            │           │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist<=600.0) && 🛡️DistanceToTarget(dist>=200.0) && 🛡️TimeLimit(2.8s [CautionTimer99])]
            │           │       ├── ⚔️ UseSkill(SwingDouble)
            │           │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [NearWalk]) && 🛡️DistanceToTarget(dist<=400.0) && 🛡️DistanceToTarget(dist>=100.0)]
            │           └── ❓ Selector
            │               ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1700.0) && 🛡️DistanceToTarget(dist>=300.0) && 🛡️CheckActorEffect(NOT Self.M_ElderPhase2_CautionCheck ON)]
            │               └── 🚶 MoveToTarget
            └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_ElderPhase2_Phase2)]
                ├── ⚔️ UseSkill(PhaseChange3)
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(BlinkB) [🛡️DistanceToTarget(dist<=600.0)]
                │   │   └── ⚔️ UseSkill(ShotAura)
                │   └── ⚔️ UseSkill(ShotAura) [🛡️DistanceToTarget(dist>=600.0)]
                └── ⚔️ UseSkill(Explosion combo=TableCommand) [🛡️DistanceToTarget(dist>=1800.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/DistanceToTarget | 63 |
| Task/UseSkill | 62 |
| Selector | 42 |
| Sequence | 34 |
| Dec/CheckActorEffect | 31 |
| Dec/TimeLimit | 25 |
| Task/CautionToTarget | 19 |
| Dec/CheckActorStat | 15 |
| Task/Blackboard | 13 |
| Dec/Blackboard | 9 |
| Dec/Random | 6 |
| Task/UseableTimeReset | 5 |
| Task/Wait | 4 |
| Dec/AimMe | 4 |
| Task/UseEffect | 4 |
| Dec/IsAlive | 3 |
| Dec/AggroLevel | 3 |
| Task/MoveToTarget | 3 |
| Dec/UseableTime | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/CheckStance | 1 |

## 스킬 목록
- UseSkill(BlinkCombo combo=TableCommand)
- UseSkill(KillRoutinePhase3 combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Explosion combo=TableCommand)
- UseSkill(Shot combo=TableCommand)
- UseSkill(BlinkF2 combo=TableCommand)
- UseSkill(ShotAura combo=TableCommand)
- UseSkill(Explosion combo=TableCommand)
- UseSkill(BlinkF combo=TableCommand)
- UseSkill(MoveSideAttack combo=TableCommand)
- UseSkill(DashStamp2 combo=TableCommand)
- UseSkill(Shield)
- UseSkill(BlinkB combo=TableCommand)
- UseSkill(DashStamp combo=TableCommand)
- UseSkill(Jump combo=TableCommand)
- UseSkill(BlinkCenterShot combo=TableCommand)
- UseSkill(WingSwing combo=TableCommand)
- UseSkill(BlinkR combo=TableCommand)
- UseSkill(RushCombo combo=TableCommand)
- UseSkill(SwingBack2 combo=TableCommand)
- UseSkill(SwingBack1)
- UseSkill(CounterCombo combo=TableCommand)
- UseSkill(BlinkB)
- UseSkill(Shot combo=TableCommand)
- UseSkill(SwingBig combo=TableCommand)
- UseSkill(BlinkDash combo=TableCommand)
- UseSkill(BlinkCombo combo=TableCommand)
- UseSkill(MoveBackSwing combo=TableCommand)
- UseSkill(KillRoutinePhase2 combo=TableCommand)
- UseSkill(RushCombo combo=TableCommand)
- UseSkill(DashStamp combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(BlinkF combo=TableCommand)
- UseSkill(Explosion combo=TableCommand)
- UseSkill(Shot combo=TableCommand)
- UseSkill(BlinkF2 combo=TableCommand)
- UseSkill(RushCombo combo=TableCommand)
- UseSkill(BlinkB)
- UseSkill(DashStamp combo=TableCommand)
- UseSkill(Shield)
- UseSkill(Jump combo=TableCommand)
- UseSkill(BlinkCenterShot combo=TableCommand)
- UseSkill(WingSwing combo=TableCommand)
- UseSkill(BlinkR)
- UseSkill(SwingBack1)
- UseSkill(CounterCombo combo=TableCommand)
- UseSkill(BlinkB)
- UseSkill(Shot combo=TableCommand)
- UseSkill(SwingBig combo=TableCommand)
- UseSkill(BlinkShortDash combo=TableCommand)
- UseSkill(BlinkCombo combo=TableCommand)
- UseSkill(MoveBackSwing combo=TableCommand)
- UseSkill(SwingDouble)
- UseSkill(PhaseChange3)
- UseSkill(BlinkB)
- UseSkill(ShotAura)
- UseSkill(ShotAura)
- UseSkill(Explosion combo=TableCommand)
