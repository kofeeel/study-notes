# M_ElderPhase1_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   └── ➡️ Sequence [🛡️Blackboard(BattleStart)]
        │       ├── 📋 Blackboard(BattleStart)
        │       └── 🚶 MoveToTarget [🛡️TimeLimit(3.0s [StartTimer1])]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [DownCautionTimer1])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ➡️ Sequence [🛡️Blackboard(TS1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(TS1)
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️UseableTime]
                │   │   ├── 🚶 MoveToTarget
                │   │   └── ⚔️ UseSkill(WingSwing)
                │   └── ⚔️ UseSkill(WingSwing) [🛡️UseableTime && 🛡️CheckActorStat(ActorStatType_MaxHP>=80.0%)]
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=80)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_MaxHP<=98.0%)]
                │   ├── ⚔️ UseSkill(ParrySwing combo=TableCommand)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=500.0)]
                ├── ⚔️ UseSkill(WingSwing) [🛡️CheckActorStat(ActorStatType_MaxHP<=80.0%)]
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(BlinkB)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=500.0)]
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⚔️ UseSkill(BlinkL|BlinkR)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=500.0)]
                ├── ❓ Selector
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(8.0s [CautionTimer3]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️CheckActorEffect(NOT Self.M_Common_HitProjectileResult ON)]
                │   └── ⚔️ UseSkill(BlinkL2|BlinkR2)
                ├── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [CautionTimer4]) && 🛡️DistanceToTarget(dist<=2000.0) && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorEffect(NOT Self.M_Common_HitProjectileResult ON)]
                ├── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [CautionTimer4]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️CheckActorEffect(NOT Self.M_Common_HitProjectileResult ON)]
                └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Common_HitProjectileResult ON)]
                    ├── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [CautionTimer4]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=1000.0)]
                    ├── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [CautionTimer5]) && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️DistanceToTarget(dist<=2000.0)]
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/DistanceToTarget | 14 |
| Selector | 10 |
| Dec/TimeLimit | 10 |
| Task/UseSkill | 10 |
| Sequence | 9 |
| Task/CautionToTarget | 9 |
| Dec/CheckActorEffect | 7 |
| Task/UseableTimeReset | 4 |
| Dec/IsAlive | 3 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Task/MoveToTarget | 3 |
| Dec/UseableTime | 3 |
| Dec/CheckActorStat | 3 |
| Dec/Random | 3 |
| Dec/AggroLevel | 2 |
| Task/Wait | 2 |
| Dec/AimMe | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |

## 스킬 목록
- UseSkill(WingSwing)
- UseSkill(WingSwing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(ParrySwing combo=TableCommand)
- UseSkill(WingSwing)
- UseSkill(BlinkB)
- UseSkill(BlinkL|BlinkR)
- UseSkill(BlinkL2|BlinkR2)
