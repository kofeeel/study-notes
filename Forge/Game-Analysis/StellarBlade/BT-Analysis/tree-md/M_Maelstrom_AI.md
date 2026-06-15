# M_Maelstrom_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── 🎬 PlayShow [🛡️TimeLimit(2.0s [AbnormalTimer])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ❓ Selector [🛡️UseableTime]
                │   ├── ✨ UseEffect(['M_Maelstrom_SummonSingle_AYL']) [🛡️CheckActorEffect(Self.M_Maelstrom_CheckAYL ON)]
                │   └── ✨ UseEffect(['M_Maelstrom_SummonSingle']) [🛡️CheckActorEffect(NOT Self.M_Maelstrom_CheckAYL ON)]
                ├── ❓ Selector [🛡️UseableTime && 🛡️DistanceToTarget(dist<800.0) && 🛡️UseableTime]
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(Roar)
                │       ├── ⚔️ UseSkill(SpitChasing)
                │       ├── ⏳ WaitTimeRandom
                │       └── ⚔️ UseSkill(Spit|VomitStraight)
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist<=800.0)]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(Vomit) [🛡️Random(rand(100)<=90)]
                │   │   └── ⚔️ UseSkill(VomitStraight)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Maelstrom_BlockRoar)]
                │   ├── ⚔️ UseSkill(Roar)
                │   └── ⏳ WaitTimeRandom
                ├── ❓ Selector [🛡️UseableTime]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Maelstrom_CheckAYL ON)]
                │   │   ├── ⚔️ UseSkill(SummonProjectile2)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Maelstrom_CheckAYL ON)]
                │       ├── ⚔️ UseSkill(SummonProjectile)
                │       └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(Self.M_Maelstrom_BlockRoar)]
                │   ├── ⚔️ UseSkill(Roar2)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⚔️ UseSkill(SpitChasing)
                │   ├── ⏳ WaitTimeRandom
                │   ├── ⚔️ UseSkill(Spit|VomitStraight)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⚔️ UseSkill(VomitStraight)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SpitMulti)
                │   └── ⏳ WaitTimeRandom
                └── ➡️ Sequence
                    ├── ⚔️ UseSkill(Spit)
                    └── ⏳ WaitTimeRandom
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 14 |
| Sequence | 11 |
| Task/WaitTimeRandom | 11 |
| Selector | 10 |
| Dec/CheckActorEffect | 8 |
| Dec/UseableTime | 7 |
| Task/UseableTimeReset | 5 |
| Dec/IsAlive | 3 |
| Task/Wait | 2 |
| Task/UseEffect | 2 |
| Dec/DistanceToTarget | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/PlayShow | 1 |
| Dec/TimeLimit | 1 |
| Dec/AggroLevel | 1 |
| Dec/Blackboard | 1 |
| Task/Blackboard | 1 |
| Dec/Random | 1 |

## 스킬 목록
- UseSkill(Roar)
- UseSkill(SpitChasing)
- UseSkill(Spit|VomitStraight)
- UseSkill(Vomit)
- UseSkill(VomitStraight)
- UseSkill(Roar)
- UseSkill(SummonProjectile2)
- UseSkill(SummonProjectile)
- UseSkill(Roar2)
- UseSkill(SpitChasing)
- UseSkill(Spit|VomitStraight)
- UseSkill(VomitStraight)
- UseSkill(SpitMulti)
- UseSkill(Spit)
