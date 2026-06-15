# M_StatueC_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
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
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.46s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(Reload) [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(Self.M_StatueC_Socket_EmptyA ON) && 🛡️CheckActorEffect(Self.M_StatueC_Socket_EmptyB ON)]
                │   └── ⏳ WaitTimeRandom
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(Swing) [🛡️Random(rand(100)<=80)]
                │   │   └── ⏳ WaitTimeRandom
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(Reload) [🛡️CheckActorEffect(Self.M_StatueC_Socket_EmptyA ON) && 🛡️CheckActorEffect(Self.M_StatueC_Socket_EmptyB ON)]
                │       └── ⏳ WaitTimeRandom
                ├── ❓ Selector
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_StatueC_Socket_EmptyA)]
                │   │   ├── ➡️ Sequence
                │   │   │   ├── ⚔️ UseSkill(ShotMultiA|ShotChainA) [🛡️DistanceToTarget(dist>=700.0) && 🛡️UseableTime]
                │   │   │   └── ⏳ WaitTimeRandom
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(ShotAroundA)
                │   │       └── ⏳ WaitTimeRandom
                │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_StatueC_Socket_EmptyB)]
                │       ├── ➡️ Sequence
                │       │   ├── ⚔️ UseSkill(ShotMultiB|ShotChainB) [🛡️DistanceToTarget(dist>=700.0) && 🛡️UseableTime]
                │       │   └── ⏳ WaitTimeRandom
                │       └── ➡️ Sequence
                │           ├── ⚔️ UseSkill(ShotAroundB)
                │           └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=50) && 🛡️DistanceToTarget(dist<=800.0) && 🛡️TimeLimit(3.0s [Timer1])]
                    └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=800.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 14 |
| Dec/CheckActorEffect | 14 |
| Sequence | 12 |
| Task/WaitTimeRandom | 8 |
| Task/UseSkill | 7 |
| Dec/AggroLevel | 5 |
| Dec/DistanceToTarget | 5 |
| Dec/IsAlive | 3 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Task/CautionToTarget | 3 |
| Dec/TimeLimit | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/Random | 2 |
| Dec/UseableTime | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/UseableTimeReset | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(Reload)
- UseSkill(Swing)
- UseSkill(Reload)
- UseSkill(ShotMultiA|ShotChainA)
- UseSkill(ShotAroundA)
- UseSkill(ShotMultiB|ShotChainB)
- UseSkill(ShotAroundB)
