# M_GorillaBBrokenHead_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   └── ➡️ Sequence [🛡️Blackboard(BattleStart)]
        │       ├── 📋 Blackboard(BattleStart)
        │       └── ⚔️ UseSkill(BodySlam)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [DownCautionTimer1])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_GorillaBBrokenHead_Phase2 ON)]
                │   └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                │       ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck)]
                │       │   ├── 🔄 UseableTimeReset
                │       │   └── 📋 Blackboard(DistanceCheck)
                │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │       │   ├── 📋 Blackboard(FirstShot)
                │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │       ├── ❓ Selector [🛡️UseableTime]
                │       │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_GorillaB_SkillCheck2 ON) && 🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=1300.0)]
                │       │   │   ├── 📋 Blackboard(DistanceCheck)
                │       │   │   └── ❓ Selector
                │       │   │       ├── ⚔️ UseSkill(S15_JumpAttack)
                │       │   │       └── ⚔️ UseSkill(S18_ThrowStone2)
                │       │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=500.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                │       │       └── 📋 Blackboard(DistanceCheck)
                │       ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_GorillaB_SkillCheck2 ON)]
                │       │   ├── ❓ Selector
                │       │   │   ├── ⚔️ UseSkill(ComboParry_ComboAttack2)
                │       │   │   └── ⚔️ UseSkill(ComboParry_ComboAttack)
                │       │   └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_GorillaB_SkillCheck2 ON)]
                │       │       ├── ⚔️ UseSkill(S14_SmashCombo)
                │       │       └── ✨ UseEffect(['M_GorillaB_SkillCheck2'])
                │       ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_GorillaB_SkillCheck2 ON)]
                │       │   ├── ➡️ Sequence
                │       │   │   ├── ⚔️ UseSkill(FistRush)
                │       │   │   └── ✨ UseEffect(['M_GorillaB_SkillCheck2'])
                │       │   └── ➡️ Sequence
                │       │       ├── ⚔️ UseSkill(Quake2) [🛡️DistanceToTarget(dist<=800.0)]
                │       │       └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [Timer1]) && 🛡️DistanceToTarget(dist>=1500.0)]
                │       ├── ❓ Selector
                │       │   └── ⚔️ UseSkill(S08_TurnAttackL|S09_TurnAttackR)
                │       ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_GorillaB_SkillCheck2 ON)]
                │       │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=250.0)]
                │       │   │   ├── ⚔️ UseSkill(BodyCheck)
                │       │   │   └── ✨ UseEffect(['M_GorillaB_SkillCheck2'])
                │       │   └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_GorillaB_SkillCheck2 ON)]
                │       │       ├── ⚔️ UseSkill(StampCombo)
                │       │       └── ✨ UseEffect(['M_GorillaB_SkillCheck2'])
                │       ├── ❓ Selector
                │       │   ├── ⚔️ UseSkill(MoveBackDust)
                │       │   └── ➡️ Sequence
                │       │       ├── ⚔️ UseSkill(MoveBack)
                │       │       └── ❓ Selector
                │       │           ├── ⚔️ UseSkill(S15_JumpAttack|S18_ThrowStone2|S07_ZigzagMoveAttack) [🛡️DistanceToTarget(dist>=600.0)]
                │       │           └── ✨ UseEffect(['M_GorillaB_SkillCheck'])
                │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON)]
                │       ├── ❓ Selector
                │       │   ├── ⚔️ UseSkill(BodySlam) [🛡️DistanceToTarget(dist>=600.0)]
                │       │   └── ➡️ Sequence
                │       │       ├── ⚔️ UseSkill(SwingCombo)
                │       │       └── ✨ UseEffect(['M_GorillaB_SkillCheck2'])
                │       ├── ➡️ Sequence
                │       │   ├── ❓ Selector
                │       │   │   ├── ⚔️ UseSkill(S03_ShortMoveLeft combo=TableCommand)
                │       │   │   └── ⚔️ UseSkill(S06_PunchAndLongMoveRight)
                │       │   ├── ⏳ Wait(0.1s)
                │       │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.7s [Timer2]) && 🛡️DistanceToTarget(dist>=1500.0)]
                │       ├── ❓ Selector
                │       │   ├── ⚔️ UseSkill(S21_RakeAttack)
                │       │   ├── ⚔️ UseSkill(S07_ZigzagMoveAttack) [🛡️DistanceToTarget(dist>=400.0)]
                │       │   └── ⚔️ UseSkill(S17_LeftBlowCombo)
                │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON)]
                │       └── ➡️ Sequence
                │           ├── ✨ UseEffect(['M_GorillaB_CheckRun'])
                │           └── ❓ Selector
                │               ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [TimerWalk]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=250.0)]
                │               └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=500.0)]
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorStat(ActorStatType_HP>60.0%)]
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck)]
                │       │   ├── 🔄 UseableTimeReset
                │       │   ├── 🔄 UseableTimeReset
                │       │   ├── 🔄 UseableTimeReset
                │       │   ├── 🔄 UseableTimeReset
                │       │   ├── 🔄 UseableTimeReset
                │       │   └── 📋 Blackboard(DistanceCheck)
                │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │       │   ├── 📋 Blackboard(FirstShot)
                │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │       ├── ❓ Selector [🛡️UseableTime]
                │       │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_GorillaB_SkillCheck ON) && 🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=1300.0)]
                │       │   │   ├── 📋 Blackboard(DistanceCheck)
                │       │   │   └── ❓ Selector
                │       │   │       ├── ⚔️ UseSkill(S15_JumpAttack) [🛡️CheckActorStat(ActorStatType_HP<=95.0%)]
                │       │   │       └── ⚔️ UseSkill(S18_ThrowStone)
                │       │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=500.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                │       │       └── 📋 Blackboard(DistanceCheck)
                │       ├── ❓ Selector
                │       │   ├── ⚔️ UseSkill(BodySlam) [🛡️DistanceToTarget(dist<=800.0)]
                │       │   ├── ➡️ Sequence
                │       │   │   ├── ⚔️ UseSkill(S14_SmashCombo)
                │       │   │   └── ✨ UseEffect(['M_GorillaB_SkillCheck'])
                │       │   └── ❓ Selector
                │       │       ├── ⚔️ UseSkill(ComboParry_ComboAttack2) [🛡️UseableTime]
                │       │       └── ⚔️ UseSkill(ComboParry_ComboAttack)
                │       ├── ❓ Selector
                │       │   ├── ⚔️ UseSkill(FistRush) [🛡️CheckActorStat(ActorStatType_MaxHP<=80.0%)]
                │       │   └── ➡️ Sequence
                │       │       ├── ⚔️ UseSkill(Quake2) [🛡️DistanceToTarget(dist<=800.0)]
                │       │       └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [Timer1]) && 🛡️DistanceToTarget(dist>=1500.0)]
                │       ├── ❓ Selector
                │       │   ├── ⚔️ UseSkill(S21_RakeAttack)
                │       │   └── ⚔️ UseSkill(S08_TurnAttackL|S09_TurnAttackR)
                │       ├── ❓ Selector
                │       │   ├── ⚔️ UseSkill(MoveBackDust) [🛡️UseableTime]
                │       │   └── ➡️ Sequence
                │       │       ├── ⚔️ UseSkill(MoveBack combo=TableCommand) [🛡️UseableTime]
                │       │       ├── ⚔️ UseSkill(S15_JumpAttack|S18_ThrowStone|S07_ZigzagMoveAttack) [🛡️DistanceToTarget(dist>=600.0)]
                │       │       └── ✨ UseEffect(['M_GorillaB_SkillCheck'])
                │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON)]
                │       ├── ➡️ Sequence
                │       │   ├── ❓ Selector
                │       │   │   ├── ⚔️ UseSkill(S03_ShortMoveLeft)
                │       │   │   └── ⚔️ UseSkill(S06_PunchAndLongMoveRight)
                │       │   ├── ⏳ Wait(0.1s)
                │       │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.7s [Timer2]) && 🛡️DistanceToTarget(dist>=1500.0)]
                │       ├── ❓ Selector
                │       │   └── ➡️ Sequence
                │       │       ├── ⚔️ UseSkill(SwingCombo) [🛡️UseableTime]
                │       │       └── ✨ UseEffect(['M_GorillaB_SkillCheck'])
                │       ├── ❓ Selector
                │       │   ├── ⚔️ UseSkill(S07_ZigzagMoveAttack) [🛡️DistanceToTarget(dist>=400.0)]
                │       │   └── ⚔️ UseSkill(S17_LeftBlowCombo)
                │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON)]
                │       └── ➡️ Sequence
                │           ├── ✨ UseEffect(['M_GorillaB_CheckRun'])
                │           └── ❓ Selector
                │               ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [TimerWalk]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=250.0)]
                │               └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=500.0)]
                └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_Phase2 ON)]
                    ├── ⚔️ UseSkill(S24_PhaseChange2)
                    └── ⚔️ UseSkill(BodyCheck)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 46 |
