# M_LesserLurker_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector
        │   ├── ❓ Selector [🛡️CheckActorEffect(Self.? ON) && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_LesserLurker_EventDig ON)]
        │   │   │   ├── 🎬 PlayShow
        │   │   │   └── ❓ Selector
        │   │   │       ├── ⏳ WaitTimeRandom [🛡️Random(rand(100)<=40)]
        │   │   │       └── 👁️ DetectTarget [🛡️DetectResult(==)]
        │   │   └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_LesserLurker_EventDetect ON)]
        │   │       ├── 🎬 PlayShow
        │   │       └── ❓ Selector
        │   │           ├── ⏳ WaitTimeRandom [🛡️Random(rand(100)<=50)]
        │   │           └── 👁️ DetectTarget [🛡️DetectResult(==)]
        │   └── ➡️ Sequence
        │       ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        │       └── ✨ UseEffect(['M_LesserLurker_EventDigDispel', 'M_LesserLurker_EventDetectDispel']) [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │       ├── 📋 Blackboard(BattleState)
            │       └── 🎬 PlayShow
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(MeleeAttack combo=TableCommand)
                │   └── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
                ├── ⏳ WaitTimeRandom [🛡️Random(rand(100)<=80)]
                └── ➡️ Sequence
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 12 |
| Dec/CheckActorEffect | 11 |
| Sequence | 9 |
| Dec/AggroLevel | 6 |
| Dec/DetectResult | 5 |
| Task/PlayShow | 4 |
| Task/WaitTimeRandom | 4 |
| Dec/Random | 4 |
| Dec/IsAlive | 3 |
| Task/DetectTarget | 3 |
| Task/Wait | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Task/UseEffect | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Task/CautionToTarget | 1 |
| Dec/TimeLimit | 1 |
| Task/UseSkill | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(MeleeAttack combo=TableCommand)
