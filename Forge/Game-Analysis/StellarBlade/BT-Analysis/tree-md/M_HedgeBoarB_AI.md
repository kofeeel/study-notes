# M_HedgeBoarB_AI

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
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=40)]
            │       │       │   └── ⚔️ UseSkill(LeapAttack)
            │       │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=40)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(LeapAttack)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(Smash|RushCombo)
            │       ├── ❓ Selector [🛡️CheckActorEffect(Self.M_HedgeBoarB_HitResult ON)]
            │       │   └── ⚔️ UseSkill(GuardCounter)
            │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0)]
            │       │   └── ⚔️ UseSkill(LeapAttack)
            │       ├── ❓ Selector [🛡️UseableTime]
            │       │   └── ⚔️ UseSkill(ChargeAttack)
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(Smash|RushCombo|HeadButt)
            │       │   └── ❓ Selector
            │       │       ├── ⚔️ UseSkill(SpinCombo) [🛡️UseableTime]
            │       │       └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=50)]
                │       │   └── ⚔️ UseSkill(LeapAttack)
                │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=300.0) && 🛡️Random(rand(100)<=50)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(LeapAttack)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(Smash|RushCombo)
                ├── ❓ Selector [🛡️CheckActorEffect(Self.M_HedgeBoarB_HitResult ON)]
                │   └── ⚔️ UseSkill(GuardCounter)
                ├── ❓ Selector [🛡️DistanceToTarget(dist>=300.0)]
                │   └── ⚔️ UseSkill(LeapAttack)
                ├── ❓ Selector [🛡️UseableTime]
                │   └── ⚔️ UseSkill(ChargeAttack)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(Smash|RushCombo|HeadButt)
                │   └── ❓ Selector
                │       ├── ⚔️ UseSkill(SpinCombo) [🛡️UseableTime]
                │       └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 25 |
| Task/UseSkill | 16 |
| Dec/CheckActorEffect | 15 |
| Sequence | 14 |
| Dec/AggroLevel | 7 |
| Task/MoveToTarget | 7 |
| Dec/Blackboard | 6 |
| Task/Blackboard | 6 |
| Task/UseableTimeReset | 6 |
| Dec/DistanceToTarget | 6 |
| Dec/TimeLimit | 4 |
| Dec/Random | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Task/CautionToTarget | 3 |
| Task/WaitTimeRandom | 3 |
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
- UseSkill(LeapAttack)
- UseSkill(LeapAttack)
- UseSkill(Smash|RushCombo)
- UseSkill(GuardCounter)
- UseSkill(LeapAttack)
- UseSkill(ChargeAttack)
- UseSkill(Smash|RushCombo|HeadButt)
- UseSkill(SpinCombo)
- UseSkill(LeapAttack)
- UseSkill(LeapAttack)
- UseSkill(Smash|RushCombo)
- UseSkill(GuardCounter)
- UseSkill(LeapAttack)
- UseSkill(ChargeAttack)
- UseSkill(Smash|RushCombo|HeadButt)
- UseSkill(SpinCombo)
