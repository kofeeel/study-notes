# M_BotRange_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── ⚔️ UseSkill(BattleEnd) [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.3s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.3s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ❓ Selector
            │       │   └── ⚔️ UseSkill(Turn_L|Turn_R)
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(SwingTail)
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_BotRange_BackMoveGlobalCoolTime ON)]
            │       │       ├── ➡️ Sequence
            │       │       │   ├── ⚔️ UseSkill(MoveBack)
            │       │       │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer])]
            │       │       └── ⚔️ UseSkill(ShotBack)
            │       ├── ❓ Selector
            │       │   ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>300.0) && 🛡️TimeLimit(3.0s [CautionTimer]) && 🛡️DistanceToTarget(dist<1400.0)]
            │       │   └── ⚔️ UseSkill(SwingTail|ShotBack) [🛡️CheckActorEffect(NOT Self.M_BotRange_BackMoveGlobalCoolTime ON)]
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>350.0)]
            │       │   ├── ⏳ WaitTimeRandom
            │       │   └── ⚔️ UseSkill(ShotLeft|ShotRight)
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>350.0) && 🛡️CheckActorEffect(NOT Self.M_BotRange_NoShotCheck ON)]
            │       │   ├── ⏳ WaitTimeRandom
            │       │   └── ⚔️ UseSkill(Shot)
            │       └── ❓ Selector
            │           ├── 🚶 MoveToTarget [🛡️UseableTime && 🛡️Random(rand(100)<=60) && 🛡️DistanceToTarget(dist<=500.0)]
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ❓ Selector
                │   └── ⚔️ UseSkill(Turn_L|Turn_R)
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(SwingTail)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_BotRange_BackMoveGlobalCoolTime ON)]
                │       ├── ⚔️ UseSkill(MoveBack|ShotBack)
                │       └── ⚠️ CautionToTarget [🛡️UseableTime]
                ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>300.0)]
                │   │   ├── ⚔️ UseSkill(LaserSpot)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=1000.0)]
                │       ├── ⚔️ UseSkill(LaserHoming)
                │       └── ⏳ WaitTimeRandom
                ├── ❓ Selector [🛡️Random(rand(100)<=60)]
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer]) && 🛡️DistanceToTarget(dist>300.0) && 🛡️DistanceToTarget(dist<1600.0)]
                │   └── ⚔️ UseSkill(MoveBack|ShotBack) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_BotRange_BackMoveGlobalCoolTime ON)]
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>350.0)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(ShotLeft|ShotRight)
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>350.0) && 🛡️CheckActorEffect(NOT Self.M_BotRange_NoShotCheck ON)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(Shot)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 20 |
| Dec/CheckActorEffect | 18 |
| Sequence | 16 |
| Task/UseSkill | 16 |
| Dec/DistanceToTarget | 11 |
| Task/WaitTimeRandom | 9 |
| Dec/AggroLevel | 7 |
| Task/CautionToTarget | 7 |
| Task/UseableTimeReset | 6 |
| Dec/TimeLimit | 5 |
| Dec/Blackboard | 4 |
| Task/Blackboard | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Task/MoveToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/Random | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/PlayShow | 1 |
| Dec/CheckActorState | 1 |
| Dec/CheckActorStat | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(Turn_L|Turn_R)
- UseSkill(SwingTail)
- UseSkill(MoveBack)
- UseSkill(ShotBack)
- UseSkill(SwingTail|ShotBack)
- UseSkill(ShotLeft|ShotRight)
- UseSkill(Shot)
- UseSkill(Turn_L|Turn_R)
- UseSkill(SwingTail)
- UseSkill(MoveBack|ShotBack)
- UseSkill(LaserSpot)
- UseSkill(LaserHoming)
- UseSkill(MoveBack|ShotBack)
- UseSkill(ShotLeft|ShotRight)
- UseSkill(Shot)
