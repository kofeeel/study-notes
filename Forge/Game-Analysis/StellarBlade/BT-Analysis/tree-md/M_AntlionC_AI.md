# M_AntlionC_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleState) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleState)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s [DownCautionTimer1])]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️IsGroupTarget]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_Common_CheckWave)]
            │   │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │   │   │   ├── 🔄 UseableTimeReset
            │   │   │   └── 📋 Blackboard(BB1)
            │   │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   │   ├── 📋 Blackboard(FirstShot) [🛡️Random(rand(100)<=50)]
            │   │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=70)]
            │   │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
            │   │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │   │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │   │   │   ├── 📋 Blackboard(BattleStartSkill)
            │   │   │   └── ❓ Selector
            │   │   │       ├── ➡️ Sequence [🛡️Random(rand(100)<=30)]
            │   │   │       │   ├── 🚶 MoveToTarget
            │   │   │       │   └── ⚔️ UseSkill(SwingTriple)
            │   │   │       └── ➡️ Sequence
            │   │   │           ├── 🚶 MoveToTarget
            │   │   │           └── ⚔️ UseSkill(PawAttack)
            │   │   ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   │   │   └── ⚠️ CautionToTarget
            │   │   ├── ➡️ Sequence
            │   │   │   ├── ⚔️ UseSkill(PawAttack)
            │   │   │   └── ⏳ WaitTimeRandom
            │   │   ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>600.0) && 🛡️DistanceToTarget(dist<800.0) && 🛡️Random(rand(100)<=50) && 🛡️UseableTime && 🛡️TimeLimit(3.0s [DownCautionTimer1])]
            │   │   └── 🚶 MoveToTarget
            │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_CheckWave ON)]
            │       ├── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️LockOnMe]
            │       │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   │   ├── 🔄 UseableTimeReset
            │       │   │   └── 📋 Blackboard(BB1)
            │       │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │       │   │   ├── 📋 Blackboard(FirstShot) [🛡️Random(rand(100)<=50)]
            │       │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=70)]
            │       │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
            │       │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   │   └── ❓ Selector
            │       │   │       ├── ➡️ Sequence [🛡️Random(rand(100)<=30)]
            │       │   │       │   ├── 🚶 MoveToTarget
            │       │   │       │   └── ⚔️ UseSkill(SwingTriple)
            │       │   │       └── ➡️ Sequence
            │       │   │           ├── 🚶 MoveToTarget
            │       │   │           └── ⚔️ UseSkill(PawAttack)
            │       │   ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │       │   │   └── ⚠️ CautionToTarget
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(PawAttack)
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>600.0) && 🛡️DistanceToTarget(dist<800.0) && 🛡️Random(rand(100)<=50) && 🛡️UseableTime && 🛡️TimeLimit(3.0s [DownCautionTimer1])]
            │       │   └── 🚶 MoveToTarget
            │       └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️LockOnMe(NOT)]
            │           ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │           │   ├── 🔄 UseableTimeReset
            │           │   └── 📋 Blackboard(BB1)
            │           ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │           │   ├── 📋 Blackboard(FirstShot) [🛡️Random(rand(100)<=50)]
            │           │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │           ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=70)]
            │           │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │           ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
            │           │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │           ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │           │   └── ⚠️ CautionToTarget
            │           ├── ❓ Selector [🛡️TimeLimit(5.0s)]
            │           │   └── ⚠️ CautionToTarget
            │           ├── ➡️ Sequence
            │           │   ├── ⏳ WaitTimeRandom
            │           │   ├── ⚔️ UseSkill(PawAttack)
            │           │   └── ⏳ WaitTimeRandom
            │           ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>600.0) && 🛡️DistanceToTarget(dist<800.0) && 🛡️Random(rand(100)<=50) && 🛡️UseableTime && 🛡️TimeLimit(3.0s [DownCautionTimer1])]
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
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
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=30)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(SwingTriple)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(PawAttack)
                ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
                │   └── ⚠️ CautionToTarget
                ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(Self.M_AntlionC_BlockRolling)]
                │   ├── ⚔️ UseSkill(RollingAttack)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SwingTriple)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(PawAttack)
                │   └── ⏳ WaitTimeRandom
                ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>600.0) && 🛡️DistanceToTarget(dist<1200.0) && 🛡️Random(rand(100)<=50) && 🛡️UseableTime && 🛡️TimeLimit(3.0s [DownCautionTimer1])]
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 34 |
| Dec/CheckActorEffect | 28 |
| Task/UseSkill | 24 |
| Selector | 23 |
| Dec/Random | 19 |
| Dec/Blackboard | 13 |
| Task/Blackboard | 13 |
| Task/CautionToTarget | 12 |
| Task/MoveToTarget | 11 |
| Dec/TimeLimit | 10 |
| Dec/AggroLevel | 9 |
| Task/WaitTimeRandom | 8 |
| Dec/AimMe | 8 |
| Dec/DistanceToTarget | 8 |
| Task/UseableTimeReset | 5 |
| Dec/UseableTime | 5 |
| Dec/IsGroupAttacker | 4 |
| Dec/CheckActorStat | 4 |
| Dec/IsAlive | 3 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/LockOnMe | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(SwingTriple)
- UseSkill(PawAttack)
- UseSkill(PawAttack)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(SwingTriple)
- UseSkill(PawAttack)
- UseSkill(PawAttack)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(PawAttack)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(SwingTriple)
- UseSkill(PawAttack)
- UseSkill(RollingAttack)
- UseSkill(SwingTriple)
- UseSkill(PawAttack)
