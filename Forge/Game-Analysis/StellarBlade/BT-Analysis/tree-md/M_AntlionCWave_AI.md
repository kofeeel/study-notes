# M_AntlionCWave_AI

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
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │       │   ├── 📋 Blackboard(FirstShot) [🛡️Random(rand(100)<=50)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=70)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │       │   └── ⚠️ CautionToTarget
            │       ├── ➡️ Sequence
            │       │   ├── ⏳ WaitTimeRandom
            │       │   ├── ⚔️ UseSkill(PawAttack)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>600.0) && 🛡️Random(rand(100)<=60) && 🛡️UseableTime && 🛡️TimeLimit(3.0s [DownCautionTimer1])]
            │       └── 🚶 MoveToTarget
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
                │       │   ├── ⏳ WaitTimeRandom
                │       │   ├── ⚔️ UseSkill(SwingTriple)
                │       │   └── ⏳ WaitTimeRandom
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           ├── ⏳ WaitTimeRandom
                │           ├── ⚔️ UseSkill(PawAttack)
                │           └── ⏳ WaitTimeRandom
                ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
                │   └── ⚠️ CautionToTarget
                ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(Self.M_AntlionC_BlockRolling)]
                │   ├── ⚔️ UseSkill(RollingAttack)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SwingTriple)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(PawAttack)
                │   └── ⏳ WaitTimeRandom
                ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>600.0) && 🛡️Random(rand(100)<=50) && 🛡️UseableTime && 🛡️TimeLimit(3.0s [DownCautionTimer1])]
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 19 |
| Dec/CheckActorEffect | 17 |
| Selector | 14 |
| Task/UseSkill | 12 |
| Task/WaitTimeRandom | 10 |
| Dec/Random | 9 |
| Dec/AggroLevel | 7 |
| Dec/Blackboard | 7 |
| Task/Blackboard | 7 |
| Task/CautionToTarget | 7 |
| Dec/TimeLimit | 5 |
| Dec/AimMe | 4 |
| Task/MoveToTarget | 4 |
| Dec/IsAlive | 3 |
| Task/UseableTimeReset | 3 |
| Dec/UseableTime | 3 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/CheckActorStat | 2 |
| Dec/DistanceToTarget | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
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
