# M_HunchbackB_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_HunchbackB_StandbySitting)]
            │   │       ├── ⚔️ UseSkill(StandbyToNormal combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent) && 🛡️CheckStance(NOT M_HunchbackB_StandbySitting)]
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
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_HunchbackB_RangeOnly)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_HunchbackB_Dash ON)]
            │       │   └── ⚔️ UseSkill(DashAttack)
            │       ├── ⚔️ UseSkill(Grab) [🛡️UseableTime]
            │       ├── ⚔️ UseSkill(Swing)
            │       ├── ⚔️ UseSkill(SwingCombo)
            │       ├── ⚔️ UseSkill(Spit)
            │       └── 🚶 MoveToTarget
            ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_HunchbackB_RangeOnly ON) && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚔️ UseSkill(SpitFar)
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_HunchbackB_RangeOnly)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_HunchbackB_Dash ON)]
                │   └── ⚔️ UseSkill(DashAttack)
                ├── ⚔️ UseSkill(Grab) [🛡️UseableTime]
                ├── ⚔️ UseSkill(Swing)
                ├── ⚔️ UseSkill(SwingCombo)
                ├── ⚔️ UseSkill(Spit)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 20 |
| Selector | 12 |
| Task/UseSkill | 12 |
| Sequence | 10 |
| Dec/AggroLevel | 8 |
| Dec/Blackboard | 5 |
| Task/Blackboard | 5 |
| Dec/IsAlive | 3 |
| Task/CautionToTarget | 3 |
| Task/MoveToTarget | 3 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Dec/CheckStance | 2 |
| Dec/TimeLimit | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/UseableTimeReset | 2 |
| Dec/UseableTime | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(StandbyToNormal combo=TableCommand)
- UseSkill(DashAttack)
- UseSkill(Grab)
- UseSkill(Swing)
- UseSkill(SwingCombo)
- UseSkill(Spit)
- UseSkill(SpitFar)
- UseSkill(DashAttack)
- UseSkill(Grab)
- UseSkill(Swing)
- UseSkill(SwingCombo)
- UseSkill(Spit)
