# M_SkullSpear_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── ⚔️ UseSkill(BattleEnd) [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==) && 🛡️CheckActorEffect(Self.M_SkullSpearBody_CombiReady)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI [🛡️CheckActorEffect(Self.M_SkullSpearBody_CombiReady)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ➡️ Sequence [🛡️CheckStance(M_SkullSpear_LieDown) && 🛡️Blackboard(BattleStart)]
            │   │   ├── ⚔️ UseSkill(StandUpDown combo=TableCommand)
            │   │   └── 📋 Blackboard(BattleStart)
            │   ├── ➡️ Sequence [🛡️CheckStance(M_SkullSpear_Sitting) && 🛡️Blackboard(BattleStart)]
            │   │   ├── ⚔️ UseSkill(StandUpSit combo=TableCommand)
            │   │   └── 📋 Blackboard(BattleStart)
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckStance(M_SkullSpear_Default) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           ├── ⚔️ UseSkill(BattleStart combo=TableCommand)
            │           └── ✨ UseEffect(['BattleMode_5s'])
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.94s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(2.94s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(M_SkullSpear_Default)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │       │   ├── 📋 Blackboard(FirstShot)
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=85)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=20)]
            │       │       │   └── ⚔️ UseSkill(S03_Thrust)
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(S02_TripleCombo)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(S01_Swing|CS01_SwingCombo)
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=300.0)]
            │       │   ├── ⚔️ UseSkill(S03_Thrust)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo)
            │       │   ├── ⏳ WaitTimeRandom
            │       │   └── ❓ Selector
            │       │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=400.0) && 🛡️TimeLimit(3.0s [Timer1]) && 🛡️Random(rand(100)<=60)]
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<=800.0) && 🛡️TimeLimit(4.0s [Timer2]) && 🛡️Random(rand(100)<=80) && 🛡️UseableTime]
            │       │           ├── ⚔️ UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo|S03_Thrust)
            │       │           └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           ├── ➡️ Sequence
            │           │   ├── ⚔️ UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo|S03_Thrust)
            │           │   ├── 🚶 MoveToTarget [🛡️UseableTime && 🛡️DistanceToTarget(dist<=600.0) && 🛡️Random(rand(100)<=50) && 🛡️TimeLimit(4.0s [Timer2])]
            │           │   └── ⏳ WaitTimeRandom
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckStance(M_SkullSpear_Default) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=85)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=20)]
                │       │   └── ⚔️ UseSkill(S03_Thrust)
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(S02_TripleCombo)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(S01_Swing|CS01_SwingCombo)
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=300.0)]
                │   ├── ⚔️ UseSkill(S03_Thrust)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo)
                │   ├── ⏳ WaitTimeRandom
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=400.0) && 🛡️TimeLimit(3.0s [Timer1]) && 🛡️Random(rand(100)<=60)]
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist<=800.0) && 🛡️TimeLimit(4.0s [Timer2]) && 🛡️Random(rand(100)<=80) && 🛡️UseableTime]
                │           ├── ⚔️ UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo|S03_Thrust)
                │           └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    ├── ➡️ Sequence
                    │   ├── 🚶 MoveToTarget [🛡️UseableTime && 🛡️DistanceToTarget(dist<=600.0) && 🛡️Random(rand(100)<=50) && 🛡️TimeLimit(4.0s [Timer2])]
                    │   ├── ⚔️ UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo|S03_Thrust)
                    │   └── ⏳ WaitTimeRandom
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 28 |
| Task/UseSkill | 24 |
| Selector | 20 |
| Dec/CheckActorEffect | 16 |
| Dec/Random | 16 |
| Dec/Blackboard | 10 |
| Task/Blackboard | 10 |
| Dec/TimeLimit | 10 |
| Dec/DistanceToTarget | 10 |
| Task/MoveToTarget | 10 |
| Task/WaitTimeRandom | 9 |
| Dec/AggroLevel | 7 |
| Dec/CheckStance | 5 |
| Task/CautionToTarget | 5 |
| Task/UseableTimeReset | 4 |
| Dec/AimMe | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/CheckActorStat | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/UseEffect | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(StandUpDown combo=TableCommand)
- UseSkill(StandUpSit combo=TableCommand)
- UseSkill(BattleStart combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S03_Thrust)
- UseSkill(S02_TripleCombo)
- UseSkill(S01_Swing|CS01_SwingCombo)
- UseSkill(S03_Thrust)
- UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo)
- UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo|S03_Thrust)
- UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo|S03_Thrust)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(S03_Thrust)
- UseSkill(S02_TripleCombo)
- UseSkill(S01_Swing|CS01_SwingCombo)
- UseSkill(S03_Thrust)
- UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo)
- UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo|S03_Thrust)
- UseSkill(S02_TripleCombo|S01_Swing|CS01_SwingCombo|S03_Thrust)