| Selector | 35 |
| Sequence | 28 |
| Dec/DistanceToTarget | 27 |
| Dec/CheckActorEffect | 19 |
| Dec/TimeLimit | 13 |
| Task/CautionToTarget | 11 |
| Task/UseEffect | 11 |
| Task/Blackboard | 9 |
| Dec/Blackboard | 7 |
| Task/UseableTimeReset | 6 |
| Dec/UseableTime | 6 |
| Dec/Random | 5 |
| Dec/CheckActorStat | 5 |
| Dec/AggroLevel | 4 |
| Task/Wait | 4 |
| Dec/IsAlive | 3 |
| Dec/AimMe | 3 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |

## 스킬 목록
- UseSkill(BodySlam)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S15_JumpAttack)
- UseSkill(S18_ThrowStone2)
- UseSkill(ComboParry_ComboAttack2)
- UseSkill(ComboParry_ComboAttack)
- UseSkill(S14_SmashCombo)
- UseSkill(FistRush)
- UseSkill(Quake2)
- UseSkill(S08_TurnAttackL|S09_TurnAttackR)
- UseSkill(BodyCheck)
- UseSkill(StampCombo)
- UseSkill(MoveBackDust)
- UseSkill(MoveBack)
- UseSkill(S15_JumpAttack|S18_ThrowStone2|S07_ZigzagMoveAttack)
- UseSkill(BodySlam)
- UseSkill(SwingCombo)
- UseSkill(S03_ShortMoveLeft combo=TableCommand)
- UseSkill(S06_PunchAndLongMoveRight)
- UseSkill(S21_RakeAttack)
- UseSkill(S07_ZigzagMoveAttack)
- UseSkill(S17_LeftBlowCombo)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S15_JumpAttack)
- UseSkill(S18_ThrowStone)
- UseSkill(BodySlam)
- UseSkill(S14_SmashCombo)
- UseSkill(ComboParry_ComboAttack2)
- UseSkill(ComboParry_ComboAttack)
- UseSkill(FistRush)
- UseSkill(Quake2)
- UseSkill(S21_RakeAttack)
- UseSkill(S08_TurnAttackL|S09_TurnAttackR)
- UseSkill(MoveBackDust)
- UseSkill(MoveBack combo=TableCommand)
- UseSkill(S15_JumpAttack|S18_ThrowStone|S07_ZigzagMoveAttack)
- UseSkill(S03_ShortMoveLeft)
- UseSkill(S06_PunchAndLongMoveRight)
- UseSkill(SwingCombo)
- UseSkill(S07_ZigzagMoveAttack)
- UseSkill(S17_LeftBlowCombo)
- UseSkill(S24_PhaseChange2)
- UseSkill(BodyCheck)
