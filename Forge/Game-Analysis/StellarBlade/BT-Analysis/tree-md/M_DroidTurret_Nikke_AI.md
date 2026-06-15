# M_DroidTurret_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent) && 🛡️CheckStance(NOT M_DroidTurret_StanbyToNormal)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_DroidTurret_Nikke_TypeA ON)]
            │   └── ➡️ Sequence
            │       ├── 👁️ DetectTarget [🛡️DetectResult(==)]
            │       └── ➡️ Sequence
            │           ├── ⏳ WaitTimeRandom
            │           └── ⚔️ UseSkill(Nikke_Shot|Nikke_ShotDozen combo=TableCommand subTarget)
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_DroidTurret_Nikke_TypeB ON)]
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ➡️ Sequence
                        ├── ⏳ WaitTimeRandom
                        └── ⚔️ UseSkill(Nikke_ShotDebuff combo=TableCommand subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 6 |
| Sequence | 5 |
| Task/DetectTarget | 3 |
| Dec/DetectResult | 3 |
| Dec/AggroLevel | 3 |
| Dec/CheckActorEffect | 3 |
| Dec/IsAlive | 2 |
| Task/WaitTimeRandom | 2 |
| Task/UseSkill | 2 |
| Dec/Blackboard | 1 |
| Dec/CheckStance | 1 |
| Task/Blackboard | 1 |
| Task/PlayShow | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(Nikke_Shot|Nikke_ShotDozen combo=TableCommand subTarget)
- UseSkill(Nikke_ShotDebuff combo=TableCommand subTarget)
