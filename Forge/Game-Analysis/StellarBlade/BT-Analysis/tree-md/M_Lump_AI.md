# M_Lump_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── ⚔️ UseSkill(BattleEnd) [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=40)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(SwingRush)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(Stamp)
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_NoGuard)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB_NoGuard)
            │       ├── ➡️ Sequence [🛡️UseableTime]
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(StabCombo)
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(SwingStamp)
            │       │       └── ⏳ WaitTimeRandom
            │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer1]) && 🛡️DistanceToTarget(dist>600.0) && 🛡️DistanceToTarget(dist<800.0) && 🛡️Random(rand(100)<=50)]
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(SwingCombo) [🛡️UseableTime]
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(Stamp|SwingRush)
            │       │       └── ⏳ WaitTimeRandom
            │       └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=40)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(SwingRush)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(Stamp)
                ├── ➡️ Sequence [🛡️Blackboard(BB_NoGuard)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB_NoGuard)
                ├── ❓ Selector [🛡️UseableTime]
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(StabCombo)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(SwingStamp)
                │       └── ⏳ WaitTimeRandom
                ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [Timer1]) && 🛡️DistanceToTarget(dist>600.0) && 🛡️DistanceToTarget(dist<800.0) && 🛡️Random(rand(100)<=50)]
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(SwingCombo) [🛡️UseableTime]
                │   │   └── ⏳ WaitTimeRandom
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(Stamp|SwingRush)
                │       └── ⏳ WaitTimeRandom
                └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 23 |
| Selector | 17 |
| Task/UseSkill | 13 |
| Dec/CheckActorEffect | 13 |
| Task/WaitTimeRandom | 9 |
| Dec/Blackboard | 8 |
| Task/Blackboard | 8 |
| Dec/AggroLevel | 7 |
| Task/MoveToTarget | 7 |
| Dec/TimeLimit | 6 |
| Dec/DistanceToTarget | 6 |
| Task/CautionToTarget | 5 |
| Task/UseableTimeReset | 4 |
| Dec/Random | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/PlayShow | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(SwingRush)
- UseSkill(Stamp)
- UseSkill(StabCombo)
- UseSkill(SwingStamp)
- UseSkill(SwingCombo)
- UseSkill(Stamp|SwingRush)
- UseSkill(SwingRush)
- UseSkill(Stamp)
- UseSkill(StabCombo)
- UseSkill(SwingStamp)
- UseSkill(SwingCombo)
- UseSkill(Stamp|SwingRush)
