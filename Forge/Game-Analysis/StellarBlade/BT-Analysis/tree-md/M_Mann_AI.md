# M_Mann_AI

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
            │       ├── ➡️ Sequence
            │       │   ├── 📋 Blackboard(BattleStart)
            │       │   ├── ⏳ Wait(0.1s)
            │       │   └── ⚔️ UseSkill(WideSlash)
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           ├── ⏳ Wait(0.1s)
            │           ├── 🚶 MoveToTarget
            │           └── ⚔️ UseSkill(WideSlash)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist<=1700.0)]
            │   │   └── 🚶 MoveToTarget
            │   └── ➡️ Sequence [🛡️DistanceToTarget(dist<=1600.0)]
            │       ├── ⏳ Wait(1.0s) [🛡️CheckActorEffect(Target.? ON)]
            │       └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [AbnormalTimer])]
            ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Mann_CounterKnockDown ON)]
            │   └── ⚔️ UseSkill(FastGetUp)
            ├── ➡️ Sequence [🛡️CheckActorEffect(Target.P_Eve_Beta_SwordAura2 ON) && 🛡️DistanceToTarget(dist>=300.0)]
            │   └── ⚔️ UseSkill(CounterSwordAura|CounterSwordAura2 combo=TableCommand)
            ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Mann_TestMode ON)]
            │   ├── 🚶 MoveToTarget
            │   └── ⏳ Wait(0.1s)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(M_Mann_Default) && 🛡️CheckActorEffect(Target.P_Eve_Beta_SwordAura2) && 🛡️CheckActorStat(ActorStatType_HP>75.0%)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(BB1)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   ├── ❓ Selector [🛡️UseableTime]
            │   │   └── ⚔️ UseSkill(JustParry)
            │   ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   ├── ⚔️ UseSkill(Smoke)
            │   │   └── ❓ Selector
            │   │       ├── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️AimMe]
            │   │       ├── ⚔️ UseSkill(MoveBackShotSmoke)
            │   │       └── ⚔️ UseSkill(DashAttackSmoke|LeapAttack_Chain)
            │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   ├── ❓ Selector [🛡️UseableTime]
            │   │   │   ├── ⚔️ UseSkill(MoveBackShot) [🛡️CheckActorEffect(Self.M_Mann_CheckMoveBack) && 🛡️CheckActorEffect(Self.M_Mann_CheckNoGuard)]
            │   │   │   └── ⚔️ UseSkill(MoveBack combo=TableCommand) [🛡️CheckActorEffect(Self.M_Mann_CheckMoveBack)]
            │   │   ├── ❓ Selector [🛡️UseableTime]
            │   │   │   └── ⚔️ UseSkill(MoveLeft|MoveRight combo=TableCommand)
            │   │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Mann_CheckCombo)]
            │   │   │   └── ⚔️ UseSkill(ComboStab|ComboShot|ComboSlash)
            │   │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Mann_CheckNoGuard) && 🛡️UseableTime]
            │   │   │   └── ⚔️ UseSkill(ThrowShot)
            │   │   ├── ❓ Selector
            │   │   │   ├── ⚔️ UseSkill(Swing)
            │   │   │   ├── ⚔️ UseSkill(SwingCombo)
            │   │   │   └── ⚔️ UseSkill(SwordCombo) [🛡️UseableTime]
            │   │   └── ❓ Selector [🛡️DistanceToTarget(dist>=600.0)]
            │   │       ├── ⚔️ UseSkill(DashAttack)
            │   │       ├── ⚔️ UseSkill(WideSlash)
            │   │       └── ⚔️ UseSkill(LeapAttack|ShotInPosition) [🛡️UseableTime]
            │   ├── ❓ Selector
            │   │   └── ⚔️ UseSkill(ShotDash)
            │   └── ❓ Selector
            │       └── 🚶 MoveToTarget
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(M_Mann_Phase2) && 🛡️CheckActorStat(ActorStatType_HP>45.0%) && 🛡️CheckActorEffect(Target.P_Eve_Beta_SwordAura2) && 🛡️CheckActorEffect(NOT Self.M_Mann_CounterKnockDown ON)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BB2)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(BB2)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
            │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Mann_CheckSpear ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   ├── ❓ Selector [🛡️UseableTime]
            │   │   │   └── ⚔️ UseSkill(BackTumbling|JustParry2)
            │   │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(Self.M_Mann_CheckCombo)]
            │   │   │   └── ⚔️ UseSkill(SwingStab|SwingShockWave combo=TableCommand)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(MoveBackMine)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(SpearSwing)
            │   │   ├── ⚔️ UseSkill(SurpriseAttack)
            │   │   ├── ❓ Selector [🛡️UseableTime]
            │   │   │   └── ⚔️ UseSkill(BackSwingShockwave combo=TableCommand)
            │   │   └── ❓ Selector
            │   │       └── ⚔️ UseSkill(ShotInPosition2)
            │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Mann_CheckHammer ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   ├── ❓ Selector [🛡️Blackboard(BattleStart2)]
            │   │   │   ├── ⚔️ UseSkill(LeapAttack2 combo=TableCommand)
            │   │   │   └── ➡️ Sequence
            │   │   │       └── 📋 Blackboard(BattleStart2)
            │   │   ├── ❓ Selector
            │   │   │   ├── ⚔️ UseSkill(DashSwing combo=TableCommand)
            │   │   │   └── ➡️ Sequence
            │   │   │       ├── ⚔️ UseSkill(MoveBack2)
            │   │   │       └── ⚔️ UseSkill(DashSwing combo=TableCommand)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(MoveBackMine)
            │   │   ├── ⚔️ UseSkill(Stamp)
            │   │   ├── ⚔️ UseSkill(Stamp2)
            │   │   ├── ⚔️ UseSkill(ComboSwing)
            │   │   ├── ❓ Selector [🛡️UseableTime]
            │   │   │   └── ⚔️ UseSkill(LeapAttack2 combo=TableCommand)
            │   │   └── ❓ Selector [🛡️UseableTime]
            │   │       └── ⚔️ UseSkill(ShotInPosition2)
            │   └── ❓ Selector
            │       └── 🚶 MoveToTarget
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(M_Mann_Phase3) && 🛡️CheckActorEffect(Target.P_Eve_Beta_SwordAura2) && 🛡️CheckActorEffect(NOT Self.M_Mann_CounterKnockDown ON)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BB3)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(BB3)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
            │   │   └── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
            │   ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   ├── ⚔️ UseSkill(Smoke2)
            │   │   └── ❓ Selector
            │   │       ├── ⚔️ UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand) [🛡️AimMe]
            │   │       ├── ⚔️ UseSkill(MoveBackMine_Chain|BackSwingShockwave_Chain)
            │   │       └── ⚔️ UseSkill(DashCombo_Chain|LeapAttack2_Chain|Grab)
            │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Mann_CheckSpear ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(JustParry2|BackTumbling)
            │   │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Mann_CheckCombo)]
            │   │   │   ├── ⚔️ UseSkill(SwingStab2|SwingShockWave2 combo=TableCommand)
            │   │   │   └── ⚔️ UseSkill(SwingStab|SwingShockWave combo=TableCommand)
            │   │   ├── ❓ Selector [🛡️UseableTime]
            │   │   │   ├── ⚔️ UseSkill(MoveBackMine combo=TableCommand)
            │   │   │   └── ⚔️ UseSkill(WideSlash2)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(SpearSwing)
            │   │   ├── ⚔️ UseSkill(SurpriseAttack)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(BackSwingShockwave combo=TableCommand)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(DashCombo combo=TableCommand)
            │   │   └── ❓ Selector
            │   │       └── ⚔️ UseSkill(ShotInPosition2)
            │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Mann_CheckHammer ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │   │   ├── ❓ Selector
            │   │   │   ├── ⚔️ UseSkill(DashSwing combo=TableCommand)
            │   │   │   └── ➡️ Sequence
            │   │   │       ├── ⚔️ UseSkill(MoveBack2)
            │   │   │       └── ⚔️ UseSkill(DashSwing combo=TableCommand)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(MoveBackMine)
            │   │   ├── ⚔️ UseSkill(Stamp)
            │   │   ├── ❓ Selector [🛡️UseableTime]
            │   │   │   ├── ⚔️ UseSkill(MoveBackMine combo=TableCommand)
            │   │   │   └── ⚔️ UseSkill(WideSlash2)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(ComboSwing)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(Stamp2)
            │   │   ├── ❓ Selector
            │   │   │   └── ⚔️ UseSkill(LeapAttack2 combo=TableCommand)
            │   │   └── ❓ Selector
            │   │       └── ⚔️ UseSkill(ShotInPosition2)
            │   └── ❓ Selector
            │       └── 🚶 MoveToTarget
            └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=45.0%) && 🛡️CheckStance(M_Mann_Phase2)]
                └── ⚔️ UseSkill(PhaseChange)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 71 |
