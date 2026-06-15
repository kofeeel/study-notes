# M_BotSupporter_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        └── ➡️ Sequence [🛡️IsAlive(Target)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                │   └── ❓ Selector [🛡️IsAlive(SubTarget)]
                │       ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                │       │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
                │       │       ├── 🎬 PlayShow
                │       │       └── 📋 Blackboard(BattleStart)
                │       ├── ❓ Selector
                │       │   ├── 🚶 MoveToTarget [🛡️CheckActorEffect(SubTarget.Check_BotSupporter) && 🛡️DistanceToTarget(dist>=500.0)]
                │       │   └── ⚔️ UseSkill(Heal combo=TableCommand subTarget) [🛡️CheckActorEffect(SubTarget.LinkState_MonsterHit) && 🛡️CheckActorEffect(SubTarget.Check_BotSupporter) && 🛡️CheckActorEffect(SubTarget.Check_RecoveryMark) && 🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
                │       └── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON) && 🛡️IsGroupTarget(NOT)]
                │           └── ⏳ Wait(3.0s)
                └── ❓ Selector [🛡️IsAlive(NOT SubTarget)]
                    ├── ⚔️ UseSkill(SuicideBomb combo=TableCommand)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 8 |
| Dec/CheckActorEffect | 8 |
| Sequence | 6 |
| Dec/AggroLevel | 5 |
| Dec/IsAlive | 4 |
| Dec/DetectResult | 3 |
| Task/DetectTarget | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Task/MoveToTarget | 2 |
| Task/UseSkill | 2 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/DistanceToTarget | 1 |
| Dec/CheckActorStat | 1 |
| Dec/IsGroupTarget | 1 |
| Task/Wait | 1 |

## 스킬 목록
- UseSkill(Heal combo=TableCommand subTarget)
- UseSkill(SuicideBomb combo=TableCommand)
