# M_TurretLaser_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent) && 🛡️CheckActorEffect(Self.M_TurretLaser_TypeFastA)]
            │       └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │           ├── 📋 Blackboard(BattleState)
            │           ├── 🎬 PlayShow
            │           └── ✨ UseEffect(['BattleMode'])
            └── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_CheckStoryMode)]
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ❓ Selector
                        ├── ⚔️ UseSkill(Nikke_Shot combo=TableCommand subTarget) [🛡️CheckActorEffect(Self.M_TurretLaser_TypeA ON)]
                        └── ⚔️ UseSkill(Nikke_LaserBeam combo=TableCommand subTarget) [🛡️CheckActorEffect(Self.M_TurretLaser_TypeB ON)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 7 |
| Dec/CheckActorEffect | 5 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Dec/AggroLevel | 2 |
| Sequence | 2 |
| Task/UseSkill | 2 |
| Dec/Blackboard | 1 |
| Task/Blackboard | 1 |
| Task/PlayShow | 1 |
| Task/UseEffect | 1 |

## 스킬 목록
- UseSkill(Nikke_Shot combo=TableCommand subTarget)
- UseSkill(Nikke_LaserBeam combo=TableCommand subTarget)
