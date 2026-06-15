# M_Behemoth_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(BB1)
                    ├── ❓ Selector [🛡️UseableTime]
                    │   └── ⚔️ UseSkill(Nikke_ChargeShot)
                    └── ➡️ Sequence
                        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                        └── ❓ Selector
                            ├── ⚔️ UseSkill(Nikke_TornadoShot subTarget) [🛡️UseableTime]
                            └── ⚔️ UseSkill(Nikke_ShotSingle|Nikke_MoveShotChainLeft|Nikke_MoveShotChainRight subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 7 |
| Task/UseSkill | 3 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Sequence | 2 |
| Task/UseableTimeReset | 2 |
| Dec/UseableTime | 2 |
| Dec/AggroLevel | 1 |
| Dec/CheckActorEffect | 1 |
| Dec/Blackboard | 1 |
| Task/Blackboard | 1 |

## 스킬 목록
- UseSkill(Nikke_ChargeShot)
- UseSkill(Nikke_TornadoShot subTarget)
- UseSkill(Nikke_ShotSingle|Nikke_MoveShotChainLeft|Nikke_MoveShotChainRight subTarget)
