# M_Tentacle_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.? ON)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │       ├── 📋 Blackboard(BattleState)
            │       └── ⚔️ UseSkill(Summoned|Summoned)
            ├── ❓ Selector [🛡️Blackboard(BattleState) && 🛡️CheckActorEffect(Self.? ON)]
            │   └── ➡️ Sequence
            │       ├── 📋 Blackboard(BattleState)
            │       └── ⚔️ UseSkill(Summoned|Summoned)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.?)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │       ├── 📋 Blackboard(BattleState)
            │       └── 🎬 PlayShow
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(NOT Self.? ON)]
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(MeleeAttack combo=TableCommand)
                │   └── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
                └── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 10 |
| Dec/CheckActorEffect | 9 |
| Sequence | 5 |
| Dec/AggroLevel | 4 |
| Task/Wait | 4 |
| Dec/IsAlive | 3 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Task/UseSkill | 3 |
| Dec/DetectResult | 2 |
| Dec/Random | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/PlayShow | 1 |
| Task/CautionToTarget | 1 |
| Dec/TimeLimit | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(Summoned|Summoned)
- UseSkill(Summoned|Summoned)
- UseSkill(MeleeAttack combo=TableCommand)
