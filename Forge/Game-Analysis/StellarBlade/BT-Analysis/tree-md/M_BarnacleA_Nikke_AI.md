# M_BarnacleA_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ❓ Selector
        │   └── 🏠 MoveToHome [🛡️CheckActorEffect(Self.M_Common_Nikke_PathWay ON)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        └── ❓ Selector [🛡️IsAlive(Target) && 🛡️CheckActorEffect(Self.M_Common_Nikke_PathWay)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_BarnacleA_Nikke_EidosCheck ON) && 🛡️CheckActorEffect(NOT Self.M_BarnacleA_Nikke_WasteLandCheck ON)]
                │   ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                │   └── ❓ Selector
                │       ├── ⚔️ UseSkill(Nikke_MoveLeft|Nikke_MoveRight subTarget)
                │       └── ⚔️ UseSkill(Nikke_MoveLeftShort|Nikke_MoveRightShort subTarget)
                └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_BarnacleA_Nikke_WasteLandCheck ON) && 🛡️CheckActorEffect(NOT Self.M_BarnacleA_Nikke_EidosCheck ON)]
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ❓ Selector
                        ├── ⚔️ UseSkill(Nikke_MoveLeft|Nikke_MoveRight2 subTarget)
                        └── ⚔️ UseSkill(Nikke_MoveLeftShort|Nikke_MoveRightShort2 subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 9 |
| Selector | 7 |
| Sequence | 5 |
| Task/UseSkill | 4 |
| Dec/AggroLevel | 3 |
| Task/DetectTarget | 3 |
| Dec/DetectResult | 3 |
| Dec/IsAlive | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Task/UseableTimeReset | 2 |
| Dec/IsActiveSkill | 1 |
| Task/PlayShow | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/IsGroupTarget | 1 |

## 스킬 목록
- UseSkill(Nikke_MoveLeft|Nikke_MoveRight subTarget)
- UseSkill(Nikke_MoveLeftShort|Nikke_MoveRightShort subTarget)
- UseSkill(Nikke_MoveLeft|Nikke_MoveRight2 subTarget)
- UseSkill(Nikke_MoveLeftShort|Nikke_MoveRightShort2 subTarget)
