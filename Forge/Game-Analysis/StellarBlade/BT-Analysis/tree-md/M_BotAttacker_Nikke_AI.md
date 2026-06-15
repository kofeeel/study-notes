# M_BotAttacker_Nikke_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ❓ Selector [🛡️UseableTime]
                │   └── ⚔️ UseSkill(Nikke_ChargeShot) [🛡️CheckActorEffect(Self.M_BotAttacker_Nikke_MissileCheck)]
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⚔️ UseSkill(Nikke_MissileMultipleShot) [🛡️CheckActorEffect(NOT Self.M_BotAttacker_Nikke_MissileCheck ON)]
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(Nikke_MissileShot) [🛡️CheckActorEffect(Self.M_BotAttacker_Nikke_MissileCheck)]
                │   └── ⏳ WaitTimeRandom
                └── ➡️ Sequence
                    ├── 👁️ DetectTarget [🛡️DetectResult(==)]
                    └── ❓ Selector
                        └── ⚔️ UseSkill(Nikke_LaserShot subTarget)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 6 |
| Dec/CheckActorEffect | 4 |
| Sequence | 4 |
| Task/UseSkill | 4 |
| Task/UseableTimeReset | 3 |
| Dec/IsAlive | 2 |
| Task/DetectTarget | 2 |
| Dec/DetectResult | 2 |
| Dec/UseableTime | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/Blackboard | 1 |
| Task/Blackboard | 1 |

## 스킬 목록
- UseSkill(Nikke_ChargeShot)
- UseSkill(Nikke_MissileMultipleShot)
- UseSkill(Nikke_MissileShot)
- UseSkill(Nikke_LaserShot subTarget)
