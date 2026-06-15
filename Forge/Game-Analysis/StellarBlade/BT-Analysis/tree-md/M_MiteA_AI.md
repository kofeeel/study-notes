# M_MiteA_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
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
            │       └── ➡️ Sequence [🛡️CheckStance(M_MiteA_Default)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ➡️ Sequence
            │   │   ├── ⏳ WaitTimeRandom
            │   │   └── ⚔️ UseSkill(MoveSkill)
            │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=300.0)]
            │       ├── 🎬 PlayShow
            │       └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence
                │   ├── ⏳ WaitTimeRandom
                │   ├── ⚔️ UseSkill(S03_HeadAttackRight)
                │   ├── ⚔️ UseSkill(S06_MoveBack)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⏳ WaitTimeRandom
                │   ├── ⚔️ UseSkill(S02_HeadAttackLeft)
                │   ├── ⚔️ UseSkill(S06_MoveBack)
                │   └── ⏳ WaitTimeRandom
                ├── ⚔️ UseSkill(S07_MoveForward)
                ├── ➡️ Sequence
                │   ├── ⏳ WaitTimeRandom
                │   ├── ⚔️ UseSkill(S01_TailSpin)
                │   └── ⏳ WaitTimeRandom
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 9 |
| Sequence | 9 |
| Task/WaitTimeRandom | 9 |
| Dec/CheckActorEffect | 8 |
| Task/UseSkill | 7 |
| Dec/AggroLevel | 5 |
| Dec/IsAlive | 3 |
| Task/PlayShow | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckStance | 1 |
| Dec/CheckActorState | 1 |
| Dec/DistanceToTarget | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(MoveSkill)
- UseSkill(S03_HeadAttackRight)
- UseSkill(S06_MoveBack)
- UseSkill(S02_HeadAttackLeft)
- UseSkill(S06_MoveBack)
- UseSkill(S07_MoveForward)
- UseSkill(S01_TailSpin)
