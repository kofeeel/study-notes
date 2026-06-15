# M_SunFlower_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ❓ Selector
        │   └── 🏠 MoveToHome [🛡️CheckActorEffect(Self.M_Common_Nikke_PathWay ON)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target) && 🛡️CheckActorEffect(Self.M_Common_Nikke_PathWay)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ➡️ Sequence
                        ├── ⚔️ UseSkill(Nikke_Spit subTarget)
                        └── ⏳ WaitTimeRandom
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 7 |
| Sequence | 5 |
| Dec/CheckActorEffect | 5 |
| Dec/AggroLevel | 3 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Dec/IsAlive | 2 |
| Task/PlayShow | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 1 |
| Task/MoveToHome | 1 |
| Dec/CheckActorState | 1 |
| Dec/IsGroupTarget | 1 |
| Task/UseableTimeReset | 1 |
| Task/UseSkill | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(Nikke_Spit subTarget)
