# M_ThornHead_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ❓ Selector
        │   ├── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackPC ON)]
        │   │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        │   └── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackTachyNPC ON)]
        │       └── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🏠 MoveToHome [🛡️CheckActorEffect(Self.Check_AttackPC ON) && 🛡️DetectResult(==)]
        │   ├── 🏠 MoveToHome [🛡️CheckActorEffect(Self.Check_AttackTachyNPC ON) && 🛡️DetectResult(==)]
        │   └── 🏠 MoveToHome [🛡️CheckActorEffect(Self.Check_AttackTachyNPC) && 🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️CheckActorEffect(Self.Check_AttackTachyNPC) && 🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                │       │   ├── 🚶 MoveToTarget
                │       │   ├── ⏳ WaitTimeRandom
                │       │   └── ⚔️ UseSkill(HeadButt)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           ├── ⏳ WaitTimeRandom
                │           └── ⚔️ UseSkill(Swing)
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⚔️ UseSkill(SwingCombo)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(Swing)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(HeadButt)
                │   └── ⏳ WaitTimeRandom
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 14 |
| Selector | 13 |
| Sequence | 10 |
| Task/WaitTimeRandom | 7 |
| Dec/DetectResult | 6 |
| Dec/AggroLevel | 5 |
| Task/UseSkill | 5 |
| Dec/Blackboard | 4 |
| Task/Blackboard | 4 |
| Dec/IsAlive | 3 |
| Task/DetectTarget | 3 |
| Task/MoveToHome | 3 |
| Task/MoveToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/UseableTimeReset | 1 |
| Dec/Random | 1 |
| Dec/UseableTime | 1 |

## 스킬 목록
- UseSkill(HeadButt)
- UseSkill(Swing)
- UseSkill(SwingCombo)
- UseSkill(Swing)
- UseSkill(HeadButt)
