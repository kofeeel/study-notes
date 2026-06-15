# M_SkullJuggernaut_AI

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
            │           └── ⚔️ UseSkill(S15_ShockWaveChain)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── 🎬 PlayShow [🛡️CheckActorEffect(Target.? ON)]
            ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorEffect(Self.M_SkullJuggernaut_S08_PhaseChange) && 🛡️CheckStance(M_SkullJuggernaut_Default)]
            │   ├── ⚔️ UseSkill(S08_PhaseChange)
            │   └── ⚔️ UseSkill(S16_ShockWaveChain_2)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorStat(ActorStatType_HP>70.0%)]
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=40)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   ├── ❓ Selector
            │   │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=800.0)]
            │   │   │   ├── 📋 Blackboard(DistanceCheck)
            │   │   │   └── ❓ Selector
            │   │   │       └── ⚔️ UseSkill(S15_ShockWaveChain) [🛡️CheckActorEffect(Self.M_SkullJuggernaut_S08_PhaseChange)]
            │   │   └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_SkullJuggernaut_CheckRangeSkill ON) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
            │   │       └── 📋 Blackboard(DistanceCheck)
            │   ├── ⚔️ UseSkill(S10_TurnL|S11_TurnR|S12_TurnB)
            │   ├── ⚔️ UseSkill(S05_Push combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=98.0%)]
            │   ├── ⚔️ UseSkill(S13_MoveBack) [🛡️TimeLimit(-1.0s [Timer2])]
            │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
            │   │   └── ⚔️ UseSkill(CS01_ComboAttack)
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.Down ON) && 🛡️DistanceToTarget(dist>=150.0)]
            │   │   └── ⚔️ UseSkill(S04_SwingBig)
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(Target.M_SkullJuggernaut_CheckBehindSkill)]
            │   │   ├── ⚔️ UseSkill(S09_Grab)
            │   │   └── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │   ├── ❓ Selector
            │   │   └── ➡️ Sequence
            │   │       └── ⚔️ UseSkill(S07_DashSwing)
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [TimerWalk2]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=500.0)]
            │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.M_SkullJuggernaut_CheckBehindSkill ON)]
            │   │   └── ⚔️ UseSkill(S03_Swing)
            │   ├── ❓ Selector
            │   │   └── ⚔️ UseSkill(S01_Stamp)
            │   ├── 🚶 MoveToTarget [🛡️TimeLimit(4.0s [TimerWalk2]) && 🛡️DistanceToTarget(dist<=600.0)]
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [TimerWalk1])]
            │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [TimerWalk2]) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️DistanceToTarget(dist<=1000.0)]
            │   └── ❓ Selector
            │       ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<=400.0)]
            │       └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_SkullJuggernaut_S08_PhaseChange ON)]
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=40)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=40) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=800.0)]
                │   │   ├── 📋 Blackboard(DistanceCheck)
                │   │   └── ❓ Selector
                │   │       └── ⚔️ UseSkill(S16_ShockWaveChain_2) [🛡️CheckActorEffect(Self.M_SkullJuggernaut_S08_PhaseChange ON)]
                │   └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_SkullJuggernaut_CheckRangeSkill ON) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                │       └── 📋 Blackboard(DistanceCheck)
                ├── ⚔️ UseSkill(S10_TurnL|S11_TurnR|S12_TurnB)
                ├── ❓ Selector
                │   └── ⚔️ UseSkill(BodyAttack)
                ├── ⚔️ UseSkill(S13_MoveBack combo=TableCommand)
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.Down ON) && 🛡️DistanceToTarget(dist>=150.0)]
                │   └── ⚔️ UseSkill(S04_SwingBig)
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_SkullJuggernaut_CheckStamp ON)]
                │   ├── ⚔️ UseSkill(S06_ShockWave)
                │   └── ⚔️ UseSkill(S02_StampChain_2)
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   └── ⚔️ UseSkill(DashSwing2)
                │   └── ⚔️ UseSkill(StampHold)
                ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [TimerWalk2]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=500.0)]
                ├── ⚔️ UseSkill(SwingChain)
                ├── ❓ Selector
                │   ├── ⚔️ UseSkill(CS01_ComboAttack) [🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                │   └── ➡️ Sequence
                │       └── ⚔️ UseSkill(S03_Swing)
                ├── ➡️ Sequence [🛡️CheckActorEffect(Target.M_SkullJuggernaut_CheckBehindSkill)]
                │   ├── ⚔️ UseSkill(S09_Grab)
                │   └── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                ├── 🚶 MoveToTarget [🛡️TimeLimit(4.0s [TimerWalk2]) && 🛡️DistanceToTarget(dist<=600.0)]
                ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [TimerWalk1])]
                ├── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [TimerWalk2]) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️DistanceToTarget(dist<=1000.0)]
                └── ❓ Selector
                    ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<=400.0)]
                    └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 31 |
| Selector | 20 |
| Sequence | 20 |
| Dec/DistanceToTarget | 20 |
| Dec/CheckActorEffect | 16 |
| Dec/TimeLimit | 11 |
| Task/Blackboard | 7 |
| Dec/CheckActorStat | 6 |
| Task/CautionToTarget | 6 |
| Task/MoveToTarget | 6 |
| Dec/Blackboard | 5 |
| Dec/Random | 5 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Dec/AggroLevel | 3 |
| Task/PlayShow | 3 |
| Dec/AimMe | 3 |
| Dec/LastSkillHitResult | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/CheckStance | 1 |

## 스킬 목록
- UseSkill(S15_ShockWaveChain)
- UseSkill(S08_PhaseChange)
- UseSkill(S16_ShockWaveChain_2)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S15_ShockWaveChain)
- UseSkill(S10_TurnL|S11_TurnR|S12_TurnB)
- UseSkill(S05_Push combo=TableCommand)
- UseSkill(S13_MoveBack)
- UseSkill(CS01_ComboAttack)
- UseSkill(S04_SwingBig)
- UseSkill(S09_Grab)
- UseSkill(S07_DashSwing)
- UseSkill(S03_Swing)
- UseSkill(S01_Stamp)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S16_ShockWaveChain_2)
- UseSkill(S10_TurnL|S11_TurnR|S12_TurnB)
- UseSkill(BodyAttack)
- UseSkill(S13_MoveBack combo=TableCommand)
- UseSkill(S04_SwingBig)
- UseSkill(S06_ShockWave)
- UseSkill(S02_StampChain_2)
- UseSkill(DashSwing2)
- UseSkill(StampHold)
- UseSkill(SwingChain)
- UseSkill(CS01_ComboAttack)
- UseSkill(S03_Swing)
- UseSkill(S09_Grab)
