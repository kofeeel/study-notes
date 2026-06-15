# M_MiteD_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
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
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           ├── ⏳ WaitTimeRandom
            │           ├── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            │           └── ⚔️ UseSkill(RollingAttack) [🛡️Random(rand(100)<=80) && 🛡️LockOnMe]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ➡️ Sequence
            │   │   ├── ⏳ WaitTimeRandom
            │   │   └── ⚔️ UseSkill(MoveSkill)
            │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=300.0)]
            │       ├── 🎬 PlayShow
            │       └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── 🎬 PlayShow [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │       │   └── ❓ Selector
            │       │       ├── 🎬 PlayShow [🛡️TimeLimit(3.0s [DownCautionTimer1])]
            │       │       └── 🚶 MoveToTarget
            │       ├── ➡️ Sequence [🛡️Blackboard(GrabTimer)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(GrabTimer)
            │       ├── ➡️ Sequence [🛡️Blackboard(GrabTimer) && 🛡️UseableTime && 🛡️LockOnMe]
            │       │   ├── ❓ Selector
            │       │   │   ├── ⚔️ UseSkill(Grab) [🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
            │       │   │   └── ⚔️ UseSkill(RollingAttack) [🛡️DistanceToTarget(dist>=400.0)]
            │       │   └── 📋 Blackboard(GrabTimer)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │       │   ├── 📋 Blackboard(FirstShot)
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=85)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=20) && 🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(Swing)
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=40) && 🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(TailStampDouble)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(TailSpinDouble)
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(MoveSkill combo=TableCommand)
            │       │   ├── ⏳ WaitTimeRandom
            │       │   └── ⚔️ UseSkill(Stamp) [🛡️UseableTime]
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
            │       │   ├── ⏳ WaitTimeRandom
            │       │   └── ⚔️ UseSkill(TailStampDouble)
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
            │       │   ├── ⏳ WaitTimeRandom
            │       │   ├── ⚔️ UseSkill(Swing)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ⚔️ UseSkill(TailSpinDouble)
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
                │   └── ❓ Selector
                │       ├── 🎬 PlayShow [🛡️TimeLimit(3.0s [DownCautionTimer1])]
                │       └── 🚶 MoveToTarget
                ├── ➡️ Sequence [🛡️Blackboard(GrabTimer)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(GrabTimer)
                ├── ➡️ Sequence [🛡️Blackboard(GrabTimer) && 🛡️UseableTime && 🛡️LockOnMe]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(Grab) [🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
                │   │   └── ⚔️ UseSkill(RollingAttack) [🛡️DistanceToTarget(dist>=400.0)]
                │   └── 📋 Blackboard(GrabTimer)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=85)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=20) && 🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(Swing)
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=40) && 🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(TailStampDouble)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(TailSpinDouble)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(MoveSkill combo=TableCommand)
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(Stamp) [🛡️UseableTime]
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(TailStampDouble)
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Check_WLMonster ON)]
                │   ├── ⏳ WaitTimeRandom
                │   ├── ⚔️ UseSkill(Swing)
                │   └── ⏳ WaitTimeRandom
                ├── ⚔️ UseSkill(TailSpinDouble)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 30 |
| Task/UseSkill | 28 |
| Dec/CheckActorEffect | 25 |
| Selector | 20 |
| Task/WaitTimeRandom | 12 |
| Dec/Random | 11 |
| Dec/Blackboard | 10 |
| Task/Blackboard | 10 |
| Task/MoveToTarget | 10 |
| Dec/AggroLevel | 7 |
| Task/PlayShow | 6 |
| Dec/TimeLimit | 4 |
| Task/UseableTimeReset | 4 |
| Dec/UseableTime | 4 |
| Dec/AimMe | 4 |
| Dec/IsAlive | 3 |
| Dec/LockOnMe | 3 |
| Dec/DistanceToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/CheckActorStat | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(RollingAttack)
- UseSkill(MoveSkill)
- UseSkill(Grab)
- UseSkill(RollingAttack)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Swing)
- UseSkill(TailStampDouble)
- UseSkill(TailSpinDouble)
- UseSkill(MoveSkill combo=TableCommand)
- UseSkill(Stamp)
- UseSkill(TailStampDouble)
- UseSkill(Swing)
- UseSkill(TailSpinDouble)
- UseSkill(Grab)
- UseSkill(RollingAttack)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Swing)
- UseSkill(TailStampDouble)
- UseSkill(TailSpinDouble)
- UseSkill(MoveSkill combo=TableCommand)
- UseSkill(Stamp)
- UseSkill(TailStampDouble)
- UseSkill(Swing)
- UseSkill(TailSpinDouble)
