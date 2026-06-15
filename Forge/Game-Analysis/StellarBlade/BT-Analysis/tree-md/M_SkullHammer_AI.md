# M_SkullHammer_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── ⚔️ UseSkill(BattleEnd) [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🏠 MoveToHome [🛡️DetectResult(==)]
        │   └── ✨ UseEffect(['BattleMode_Dispel']) [🛡️CheckActorEffect(Self.BattleMode_5s ON)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   └── ➡️ Sequence
            │   │       ├── 📋 Blackboard(BattleStart)
            │   │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent ON)]
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=200.0)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.34s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.34s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence [🛡️Blackboard(BB_NoGuard)]
            │       │   │   ├── 🔄 UseableTimeReset
            │       │   │   └── 📋 Blackboard(BB_NoGuard)
            │       │   └── ➡️ Sequence [🛡️UseableTime]
            │       │       ├── ⚔️ UseSkill(S01_VerticalSmash)
            │       │       ├── ⏳ WaitTimeRandom
            │       │       └── ❓ Selector
            │       │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer1]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️Random(rand(100)<=60) && 🛡️UseableTime]
            │       │           └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(S03_SwingAndSmash|S02_HorizontalDoubleSwing|CS01_TripleSmash)
            │       │   ├── ⏳ WaitTimeRandom
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence
            │       │       │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer1]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️Random(rand(100)<=60) && 🛡️UseableTime]
            │       │       │   ├── ⚔️ UseSkill(S03_SwingAndSmash|S02_HorizontalDoubleSwing)
            │       │       │   └── ⏳ WaitTimeRandom
            │       │       └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           ├── 🚶 MoveToTarget [🛡️UseableTime && 🛡️Random(rand(100)<=50) && 🛡️TimeLimit(3.0s [Timer1]) && 🛡️DistanceToTarget(dist<=500.0)]
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️Blackboard(BB_NoGuard)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB_NoGuard)
                │   └── ➡️ Sequence [🛡️UseableTime]
                │       ├── ⚔️ UseSkill(S01_VerticalSmash)
                │       ├── ⏳ WaitTimeRandom
                │       └── ❓ Selector
                │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer1]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️Random(rand(100)<=60) && 🛡️UseableTime]
                │           └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(S03_SwingAndSmash|S02_HorizontalDoubleSwing|CS01_TripleSmash)
                │   ├── ⏳ WaitTimeRandom
                │   └── ❓ Selector
                │       ├── ➡️ Sequence
                │       │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer1]) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️Random(rand(100)<=60) && 🛡️UseableTime]
                │       │   ├── ⚔️ UseSkill(S03_SwingAndSmash|S02_HorizontalDoubleSwing)
                │       │   └── ⏳ WaitTimeRandom
                │       └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    ├── 🚶 MoveToTarget [🛡️UseableTime && 🛡️Random(rand(100)<=50) && 🛡️TimeLimit(3.0s [Timer1]) && 🛡️DistanceToTarget(dist<=500.0)]
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 21 |
| Sequence | 15 |
| Dec/CheckActorEffect | 14 |
| Task/WaitTimeRandom | 11 |
| Dec/TimeLimit | 8 |
| Dec/UseableTime | 8 |
| Dec/AggroLevel | 7 |
| Task/UseSkill | 7 |
| Dec/Blackboard | 7 |
| Task/Blackboard | 7 |
| Dec/DistanceToTarget | 7 |
| Task/CautionToTarget | 7 |
| Task/UseableTimeReset | 6 |
| Dec/Random | 6 |
| Task/MoveToTarget | 5 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/UseEffect | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/PlayShow | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(S01_VerticalSmash)
- UseSkill(S03_SwingAndSmash|S02_HorizontalDoubleSwing|CS01_TripleSmash)
- UseSkill(S03_SwingAndSmash|S02_HorizontalDoubleSwing)
- UseSkill(S01_VerticalSmash)
- UseSkill(S03_SwingAndSmash|S02_HorizontalDoubleSwing|CS01_TripleSmash)
- UseSkill(S03_SwingAndSmash|S02_HorizontalDoubleSwing)