| Selector | 54 |
| Dec/CheckActorEffect | 32 |
| Sequence | 22 |
| Dec/UseableTime | 15 |
| Task/UseableTimeReset | 11 |
| Dec/Blackboard | 8 |
| Task/Blackboard | 8 |
| Dec/Random | 7 |
| Task/Wait | 6 |
| Task/MoveToTarget | 6 |
| Dec/AimMe | 6 |
| Dec/AggroLevel | 4 |
| Dec/DistanceToTarget | 4 |
| Dec/CheckStance | 4 |
| Dec/CheckActorStat | 4 |
| Dec/IsAlive | 3 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/CautionToTarget | 1 |
| Dec/TimeLimit | 1 |

## 스킬 목록
- UseSkill(WideSlash)
- UseSkill(WideSlash)
- UseSkill(FastGetUp)
- UseSkill(CounterSwordAura|CounterSwordAura2 combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(JustParry)
- UseSkill(Smoke)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(MoveBackShotSmoke)
- UseSkill(DashAttackSmoke|LeapAttack_Chain)
- UseSkill(MoveBackShot)
- UseSkill(MoveBack combo=TableCommand)
- UseSkill(MoveLeft|MoveRight combo=TableCommand)
- UseSkill(ComboStab|ComboShot|ComboSlash)
- UseSkill(ThrowShot)
- UseSkill(Swing)
- UseSkill(SwingCombo)
- UseSkill(SwordCombo)
- UseSkill(DashAttack)
- UseSkill(WideSlash)
- UseSkill(LeapAttack|ShotInPosition)
- UseSkill(ShotDash)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(BackTumbling|JustParry2)
- UseSkill(SwingStab|SwingShockWave combo=TableCommand)
- UseSkill(MoveBackMine)
- UseSkill(SpearSwing)
- UseSkill(SurpriseAttack)
- UseSkill(BackSwingShockwave combo=TableCommand)
- UseSkill(ShotInPosition2)
- UseSkill(LeapAttack2 combo=TableCommand)
- UseSkill(DashSwing combo=TableCommand)
- UseSkill(MoveBack2)
- UseSkill(DashSwing combo=TableCommand)
- UseSkill(MoveBackMine)
- UseSkill(Stamp)
- UseSkill(Stamp2)
- UseSkill(ComboSwing)
- UseSkill(LeapAttack2 combo=TableCommand)
- UseSkill(ShotInPosition2)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(Smoke2)
- UseSkill(EvasionLeft2|EvasionRight2 combo=TableCommand)
- UseSkill(MoveBackMine_Chain|BackSwingShockwave_Chain)
- UseSkill(DashCombo_Chain|LeapAttack2_Chain|Grab)
- UseSkill(JustParry2|BackTumbling)
- UseSkill(SwingStab2|SwingShockWave2 combo=TableCommand)
- UseSkill(SwingStab|SwingShockWave combo=TableCommand)
- UseSkill(MoveBackMine combo=TableCommand)
- UseSkill(WideSlash2)
- UseSkill(SpearSwing)
- UseSkill(SurpriseAttack)
- UseSkill(BackSwingShockwave combo=TableCommand)
- UseSkill(DashCombo combo=TableCommand)
- UseSkill(ShotInPosition2)
- UseSkill(DashSwing combo=TableCommand)
- UseSkill(MoveBack2)
- UseSkill(DashSwing combo=TableCommand)
- UseSkill(MoveBackMine)
- UseSkill(Stamp)
- UseSkill(MoveBackMine combo=TableCommand)
- UseSkill(WideSlash2)
- UseSkill(ComboSwing)
- UseSkill(Stamp2)
- UseSkill(LeapAttack2 combo=TableCommand)
- UseSkill(ShotInPosition2)
- UseSkill(PhaseChange)
