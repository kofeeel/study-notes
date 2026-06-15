# M_Hydra_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector [🛡️Blackboard(CheckTriggerSignal)]
        │   └── ⚔️ UseSkill(DashAttack1_3)
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
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │       ├── 📋 Blackboard(BattleState)
            │       ├── 🎬 PlayShow
            │       └── ⏳ Wait(2.0s)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT) && 🛡️CheckActorEffect(Self.M_Common_Tutorial)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_Common_Tutorial)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️CheckActorEffect(Self.M_Common_InvisibleOn) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=35)]
            │       │       │   └── ⚔️ UseSkill(DashAttack1_1)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(AssaultAttack_1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️UseableTime]
            │       │   ├── ⚔️ UseSkill(MoveBackLeft1_1|MoveBackRight1_1|MoveBack1_1 combo=TableCommand)
            │       │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s) && 🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=40)]
            │       ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
            │       │   ├── ⚔️ UseSkill(InkRunAttack_1 combo=TableCommand)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(DashAttack1_1 combo=TableCommand)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ❓ Selector
            │       │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s)]
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(DashAttack2_1 combo=TableSkillFlag)
            │       │       └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(AssaultAttack_1|PokeAttack_1 combo=TableCommand)
            │       │   └── ⏳ WaitTimeRandom
            │       └── ➡️ Sequence
            │           ├── 🚶 MoveToTarget
            │           └── ⏳ Wait(1.4s)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_Common_Tutorial ON)]
            │   ├── ❓ Selector [🛡️Blackboard(Tutorial)]
            │   │   └── ⚔️ UseSkill(HideTutorial combo=TableCommand)
            │   └── ❓ Selector
            │       ├── ➡️ Sequence
            │       │   ├── 🚶 MoveToTarget
            │       │   ├── ⚔️ UseSkill(PokeAttack_1)
            │       │   └── 📋 Blackboard(Tutorial)
            │       └── ➡️ Sequence
            │           ├── ⚔️ UseSkill(PokeAttack_1)
            │           └── 📋 Blackboard(Tutorial)
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_Common_Tutorial)]
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️CheckActorEffect(Self.M_Common_InvisibleOn) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=35)]
                │       │   └── ⚔️ UseSkill(DashAttack1_1)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(AssaultAttack_1)
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_InvisibleOn ON)]
                │   ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️DistanceToTarget(dist>=500.0)]
                │   └── ⚔️ UseSkill(HideRush combo=TableCommand)
                ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   ├── ⚔️ UseSkill(InkRunAttack_1 combo=TableCommand)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s) && 🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=40)]
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⚔️ UseSkill(MoveBackLeft1_1|MoveBackRight1_1|MoveBack1_1 combo=TableCommand)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s) && 🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=40)]
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=50.0%) && 🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   └── ⚔️ UseSkill(Hide combo=TableCommand)
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   └── ⚔️ UseSkill(DashAttack1_1 combo=TableCommand)
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s)]
                │   └── ⚔️ UseSkill(DashAttack2_1 combo=TableSkillFlag) [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   ├── ⚔️ UseSkill(AssaultAttack_1 combo=TableCommand)
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s) && 🛡️DistanceToTarget(dist>=300.0)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.M_Common_InvisibleOn ON)]
                │   ├── ⚔️ UseSkill(PokeAttack_1 combo=TableCommand)
                │   ├── ⚔️ UseSkill(MoveBackLeft1_1|MoveBackRight1_1|MoveBack1_1 combo=TableCommand) [🛡️UseableTime]
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s) && 🛡️DistanceToTarget(dist>=300.0)]
                └── ➡️ Sequence
                    ├── 🚶 MoveToTarget
                    └── ⏳ Wait(1.4s)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 28 |
| Sequence | 26 |
| Task/UseSkill | 22 |
| Selector | 21 |
| Task/CautionToTarget | 11 |
| Dec/TimeLimit | 11 |
| Dec/Blackboard | 8 |
| Task/Blackboard | 8 |
| Dec/AggroLevel | 7 |
| Dec/DistanceToTarget | 6 |
| Task/Wait | 5 |
| Task/WaitTimeRandom | 5 |
| Dec/Random | 5 |
| Task/MoveToTarget | 5 |
| Task/UseableTimeReset | 5 |
| Dec/UseableTime | 5 |
| Dec/IsAlive | 3 |
| Dec/CheckActorStat | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |

## 스킬 목록
- UseSkill(DashAttack1_3)
- UseSkill(DashAttack1_1)
- UseSkill(AssaultAttack_1)
- UseSkill(MoveBackLeft1_1|MoveBackRight1_1|MoveBack1_1 combo=TableCommand)
- UseSkill(InkRunAttack_1 combo=TableCommand)
- UseSkill(DashAttack1_1 combo=TableCommand)
- UseSkill(DashAttack2_1 combo=TableSkillFlag)
- UseSkill(AssaultAttack_1|PokeAttack_1 combo=TableCommand)
- UseSkill(HideTutorial combo=TableCommand)
- UseSkill(PokeAttack_1)
- UseSkill(PokeAttack_1)
- UseSkill(DashAttack1_1)
- UseSkill(AssaultAttack_1)
- UseSkill(HideRush combo=TableCommand)
- UseSkill(InkRunAttack_1 combo=TableCommand)
- UseSkill(MoveBackLeft1_1|MoveBackRight1_1|MoveBack1_1 combo=TableCommand)
- UseSkill(Hide combo=TableCommand)
- UseSkill(DashAttack1_1 combo=TableCommand)
- UseSkill(DashAttack2_1 combo=TableSkillFlag)
- UseSkill(AssaultAttack_1 combo=TableCommand)
- UseSkill(PokeAttack_1 combo=TableCommand)
- UseSkill(MoveBackLeft1_1|MoveBackRight1_1|MoveBack1_1 combo=TableCommand)
