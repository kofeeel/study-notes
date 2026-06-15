# M_BotDefense_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── ⚔️ UseSkill(BattleEnd) [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │       └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │           ├── 📋 Blackboard(BattleState)
            │           └── 🎬 PlayShow
            └── ❓ Selector [🛡️CheckStance(NOT M_BotUpperBody_Default) && 🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ❓ Selector
                        └── ⚔️ UseSkill(Nikke_Guard subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 7 |
| Sequence | 5 |
| Dec/AggroLevel | 5 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Dec/DetectResult | 3 |
| Dec/CheckActorEffect | 3 |
| Dec/IsAlive | 2 |
| Dec/IsActiveSkill | 2 |
| Task/UseSkill | 2 |
| Task/DetectTarget | 2 |
| Task/UseableTimeReset | 2 |
| Task/MoveToHome | 1 |
| Task/PlayShow | 1 |
| Dec/CheckStance | 1 |
| Dec/IsGroupTarget | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(Nikke_Guard subTarget)
