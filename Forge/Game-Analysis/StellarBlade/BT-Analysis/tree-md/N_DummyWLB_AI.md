# N_DummyWLB_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ➡️ Sequence [🛡️Blackboard(BattleStart)]
            │   ├── 📋 Blackboard(BattleStart)
            │   └── ⏳ WaitTimeRandom
            ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle)]
            │   └── 📋 Blackboard(BattleStart)
            └── ➡️ Sequence
                ├── ⚔️ UseSkill(ExplosionWLB) [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                └── ⏳ WaitTimeRandom
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 3 |
| Sequence | 3 |
| Dec/IsAlive | 2 |
| Task/Blackboard | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/AggroLevel | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/Blackboard | 1 |
| Task/UseSkill | 1 |

## 스킬 목록
- UseSkill(ExplosionWLB)
