# N_TumblerDrone_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️DetectResult(==)]
            │   └── ❓ Selector [🛡️Blackboard(BattleStart)]
            │       └── ➡️ Sequence [🛡️CheckStance(N_TumblerDrone_Default)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            └── ❓ Selector [🛡️DetectResult(==)]
                └── ❓ Selector
                    ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=99.0%) && 🛡️Blackboard(RangeAttack) && 🛡️DistanceToTarget(dist>=900.0)]
                    │   ├── ⚔️ UseSkill(S01_Move) [🛡️CheckActorEffect(NOT Self.M_TumblerDrone_Tremble ON)]
                    │   └── 📋 Blackboard(RangeAttack)
                    └── ⚔️ UseSkill(S01_Move) [🛡️DistanceToTarget(dist<=900.0) && 🛡️CheckActorEffect(NOT Self.M_TumblerDrone_Tremble ON)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 8 |
| Dec/DetectResult | 4 |
| Dec/IsAlive | 2 |
| Sequence | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Dec/DistanceToTarget | 2 |
| Task/UseSkill | 2 |
| Dec/CheckActorEffect | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Dec/CheckStance | 1 |
| Task/PlayShow | 1 |
| Dec/CheckActorState | 1 |
| Dec/CheckActorStat | 1 |

## 스킬 목록
- UseSkill(S01_Move)
- UseSkill(S01_Move)
