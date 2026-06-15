# M_HedgeBoarBrute_AI_Tutorial

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
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget
                │   └── ❓ Selector
                │       └── ⚔️ UseSkill(SpinSwingLeft|SpinSwingRight)
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget
                │   └── ❓ Selector [🛡️DistanceToTarget(dist>=350.0) && 🛡️UseableTime]
                │       └── ⚔️ UseSkill(DashSwing)
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget
                │   └── ❓ Selector [🛡️UseableTime]
                │       └── ⚔️ UseSkill(Smash)
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget
                │   └── ❓ Selector [🛡️UseableTime && 🛡️CheckActorStat(ActorStatType_HP<=60.0%)]
                │       └── ⚔️ UseSkill(SpinCombo)
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget
                │   └── ❓ Selector [🛡️UseableTime]
                │       └── ⚔️ UseSkill(SwingCombo)
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget
                │   └── ❓ Selector [🛡️DistanceToTarget(dist>=350.0)]
                │       └── 🚶 MoveToTarget
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget
                │   └── ❓ Selector [🛡️UseableTime]
                │       └── ⚔️ UseSkill(SwingDouble)
                ├── ➡️ Sequence
                │   ├── 👁️ DetectTarget
                │   └── ➡️ Sequence
                │       └── ⚔️ UseSkill(Uppercut|Swing)
                └── ❓ Selector
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 14 |
| Sequence | 10 |
| Task/DetectTarget | 9 |
| Task/UseSkill | 7 |
| Dec/UseableTime | 5 |
| Task/UseableTimeReset | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 2 |
| Dec/CheckActorEffect | 2 |
| Dec/DistanceToTarget | 2 |
| Task/MoveToTarget | 2 |
| Dec/DetectResult | 1 |
| Task/CautionToTarget | 1 |
| Dec/TimeLimit | 1 |
| Dec/AggroLevel | 1 |
| Dec/Blackboard | 1 |
| Task/Blackboard | 1 |
| Dec/CheckActorStat | 1 |

## 스킬 목록
- UseSkill(SpinSwingLeft|SpinSwingRight)
- UseSkill(DashSwing)
- UseSkill(Smash)
- UseSkill(SpinCombo)
- UseSkill(SwingCombo)
- UseSkill(SwingDouble)
- UseSkill(Uppercut|Swing)
