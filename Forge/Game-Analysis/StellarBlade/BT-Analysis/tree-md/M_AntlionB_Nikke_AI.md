# M_AntlionB_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector
        │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── 🚶 MoveToTarget
                └── ❓ Selector [🛡️DistanceToTarget(dist<=1200.0)]
                    ├── ➡️ Sequence [🛡️Blackboard(BB1) && 🛡️DistanceToTarget(dist<=1200.0)]
                    │   ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    │   ├── 🎬 PlayShow
                    │   └── 📋 Blackboard(BB1)
                    ├── ⚔️ UseSkill(Nikke_SuicideRun combo=TableCommand subTarget)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 6 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Task/MoveToTarget | 2 |
| Dec/DistanceToTarget | 2 |
| Dec/AggroLevel | 1 |
| Sequence | 1 |
| Dec/Blackboard | 1 |
| Task/PlayShow | 1 |
| Task/Blackboard | 1 |
| Task/UseSkill | 1 |

## 스킬 목록
- UseSkill(Nikke_SuicideRun combo=TableCommand subTarget)
