# M_MiteC_AI

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
            │       └── ➡️ Sequence
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
                ├── ➡️ Sequence [🛡️Blackboard(Timer1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(Timer1)
                ├── ➡️ Sequence [🛡️Blackboard(Timer1) && 🛡️UseableTime && 🛡️LockOnMe && 🛡️DistanceToTarget(dist>=400.0)]
                │   ├── ⚔️ UseSkill(RollingAttack)
                │   ├── 📋 Blackboard(Timer1)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50) && 🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
                │       │   ├── 🚶 MoveToTarget
                │       │   ├── ⏳ WaitTimeRandom
                │       │   └── ⚔️ UseSkill(TailStamp)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           ├── ⏳ WaitTimeRandom
                │           └── ⚔️ UseSkill(TailSpin)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(MoveSkill combo=TableCommand)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
                │   ├── ⏳ WaitTimeRandom
                │   ├── ⚔️ UseSkill(TailStamp)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(TailSpin)
                │   └── ⏳ WaitTimeRandom
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 14 |
| Selector | 10 |
| Dec/CheckActorEffect | 10 |
| Task/WaitTimeRandom | 10 |
| Task/UseSkill | 7 |
| Dec/AggroLevel | 5 |
| Dec/Blackboard | 5 |
| Task/Blackboard | 5 |
| Dec/IsAlive | 3 |
| Task/PlayShow | 3 |
| Task/MoveToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/DistanceToTarget | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/UseableTimeReset | 1 |
| Dec/UseableTime | 1 |
| Dec/LockOnMe | 1 |
| Dec/TimeLimit | 1 |
| Dec/Random | 1 |

## 스킬 목록
- UseSkill(MoveSkill)
- UseSkill(RollingAttack)
- UseSkill(TailStamp)
- UseSkill(TailSpin)
- UseSkill(MoveSkill combo=TableCommand)
- UseSkill(TailStamp)
- UseSkill(TailSpin)
