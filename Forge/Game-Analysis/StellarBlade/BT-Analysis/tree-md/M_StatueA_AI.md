# M_StatueA_AI

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
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_StatueA_StanbyToNormal)]
            │   │       ├── ⚔️ UseSkill(StanbyToNormal combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   ├── ➡️ Sequence [🛡️CheckStance(M_StatueA_StanbyToAttack)]
            │   │   │   ├── ⚔️ UseSkill(StanbyToAttack combo=TableCommand)
            │   │   │   └── 📋 Blackboard(BattleStart)
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_StatueA_StanbyToAttack)]
            │   │       ├── ⚔️ UseSkill(StanbyToNormal combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_StatueA_StanbyToNormal2)]
            │   │       ├── ⚔️ UseSkill(StanbyToNormal2 combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   ├── ➡️ Sequence [🛡️CheckStance(M_StatueA_StanbyToGrab)]
            │   │   │   ├── ⚔️ UseSkill(StanbyToGrab combo=TableCommand)
            │   │   │   └── 📋 Blackboard(BattleStart)
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_StatueA_StanbyToGrab)]
            │   │       ├── ⚔️ UseSkill(StanbyToNormal2 combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_StatueA_StanbyToNormal3)]
            │   │       ├── ⚔️ UseSkill(StanbyToNormal3 combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence [🛡️CheckStance(M_StatueA_Default)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.46s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToNormal)]
            │       │   ├── 📋 Blackboard(BB_StanceCheck)
            │       │   └── ✨ UseEffect(['M_StatueA_StanbyToNormal'])
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToAttack)]
            │       │   ├── 📋 Blackboard(BB_StanceCheck)
            │       │   └── ✨ UseEffect(['M_StatueA_StanbyToAttack'])
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToNormal2)]
            │       │   ├── 📋 Blackboard(BB_StanceCheck)
            │       │   └── ✨ UseEffect(['M_StatueA_StanbyToNormal2'])
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToGrab)]
            │       │   ├── 📋 Blackboard(BB_StanceCheck)
            │       │   └── ✨ UseEffect(['M_StatueA_StanbyToGrab'])
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToNormal3)]
            │       │   ├── 📋 Blackboard(BB_StanceCheck)
            │       │   └── ✨ UseEffect(['M_StatueA_StanbyToNormal3'])
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=25)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(RushSlash)
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=35)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(SlashDouble)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(Slash)
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(RushSlash) [🛡️DistanceToTarget(dist>=200.0)]
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(Slash|SlashDouble)
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=1000.0) && 🛡️DistanceToTarget(dist<1200.0) && 🛡️TimeLimit(2.5s [Timer1])]
            │       └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=300.0)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToNormal)]
                │   ├── 📋 Blackboard(BB_StanceCheck)
                │   └── ✨ UseEffect(['M_StatueA_StanbyToNormal'])
                ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToAttack)]
                │   ├── 📋 Blackboard(BB_StanceCheck)
                │   └── ✨ UseEffect(['M_StatueA_StanbyToAttack'])
                ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToNormal2)]
                │   ├── 📋 Blackboard(BB_StanceCheck)
                │   └── ✨ UseEffect(['M_StatueA_StanbyToNormal2'])
                ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToGrab)]
                │   ├── 📋 Blackboard(BB_StanceCheck)
                │   └── ✨ UseEffect(['M_StatueA_StanbyToGrab'])
                ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueA_StanbyToNormal3)]
                │   ├── 📋 Blackboard(BB_StanceCheck)
                │   └── ✨ UseEffect(['M_StatueA_StanbyToNormal2'])
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=25)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(RushSlash)
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=35)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(SlashDouble)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(Slash)
                ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%)]
                │   ├── ⚔️ UseSkill(Grab)
                │   └── ⏳ WaitTimeRandom
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(RushSlash) [🛡️DistanceToTarget(dist>=200.0)]
                │   │   └── ⏳ WaitTimeRandom
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(Slash|SlashDouble)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=1000.0) && 🛡️DistanceToTarget(dist<1200.0) && 🛡️TimeLimit(2.5s [Timer1])]
                └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=300.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 34 |
| Selector | 21 |
| Task/Blackboard | 21 |
| Dec/Blackboard | 19 |
| Dec/CheckStance | 18 |
| Task/UseSkill | 18 |
| Dec/CheckActorEffect | 17 |
| Task/UseEffect | 10 |
| Task/MoveToTarget | 8 |
| Dec/DistanceToTarget | 8 |
| Dec/AggroLevel | 7 |
| Dec/TimeLimit | 6 |
| Task/WaitTimeRandom | 6 |
| Task/CautionToTarget | 5 |
| Dec/Random | 4 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Dec/CheckActorStat | 1 |

## 스킬 목록
- UseSkill(StanbyToNormal combo=TableCommand)
- UseSkill(StanbyToAttack combo=TableCommand)
- UseSkill(StanbyToNormal combo=TableCommand)
- UseSkill(StanbyToNormal2 combo=TableCommand)
- UseSkill(StanbyToGrab combo=TableCommand)
- UseSkill(StanbyToNormal2 combo=TableCommand)
- UseSkill(StanbyToNormal3 combo=TableCommand)
- UseSkill(RushSlash)
- UseSkill(SlashDouble)
- UseSkill(Slash)
- UseSkill(RushSlash)
- UseSkill(Slash|SlashDouble)
- UseSkill(RushSlash)
- UseSkill(SlashDouble)
- UseSkill(Slash)
- UseSkill(Grab)
- UseSkill(RushSlash)
- UseSkill(Slash|SlashDouble)
