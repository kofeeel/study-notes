# M_HydraB_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
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
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=40) && 🛡️TimeLimit(2.666666s [Timer_BattleStartSkill]) && 🛡️DistanceToTarget(dist>=400.0)]
            │       │       ├── ⚔️ UseSkill(PokeAttack)
            │       │       └── ⚔️ UseSkill(Spit combo=TableCommand)
            │       ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_InvisibleOn ON)]
            │       │   ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️DistanceToTarget(dist>=500.0) && 🛡️TimeLimit(3.0s)]
            │       │   └── ⚔️ UseSkill(SpitTriple combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️UseableTime]
            │       │   ├── ⚔️ UseSkill(MoveBackLeft|MoveBackRight|MoveBack combo=TableCommand)
            │       │   └── ❓ Selector
            │       │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=300.0) && 🛡️UseableTime && 🛡️Random(rand(100)<=40) && 🛡️TimeLimit(3.0s)]
            │       │       └── ⚔️ UseSkill(Spit combo=TableCommand)
            │       ├── ⚔️ UseSkill(InkRunAttack combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=600.0)]
            │       │   ├── ⚔️ UseSkill(Spit combo=TableCommand)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(DashAttack combo=TableCommand)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️TimeLimit(3.0s) && 🛡️DistanceToTarget(dist<1400.0)]
            │       ├── ⚔️ UseSkill(PokeAttack combo=TableCommand)
            │       └── ➡️ Sequence
            │           ├── 🚶 MoveToTarget
            │           └── ⏳ Wait(1.4s)
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=40) && 🛡️TimeLimit(2.666666s [Timer_BattleStartSkill]) && 🛡️DistanceToTarget(dist>=400.0)]
                │       ├── ⚔️ UseSkill(PokeAttack)
                │       └── ⚔️ UseSkill(Spit combo=TableCommand)
                ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_InvisibleOn ON)]
                │   ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️DistanceToTarget(dist>=500.0) && 🛡️TimeLimit(3.0s)]
                │   └── ⚔️ UseSkill(HideRush|HideShot combo=TableCommand)
                ├── ⚔️ UseSkill(Hide combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   ├── ⚔️ UseSkill(InkRunAttack combo=TableCommand)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=300.0) && 🛡️UseableTime && 🛡️Random(rand(100)<=40) && 🛡️TimeLimit(3.0s)]
                │       └── ⚔️ UseSkill(Spit combo=TableCommand)
                ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   ├── ⚔️ UseSkill(MoveBackLeft|MoveBackRight|MoveBack combo=TableCommand)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=40) && 🛡️UseableTime && 🛡️TimeLimit(3.0s)]
                │       ├── ⚔️ UseSkill(SpitTriple combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │       └── ⚔️ UseSkill(Spit combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                ├── ⚔️ UseSkill(Hide combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   ├── ⚔️ UseSkill(SpitTriple combo=TableCommand)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   ├── ⚔️ UseSkill(Spit combo=TableCommand)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   ├── ⚔️ UseSkill(DashAttack combo=TableCommand)
                │   └── ⏳ WaitTimeRandom
                ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️TimeLimit(3.0s) && 🛡️DistanceToTarget(dist<1400.0)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   ├── ⚔️ UseSkill(PokeAttack combo=TableCommand)
                │   ├── ⚔️ UseSkill(MoveBackLeft|MoveBackRight|MoveBack combo=TableCommand)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=300.0) && 🛡️UseableTime && 🛡️Random(rand(100)<=40) && 🛡️TimeLimit(3.0s)]
                │       ├── ⚔️ UseSkill(SpitTriple combo=TableCommand) [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │       └── ⚔️ UseSkill(Spit combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                └── ➡️ Sequence
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 27 |
| Task/UseSkill | 26 |
| Selector | 19 |
| Sequence | 19 |
| Task/CautionToTarget | 13 |
| Dec/DistanceToTarget | 13 |
| Dec/TimeLimit | 12 |
| Dec/UseableTime | 11 |
| Dec/AggroLevel | 7 |
| Dec/Blackboard | 6 |
| Task/Blackboard | 6 |
| Task/WaitTimeRandom | 6 |
| Dec/Random | 6 |
| Dec/CheckActorStat | 6 |
| Task/UseableTimeReset | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Task/MoveToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(PokeAttack)
- UseSkill(Spit combo=TableCommand)
- UseSkill(SpitTriple combo=TableCommand)
- UseSkill(MoveBackLeft|MoveBackRight|MoveBack combo=TableCommand)
- UseSkill(Spit combo=TableCommand)
- UseSkill(InkRunAttack combo=TableCommand)
- UseSkill(Spit combo=TableCommand)
- UseSkill(DashAttack combo=TableCommand)
- UseSkill(PokeAttack combo=TableCommand)
- UseSkill(PokeAttack)
- UseSkill(Spit combo=TableCommand)
- UseSkill(HideRush|HideShot combo=TableCommand)
- UseSkill(Hide combo=TableCommand)
- UseSkill(InkRunAttack combo=TableCommand)
- UseSkill(Spit combo=TableCommand)
- UseSkill(MoveBackLeft|MoveBackRight|MoveBack combo=TableCommand)
- UseSkill(SpitTriple combo=TableCommand)
- UseSkill(Spit combo=TableCommand)
- UseSkill(Hide combo=TableCommand)
- UseSkill(SpitTriple combo=TableCommand)
- UseSkill(Spit combo=TableCommand)
- UseSkill(DashAttack combo=TableCommand)
- UseSkill(PokeAttack combo=TableCommand)
- UseSkill(MoveBackLeft|MoveBackRight|MoveBack combo=TableCommand)
- UseSkill(SpitTriple combo=TableCommand)
- UseSkill(Spit combo=TableCommand)
