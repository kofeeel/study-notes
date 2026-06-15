# M_HedgeBoarBrute_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector
                │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_HedgeBoarBrute_Default)]
                │       └── ⚔️ UseSkill(PhaseChange)
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP>60.0%) && 🛡️CheckStance(M_HedgeBoarBrute_Default)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=30)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_HedgeBoarBrute_NoGuardCheck ON) && 🛡️DistanceToTarget(dist>=350.0)]
                │   │   └── ⚔️ UseSkill(ShockWave)
                │   ├── ❓ Selector
                │   │   └── ⚔️ UseSkill(SpinSwingLeft|SpinSwingRight)
                │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=350.0) && 🛡️UseableTime]
                │   │   └── ⚔️ UseSkill(DashSwing)
                │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=350.0) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_HedgeBoarBrute_ApproachCheck ON)]
                │   │   └── ⚔️ UseSkill(LeapCombo)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_HedgeBoarBrute_NoGuardCheck ON)]
                │   │   ├── ⚔️ UseSkill(StrikeGrab)
                │   │   └── ⚔️ UseSkill(SwingSmash|ShockWave)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   ├── ⚔️ UseSkill(SpinCombo) [🛡️UseableTime]
                │   │   └── ⚔️ UseSkill(SwingDouble|SwingCombo)
                │   ├── ➡️ Sequence
                │   │   └── ⚔️ UseSkill(Uppercut|Swing)
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_HedgeBoarBrute_Phase2)]
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                    │   ├── 📋 Blackboard(FirstShot)
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=30)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=30) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON)]
                    │   └── ⚔️ UseSkill(AreaExplosion)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_HedgeBoarBrute_ExplosionBuffFX ON) && 🛡️CheckActorEffect(NOT Self.M_HedgeBoarBrute_BuffSkillEndAttack ON)]
                    │   ├── ⚔️ UseSkill(ExplosionCombo)
                    │   └── ⚔️ UseSkill(RangeExplosion|LeapExplosionCombo)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(ExplosionBuffON)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️DistanceToTarget(dist>=350.0)]
                    │   └── ⚔️ UseSkill(ShockWaveChain)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_HedgeBoarBrute_ExplosionBuffFX ON)]
                    │   └── ⚔️ UseSkill(SpinSwingLeft|SpinSwingRight)
                    ├── ❓ Selector [🛡️DistanceToTarget(dist>=350.0) && 🛡️CheckActorEffect(NOT Self.M_HedgeBoarBrute_ExplosionBuffFX ON)]
                    │   └── ⚔️ UseSkill(DashSwing)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(StrikeGrab|SwingSmash|ShockWaveChain)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_HedgeBoarBrute_ExplosionBuffFX ON)]
                    │   ├── ⚔️ UseSkill(SmashCombo)
                    │   └── ⚔️ UseSkill(SwingDouble|SwingCombo)
                    ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_HedgeBoarBrute_ExplosionBuffFX ON)]
                    │   └── ⚔️ UseSkill(Uppercut|Swing)
                    └── ❓ Selector
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 26 |
| Selector | 25 |
| Dec/CheckActorEffect | 17 |
| Sequence | 9 |
| Dec/UseableTime | 6 |
| Dec/Random | 5 |
| Dec/DistanceToTarget | 5 |
| Dec/CheckActorStat | 4 |
| Task/UseableTimeReset | 4 |
| Dec/IsAlive | 3 |
| Dec/CheckStance | 3 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Dec/AimMe | 3 |
| Task/Wait | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/CautionToTarget | 1 |
| Dec/TimeLimit | 1 |
| Dec/AggroLevel | 1 |

## 스킬 목록
- UseSkill(PhaseChange)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(ShockWave)
- UseSkill(SpinSwingLeft|SpinSwingRight)
- UseSkill(DashSwing)
- UseSkill(LeapCombo)
- UseSkill(StrikeGrab)
- UseSkill(SwingSmash|ShockWave)
- UseSkill(SpinCombo)
- UseSkill(SwingDouble|SwingCombo)
- UseSkill(Uppercut|Swing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(AreaExplosion)
- UseSkill(ExplosionCombo)
- UseSkill(RangeExplosion|LeapExplosionCombo)
- UseSkill(ExplosionBuffON)
- UseSkill(ShockWaveChain)
- UseSkill(SpinSwingLeft|SpinSwingRight)
- UseSkill(DashSwing)
- UseSkill(StrikeGrab|SwingSmash|ShockWaveChain)
- UseSkill(SmashCombo)
- UseSkill(SwingDouble|SwingCombo)
- UseSkill(Uppercut|Swing)
