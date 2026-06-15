# M_BotRangeB_Nikke_AI

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
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ❓ Selector
                │   └── ⚔️ UseSkill(Nikke_Missile)
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ❓ Selector
                        └── ⚔️ UseSkill(Nikke_ShotLeft|Nikke_ShotRight subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 7 |
| Sequence | 5 |
| Dec/AggroLevel | 4 |
| Dec/CheckActorEffect | 4 |
| Task/UseSkill | 3 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Dec/DetectResult | 3 |
| Dec/IsAlive | 2 |
| Dec/IsActiveSkill | 2 |
| Task/DetectTarget | 2 |
| Task/UseableTimeReset | 2 |
| Task/MoveToHome | 1 |
| Task/PlayShow | 1 |
| Dec/CheckActorState | 1 |
| Dec/IsGroupTarget | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(Nikke_Missile)
- UseSkill(Nikke_ShotLeft|Nikke_ShotRight subTarget)
