# M_BotSupporter_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ➡️ Sequence [🛡️IsAlive(Target)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ❓ Selector [🛡️IsAlive(SubTarget)]
                        └── ❓ Selector
                            ├── ⚔️ UseSkill(Nikke_Heal combo=TableCommand subTarget) [🛡️CheckActorEffect(SubTarget.LinkState_MonsterHit) && 🛡️CheckActorEffect(SubTarget.Check_BotSupporter) && 🛡️CheckActorEffect(SubTarget.Check_RecoveryMark) && 🛡️CheckActorStat(ActorStatType_HP<=90.0%) && 🛡️DistanceToTarget(dist<=600.0)]
                            └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(SubTarget.Check_BotSupporter)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 5 |
| Dec/CheckActorEffect | 4 |
| Dec/IsAlive | 3 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Sequence | 2 |
| Dec/DistanceToTarget | 2 |
| Dec/AggroLevel | 1 |
| Task/UseSkill | 1 |
| Dec/CheckActorStat | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(Nikke_Heal combo=TableCommand subTarget)
