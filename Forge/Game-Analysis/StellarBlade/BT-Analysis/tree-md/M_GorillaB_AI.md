# M_GorillaB_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   └── ➡️ Sequence [🛡️Blackboard(BattleStart)]
        │       ├── 📋 Blackboard(BattleStart)
        │       └── 🚶 MoveToTarget [🛡️TimeLimit(?s [StartTimer1])]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [DownCautionTimer1])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_Phase2 ON)]
                │   └── ⚔️ UseSkill(S24_PhaseChange2)
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorStat(ActorStatType_HP>60.0%)]
                │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                │   │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck)]
                │   │   │   ├── 🔄 UseableTimeReset
                │   │   │   ├── 🔄 UseableTimeReset
                │   │   │   ├── 🔄 UseableTimeReset
                │   │   │   ├── 🔄 UseableTimeReset
                │   │   │   └── 📋 Blackboard(DistanceCheck)
                │   │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   │   ├── 📋 Blackboard(FirstShot)
                │   │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   │   ├── ❓ Selector [🛡️UseableTime]
                │   │   │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=1200.0)]
                │   │   │   │   ├── 📋 Blackboard(DistanceCheck)
                │   │   │   │   └── ❓ Selector
                │   │   │   │       ├── ⚔️ UseSkill(S15_JumpAttack) [🛡️CheckActorStat(ActorStatType_HP<=95.0%)]
                │   │   │   │       └── ⚔️ UseSkill(S18_ThrowStone)
                │   │   │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=500.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                │   │   │       └── 📋 Blackboard(DistanceCheck)
                │   │   ├── ❓ Selector
                │   │   │   ├── ⚔️ UseSkill(ComboParry_ComboAttack) [🛡️CheckActorStat(ActorStatType_HP<=85.0%)]
                │   │   │   └── ⚔️ UseSkill(S14_SmashCombo) [🛡️CheckActorStat(ActorStatType_HP<=95.0%)]
                │   │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=90.0%) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_Phase2 ON)]
                │   │   │   ├── ⚔️ UseSkill(Quake) [🛡️DistanceToTarget(dist<=800.0)]
                │   │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [Timer1]) && 🛡️DistanceToTarget(dist>=1500.0)]
                │   │   ├── ❓ Selector
                │   │   │   └── ⚔️ UseSkill(S08_TurnAttackL|S09_TurnAttackR)
                │   │   ├── ❓ Selector
                │   │   │   ├── ⚔️ UseSkill(S22_Grab) [🛡️UseableTime]
                │   │   │   ├── ⚔️ UseSkill(MoveBackDust) [🛡️UseableTime]
                │   │   │   └── ⚔️ UseSkill(MoveBack) [🛡️UseableTime]
                │   │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON)]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ❓ Selector
                │   │   │   │   ├── ⚔️ UseSkill(S03_ShortMoveLeft)
                │   │   │   │   └── ⚔️ UseSkill(S06_PunchAndLongMoveRight)
                │   │   │   ├── ⏳ Wait(0.1s)
                │   │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.7s [Timer2]) && 🛡️DistanceToTarget(dist>=1500.0)]
                │   │   ├── ❓ Selector
                │   │   │   ├── ⚔️ UseSkill(BodySlam) [🛡️DistanceToTarget(dist>=800.0)]
                │   │   │   └── ⚔️ UseSkill(S21_RakeAttack) [🛡️DistanceToTarget(dist>=400.0)]
                │   │   ├── ❓ Selector
                │   │   │   └── ⚔️ UseSkill(S17_LeftBlowCombo)
                │   │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON)]
                │   │   └── ❓ Selector
                │   │       └── ⚔️ UseSkill(S19_RightBlow)
                │   └── ➡️ Sequence
                │       ├── ✨ UseEffect(['M_GorillaB_CheckRun'])
                │       └── ❓ Selector
                │           ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<600.0)]
                │           └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=600.0)]
                └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_GorillaB_Phase2 ON)]
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_GorillaB_Phase2 ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    │   ├── ➡️ Sequence [🛡️Blackboard(Phase2Check)]
                    │   │   ├── 🔄 UseableTimeReset
                    │   │   └── 📋 Blackboard(Phase2Check)
                    │   ├── ➡️ Sequence [🛡️UseableTime]
                    │   │   ├── 📋 Blackboard(Phase2Check)
                    │   │   └── ❓ Selector
                    │   │       ├── ➡️ Sequence
                    │   │       │   ├── ⚔️ UseSkill(Quake2) [🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                    │   │       │   └── ✨ UseEffect(['M_GorillaB_SkillCheck'])
                    │   │       └── ➡️ Sequence
                    │   │           ├── ⚔️ UseSkill(FistRush)
                    │   │           └── ✨ UseEffect(['M_GorillaB_SkillCheck'])
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer1]) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON) && 🛡️DistanceToTarget(dist>=500.0)]
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                    │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck)]
                    │   │   ├── 🔄 UseableTimeReset
                    │   │   ├── 🔄 UseableTimeReset
                    │   │   ├── 🔄 UseableTimeReset
                    │   │   ├── 🔄 UseableTimeReset
                    │   │   └── 📋 Blackboard(DistanceCheck)
                    │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                    │   │   ├── 📋 Blackboard(FirstShot)
                    │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                    │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                    │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                    │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                    │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                    │   ├── ❓ Selector [🛡️UseableTime]
                    │   │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck) && 🛡️DistanceToTarget(dist>=1200.0)]
                    │   │   │   ├── 📋 Blackboard(DistanceCheck)
                    │   │   │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_GorillaB_SkillCheck ON)]
                    │   │   │       ├── ⚔️ UseSkill(S15_JumpAttack)
                    │   │   │       └── ⚔️ UseSkill(S18_ThrowStone_2)
                    │   │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=500.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                    │   │       └── 📋 Blackboard(DistanceCheck)
                    │   ├── ❓ Selector
                    │   │   ├── ⚔️ UseSkill(ComboParry_ComboAttack)
                    │   │   └── ⚔️ UseSkill(S14_SmashCombo)
                    │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=90.0%) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_Phase2 ON)]
                    │   │   ├── ⚔️ UseSkill(Quake) [🛡️DistanceToTarget(dist<=800.0)]
                    │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [Timer1]) && 🛡️DistanceToTarget(dist>=1500.0)]
                    │   ├── ❓ Selector
                    │   │   └── ⚔️ UseSkill(S08_TurnAttackL|S09_TurnAttackR)
                    │   ├── ❓ Selector
                    │   │   ├── ⚔️ UseSkill(S22_Grab) [🛡️UseableTime]
                    │   │   ├── ⚔️ UseSkill(MoveBackDust) [🛡️UseableTime]
                    │   │   └── ⚔️ UseSkill(MoveBack) [🛡️UseableTime]
                    │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON)]
                    │   ├── ➡️ Sequence
                    │   │   ├── ❓ Selector
                    │   │   │   ├── ⚔️ UseSkill(S03_ShortMoveLeft)
                    │   │   │   └── ⚔️ UseSkill(S06_PunchAndLongMoveRight)
                    │   │   ├── ⏳ Wait(0.1s)
                    │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.7s [Timer2]) && 🛡️DistanceToTarget(dist>=1500.0)]
                    │   ├── ❓ Selector
                    │   │   ├── ⚔️ UseSkill(BodySlam) [🛡️DistanceToTarget(dist>=800.0)]
                    │   │   └── ⚔️ UseSkill(S21_RakeAttack) [🛡️DistanceToTarget(dist>=400.0)]
                    │   ├── ❓ Selector
                    │   │   └── ⚔️ UseSkill(S17_LeftBlowCombo)
                    │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [Timer3]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_GorillaB_CheckRun ON)]
                    │   └── ❓ Selector
                    │       └── ⚔️ UseSkill(S19_RightBlow)
                    └── ➡️ Sequence
                        ├── ✨ UseEffect(['M_GorillaB_CheckRun'])
                        └── ❓ Selector
                            ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<600.0)]
                            └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=600.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 38 |
