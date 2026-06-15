# M_StatueB_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
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
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_StatueB_StanbyToNormal)]
            │   │       ├── ⚔️ UseSkill(StanbyToNormal combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   ├── ➡️ Sequence [🛡️CheckStance(M_StatueB_StanbyToAttack)]
            │   │   │   ├── ⚔️ UseSkill(StanbyToAttack combo=TableCommand)
            │   │   │   └── 📋 Blackboard(BattleStart)
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_StatueB_StanbyToAttack)]
            │   │       ├── ⚔️ UseSkill(StanbyToNormal combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence [🛡️CheckStance(M_StatueB_Default)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.46s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT) && 🛡️CheckActorEffect(Self.M_Common_Tutorial)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_Common_Tutorial)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_NoGuard)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB_NoGuard)
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueB_StanbyToNormal)]
            │       │   ├── 📋 Blackboard(BB_StanceCheck)
            │       │   └── ✨ UseEffect(['M_StatueB_StanbyToNormal'])
            │       ├── ➡️ Sequence [🛡️CheckStance(M_StatueB_StanbyToAttack) && 🛡️Blackboard(BB_StanceCheck)]
            │       │   ├── 📋 Blackboard(BB_StanceCheck)
            │       │   └── ✨ UseEffect(['M_StatueB_StanbyToAttack'])
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
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=200.0) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_StatueB_NoGuardCheck ON)]
            │       │   ├── ⚔️ UseSkill(RushSlash2)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_StatueB_NoGuardCheck ON)]
            │       │   ├── ⚔️ UseSkill(GroundExplosion)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
            │       │   ├── ⚔️ UseSkill(SwingCombo)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(RushSlash) [🛡️DistanceToTarget(dist>=200.0)]
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(Slash|SlashDouble|SlashTriple)
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=1000.0) && 🛡️DistanceToTarget(dist<1200.0) && 🛡️TimeLimit(2.5s [Timer1])]
            │       └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=200.0)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_Common_Tutorial ON)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BB_NoGuard)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(BB_NoGuard)
            │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │   │   ├── 📋 Blackboard(BattleStartSkill)
            │   │   └── ❓ Selector
            │   │       ├── ❓ Selector [🛡️Random(rand(100)<=50)]
            │   │       │   ├── ➡️ Sequence
            │   │       │   │   ├── 🚶 MoveToTarget
            │   │       │   │   └── ⚔️ UseSkill(SlashDouble)
            │   │       │   └── ⚔️ UseSkill(SlashDouble)
            │   │       └── ❓ Selector
            │   │           ├── ➡️ Sequence
            │   │           │   ├── 🚶 MoveToTarget
            │   │           │   └── ⚔️ UseSkill(Slash)
            │   │           └── ⚔️ UseSkill(Slash)
            │   ├── ❓ Selector [🛡️UseableTime]
            │   │   ├── ➡️ Sequence
            │   │   │   ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=450.0)]
            │   │   │   └── ⚔️ UseSkill(RushSlash2_Tutorial)
            │   │   └── ⚔️ UseSkill(RushSlash2_Tutorial)
            │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=200.0) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_StatueB_NoGuardCheck ON)]
            │   │   ├── ⚔️ UseSkill(RushSlash2_Tutorial)
            │   │   └── ⏳ WaitTimeRandom
            │   ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_StatueB_NoGuardCheck ON)]
            │   │   ├── ⚔️ UseSkill(GroundExplosion)
            │   │   └── ⏳ WaitTimeRandom
            │   ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorStat(ActorStatType_HP<=65.0%)]
            │   │   ├── ⚔️ UseSkill(SwingCombo)
            │   │   └── ⏳ WaitTimeRandom
            │   ├── ❓ Selector
            │   │   ├── ➡️ Sequence
            │   │   │   ├── ⚔️ UseSkill(RushSlash) [🛡️DistanceToTarget(dist>=200.0)]
            │   │   │   └── ⏳ WaitTimeRandom
            │   │   ├── ➡️ Sequence
            │   │   │   ├── ⚔️ UseSkill(Slash|SlashDouble|SlashTriple)
            │   │   │   └── ⏳ WaitTimeRandom
            │   │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=1000.0) && 🛡️DistanceToTarget(dist<1200.0) && 🛡️TimeLimit(2.5s [Timer1])]
            │   └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=200.0)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_Common_Tutorial)]
                ├── ➡️ Sequence [🛡️Blackboard(BB_NoGuard)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB_NoGuard)
                ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueB_StanbyToNormal)]
                │   ├── 📋 Blackboard(BB_StanceCheck)
                │   └── ✨ UseEffect(['M_StatueB_StanbyToNormal'])
                ├── ➡️ Sequence [🛡️Blackboard(BB_StanceCheck) && 🛡️CheckStance(M_StatueB_StanbyToAttack)]
                │   ├── 📋 Blackboard(BB_StanceCheck)
                │   └── ✨ UseEffect(['M_StatueB_StanbyToAttack'])
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
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=200.0) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_StatueB_NoGuardCheck ON)]
                │   ├── ⚔️ UseSkill(RushSlash2)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_StatueB_NoGuardCheck ON)]
                │   ├── ⚔️ UseSkill(GroundExplosion)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                │   ├── ⚔️ UseSkill(SwingCombo)
                │   └── ⏳ WaitTimeRandom
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(RushSlash) [🛡️DistanceToTarget(dist>=200.0)]
                │   │   └── ⏳ WaitTimeRandom
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(Slash|SlashDouble|SlashTriple)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [Timer1]) && 🛡️DistanceToTarget(dist>=1000.0) && 🛡️DistanceToTarget(dist<1200.0)]
                └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=200.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 41 |
