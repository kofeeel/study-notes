# M_Maelstrom_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            └── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_CheckStoryMode)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                └── ❓ Selector
                    ├── ❓ Selector [🛡️UseableTime]
                    │   ├── ⚔️ UseSkill(Nikke_RoarStrong combo=TableCommand subTarget) [🛡️UseableTime]
                    │   └── ⚔️ UseSkill(Nikke_SpitChasing|Nikke_SummonProjectile combo=TableCommand subTarget)
                    └── ➡️ Sequence
                        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                        └── ⚔️ UseSkill(Nikke_Spit|Nikke_VomitStraight combo=TableCommand subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 6 |
| Task/UseSkill | 3 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Sequence | 2 |
| Task/UseableTimeReset | 2 |
| Dec/UseableTime | 2 |
| Dec/CheckActorEffect | 1 |
| Dec/Blackboard | 1 |
| Task/Blackboard | 1 |

## 스킬 목록
- UseSkill(Nikke_RoarStrong combo=TableCommand subTarget)
- UseSkill(Nikke_SpitChasing|Nikke_SummonProjectile combo=TableCommand subTarget)
- UseSkill(Nikke_Spit|Nikke_VomitStraight combo=TableCommand subTarget)