| Selector | 33 |
| Sequence | 23 |
| Dec/DistanceToTarget | 23 |
| Dec/CheckActorEffect | 18 |
| Dec/TimeLimit | 13 |
| Task/Blackboard | 11 |
| Task/CautionToTarget | 10 |
| Dec/CheckActorStat | 9 |
| Task/UseableTimeReset | 9 |
| Dec/UseableTime | 9 |
| Dec/Blackboard | 8 |
| Task/MoveToTarget | 5 |
| Dec/Random | 5 |
| Dec/AggroLevel | 4 |
| Task/Wait | 4 |
| Task/UseEffect | 4 |
| Dec/IsAlive | 3 |
| Dec/AimMe | 3 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |

## 스킬 목록
- UseSkill(S24_PhaseChange2)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S15_JumpAttack)
- UseSkill(S18_ThrowStone)
- UseSkill(ComboParry_ComboAttack)
- UseSkill(S14_SmashCombo)
- UseSkill(Quake)
- UseSkill(S08_TurnAttackL|S09_TurnAttackR)
- UseSkill(S22_Grab)
- UseSkill(MoveBackDust)
- UseSkill(MoveBack)
- UseSkill(S03_ShortMoveLeft)
- UseSkill(S06_PunchAndLongMoveRight)
- UseSkill(BodySlam)
- UseSkill(S21_RakeAttack)
- UseSkill(S17_LeftBlowCombo)
- UseSkill(S19_RightBlow)
- UseSkill(Quake2)
- UseSkill(FistRush)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S15_JumpAttack)
- UseSkill(S18_ThrowStone_2)
- UseSkill(ComboParry_ComboAttack)
- UseSkill(S14_SmashCombo)
- UseSkill(Quake)
- UseSkill(S08_TurnAttackL|S09_TurnAttackR)
- UseSkill(S22_Grab)
- UseSkill(MoveBackDust)
- UseSkill(MoveBack)
- UseSkill(S03_ShortMoveLeft)
- UseSkill(S06_PunchAndLongMoveRight)
- UseSkill(BodySlam)
- UseSkill(S21_RakeAttack)
- UseSkill(S17_LeftBlowCombo)
- UseSkill(S19_RightBlow)
