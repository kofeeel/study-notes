# M_GrubDashElite_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>100.0) && 🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       ├── 🎬 PlayShow
            │       └── ⏳ Wait(1.0s)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ➡️ Sequence [🛡️CheckActorEffect(Target.? ON)]
            │   ├── 🎬 PlayShow
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(KnockBack_Timer)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(KnockBack_Timer)
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⚔️ UseSkill(KnockbackAttack1_1 combo=TableCommand)
                │   └── 📋 Blackboard(KnockBack_Timer)
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⚔️ UseSkill(ComboAttack_1) [🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=60.0%)]
                │   └── ⏳ WaitTimeRandom
                ├── ⚔️ UseSkill(JumpStand_1)
                ├── ⚔️ UseSkill(RushAttack2_1|HeadAttack_1 combo=TableCommand)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(PowerfulHeadAttack_1|ShockWaveAttack_1 combo=TableCommand)
                │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=600.0) && 🛡️TimeLimit(2.0s)]
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 6 |
| Selector | 5 |
| Task/UseSkill | 5 |
| Dec/CheckActorEffect | 4 |
| Task/Blackboard | 3 |
| Dec/IsAlive | 2 |
| Dec/AggroLevel | 2 |
| Dec/DistanceToTarget | 2 |
| Dec/Blackboard | 2 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/UseableTime | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/IsGroupTarget | 1 |
| Task/UseableTimeReset | 1 |
| Dec/Random | 1 |
| Dec/CheckActorStat | 1 |
| Task/CautionToTarget | 1 |
| Dec/TimeLimit | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(KnockbackAttack1_1 combo=TableCommand)
- UseSkill(ComboAttack_1)
- UseSkill(JumpStand_1)
- UseSkill(RushAttack2_1|HeadAttack_1 combo=TableCommand)
- UseSkill(PowerfulHeadAttack_1|ShockWaveAttack_1 combo=TableCommand)
