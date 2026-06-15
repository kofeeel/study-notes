# M_GrubShooterB_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       ├── 🎬 PlayShow
            │       └── ⏳ Wait(1.2s)
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ❓ Selector
                        └── ⚔️ UseSkill(Nikke_SpitMassAttack|Nikke_SpitAttack|Nikke_ShotInPosition combo=TableCommand subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 6 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Dec/AggroLevel | 2 |
| Sequence | 2 |
| Dec/Blackboard | 1 |
| Dec/CheckActorEffect | 1 |
| Task/Blackboard | 1 |
| Task/PlayShow | 1 |
| Task/Wait | 1 |
| Task/UseSkill | 1 |

## 스킬 목록
- UseSkill(Nikke_SpitMassAttack|Nikke_SpitAttack|Nikke_ShotInPosition combo=TableCommand subTarget)