| Task/UseSkill | 30 |
| Dec/CheckActorEffect | 26 |
| Selector | 24 |
| Task/WaitTimeRandom | 16 |
| Dec/DistanceToTarget | 16 |
| Task/Blackboard | 15 |
| Dec/Blackboard | 14 |
| Task/MoveToTarget | 12 |
| Dec/UseableTime | 10 |
| Dec/AggroLevel | 8 |
| Dec/CheckStance | 8 |
| Dec/TimeLimit | 8 |
| Task/UseableTimeReset | 7 |
| Task/CautionToTarget | 6 |
| Dec/Random | 5 |
| Task/UseEffect | 4 |
| Dec/IsAlive | 3 |
| Dec/CheckActorStat | 3 |
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

## 스킬 목록
- UseSkill(StanbyToNormal combo=TableCommand)
- UseSkill(StanbyToAttack combo=TableCommand)
- UseSkill(StanbyToNormal combo=TableCommand)
- UseSkill(RushSlash)
- UseSkill(SlashDouble)
- UseSkill(Slash)
- UseSkill(RushSlash2)
- UseSkill(GroundExplosion)
- UseSkill(SwingCombo)
- UseSkill(RushSlash)
- UseSkill(Slash|SlashDouble|SlashTriple)
- UseSkill(SlashDouble)
- UseSkill(SlashDouble)
- UseSkill(Slash)
- UseSkill(Slash)
- UseSkill(RushSlash2_Tutorial)
- UseSkill(RushSlash2_Tutorial)
- UseSkill(RushSlash2_Tutorial)
- UseSkill(GroundExplosion)
- UseSkill(SwingCombo)
- UseSkill(RushSlash)
- UseSkill(Slash|SlashDouble|SlashTriple)
- UseSkill(RushSlash)
- UseSkill(SlashDouble)
- UseSkill(Slash)
- UseSkill(RushSlash2)
- UseSkill(GroundExplosion)
- UseSkill(SwingCombo)
- UseSkill(RushSlash)
- UseSkill(Slash|SlashDouble|SlashTriple)
