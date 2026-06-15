# M_DummyElderPhase2_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ⚔️ UseSkill(ShotTest1|ShotTest2|ShotTest3|ShotTest4|ShotTest5|ShotTest6) [🛡️CheckActorEffect(Self.DummyElderPhase2_TestSkillUse ON)]
                └── ⚔️ UseSkill(Shot)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 4 |
| Dec/IsAlive | 2 |
| Task/UseSkill | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/IsGroupTarget | 1 |
| Dec/AggroLevel | 1 |
| Dec/CheckActorEffect | 1 |

## 스킬 목록
- UseSkill(ShotTest1|ShotTest2|ShotTest3|ShotTest4|ShotTest5|ShotTest6)
- UseSkill(Shot)
