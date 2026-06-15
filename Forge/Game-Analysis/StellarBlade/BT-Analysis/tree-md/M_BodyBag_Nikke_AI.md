# M_BodyBag_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ❓ Selector
                        ├── ⚔️ UseSkill(Nikke_SuicideBomb subTarget)
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 5 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Dec/AggroLevel | 1 |
| Sequence | 1 |
| Task/UseSkill | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(Nikke_SuicideBomb subTarget)
