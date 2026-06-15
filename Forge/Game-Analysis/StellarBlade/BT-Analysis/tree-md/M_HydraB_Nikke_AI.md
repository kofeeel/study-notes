# M_HydraB_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleState) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleState)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    ├── ⚔️ UseSkill(Nikke_MoveLeft|Nikke_MoveRight subTarget)
                    └── ➡️ Sequence
                        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                        └── ⚔️ UseSkill(Nikke_Spit subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 6 |
| Selector | 5 |
| Dec/AggroLevel | 4 |
| Dec/CheckActorEffect | 4 |
| Dec/DetectResult | 4 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Task/DetectTarget | 3 |
| Dec/IsAlive | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/UseableTimeReset | 2 |
| Task/UseSkill | 2 |
| Task/MoveToHome | 1 |
| Dec/CheckActorState | 1 |
| Dec/IsGroupTarget | 1 |

## 스킬 목록
- UseSkill(Nikke_MoveLeft|Nikke_MoveRight subTarget)
- UseSkill(Nikke_Spit subTarget)
