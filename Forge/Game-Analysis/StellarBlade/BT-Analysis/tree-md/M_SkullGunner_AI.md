# M_SkullGunner_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── ⚔️ UseSkill(BattleEnd) [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==) && 🛡️CheckActorEffect(Self.M_SkullGunnerBody_CombiReady)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI [🛡️CheckActorEffect(Self.M_SkullGunnerBody_CombiReady)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ➡️ Sequence [🛡️CheckStance(M_SkullGunner_LieDown) && 🛡️Blackboard(BattleStart)]
            │   │   ├── ⚔️ UseSkill(StandUpDown combo=TableCommand)
            │   │   └── 📋 Blackboard(BattleStart)
            │   ├── ➡️ Sequence [🛡️CheckStance(M_SkullGunner_Sitting) && 🛡️Blackboard(BattleStart)]
            │   │   ├── ⚔️ UseSkill(StandUpSit combo=TableCommand)
            │   │   └── 📋 Blackboard(BattleStart)
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckStance(M_SkullGunner_Default) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           ├── 🎬 PlayShow
            │           └── ✨ UseEffect(['BattleMode_5s'])
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.94s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(2.94s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(M_SkullGunner_Default)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(StartTimer1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(StartTimer1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=40) && 🛡️TimeLimit(2.94s [Timer_BattleStartSkill]) && 🛡️DistanceToTarget(dist>300.0)]
            │       │       ├── ⚔️ UseSkill(S05_ArmSwing combo=TableCommand)
            │       │       ├── ⚔️ UseSkill(S01_SingleShot|S03_DoubleShot)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(S01_SingleShot|S03_DoubleShot)
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(S05_ArmSwing) [🛡️Random(rand(100)<=60)]
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   └── ⚔️ UseSkill(S02_BackMove)
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(S03_DoubleShot) [🛡️Random(rand(100)<=30)]
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(S01_SingleShot)
            │       │   ├── ⏳ WaitTimeRandom
            │       │   └── ❓ Selector
            │       │       ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️Random(rand(100)<=50) && 🛡️DistanceToTarget(dist<1000.0) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(2.0s [Timer1])]
            │       │       └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckStance(M_SkullGunner_Default) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(StartTimer1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(StartTimer1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=40) && 🛡️TimeLimit(2.94s [Timer_BattleStartSkill]) && 🛡️DistanceToTarget(dist>300.0)]
                │       ├── ⚔️ UseSkill(S05_ArmSwing combo=TableCommand)
                │       ├── ⚔️ UseSkill(S01_SingleShot|S03_DoubleShot)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(S01_SingleShot|S03_DoubleShot)
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(S05_ArmSwing) [🛡️Random(rand(100)<=60)]
                │   │   └── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(S02_BackMove)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(S04_ChargeShot) [🛡️Random(rand(100)<=20)]
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(S01_SingleShot|S03_DoubleShot)
                │   ├── ⏳ WaitTimeRandom
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️Random(rand(100)<=50) && 🛡️DistanceToTarget(dist<1000.0) && 🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(2.0s [Timer1])]
                │       └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 20 |
| Sequence | 18 |
| Task/UseSkill | 17 |
| Dec/CheckActorEffect | 14 |
| Task/WaitTimeRandom | 9 |
| Dec/Blackboard | 8 |
| Task/Blackboard | 8 |
| Dec/TimeLimit | 8 |
| Dec/Random | 8 |
| Dec/AggroLevel | 7 |
| Task/CautionToTarget | 7 |
| Dec/DistanceToTarget | 6 |
| Dec/CheckStance | 5 |
| Task/UseableTimeReset | 4 |
| Task/MoveToTarget | 4 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/UseableTime | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/PlayShow | 1 |
| Task/UseEffect | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(StandUpDown combo=TableCommand)
- UseSkill(StandUpSit combo=TableCommand)
- UseSkill(S05_ArmSwing combo=TableCommand)
- UseSkill(S01_SingleShot|S03_DoubleShot)
- UseSkill(S01_SingleShot|S03_DoubleShot)
- UseSkill(S05_ArmSwing)
- UseSkill(S02_BackMove)
- UseSkill(S03_DoubleShot)
- UseSkill(S01_SingleShot)
- UseSkill(S05_ArmSwing combo=TableCommand)
- UseSkill(S01_SingleShot|S03_DoubleShot)
- UseSkill(S01_SingleShot|S03_DoubleShot)
- UseSkill(S05_ArmSwing)
- UseSkill(S02_BackMove)
- UseSkill(S04_ChargeShot)
- UseSkill(S01_SingleShot|S03_DoubleShot)
