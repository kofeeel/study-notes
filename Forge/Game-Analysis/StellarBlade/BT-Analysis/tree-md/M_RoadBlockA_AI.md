# M_RoadBlockA_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️Blackboard(BattleStart)]
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ➡️ Sequence [🛡️Blackboard(SummonCount)]
                │   ├── 📋 Blackboard(SummonCount) [🛡️CheckSummonedCount(count<?)]
                │   └── ⚔️ UseSkill(Summon combo=TableCommand)
                ├── ➡️ Sequence [🛡️Blackboard(SummonCount) && 🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── 📋 Blackboard(SummonCount)
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(Vomit combo=TableCommand)
                │   └── ⏳ WaitTimeRandom
                └── ❓ Selector [🛡️CheckStance(M_RoadBlockA_ATL)]
                    └── ⚔️ UseSkill(GranadeShot combo=TableCommand)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 9 |
| Sequence | 6 |
| Task/Blackboard | 6 |
| Dec/Blackboard | 5 |
| Dec/AggroLevel | 4 |
| Dec/IsAlive | 3 |
| Task/UseSkill | 3 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsActiveSkill | 1 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/CheckActorState | 1 |
| Dec/CheckActorEffect | 1 |
| Dec/CheckSummonedCount | 1 |
| Task/UseableTimeReset | 1 |
| Dec/UseableTime | 1 |
| Task/WaitTimeRandom | 1 |
| Dec/CheckStance | 1 |

## 스킬 목록
- UseSkill(Summon combo=TableCommand)
- UseSkill(Vomit combo=TableCommand)
- UseSkill(GranadeShot combo=TableCommand)
