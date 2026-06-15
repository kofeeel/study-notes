# M_DroidOld_AI

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
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s [Timer_LinkWait])]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(DashSwing)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(SwingCombo|Swing)
            │       ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%)]
            │       │   ├── ⚔️ UseSkill(Guard) [🛡️Random(rand(100)<=80)]
            │       │   └── ⚔️ UseSkill(SwingChain|DashSlow|SwingCombo)
            │       ├── ⚔️ UseSkill(TurnL|TurnR|TurnB)
            │       ├── ❓ Selector [🛡️UseableTime]
            │       │   └── ⚔️ UseSkill(DashSlow)
            │       ├── ❓ Selector
            │       │   └── ⚔️ UseSkill(DashSwing)
            │       ├── ❓ Selector [🛡️UseableTime]
            │       │   └── ⚔️ UseSkill(SwingChain)
            │       ├── ⚔️ UseSkill(Swing|SwingCombo)
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(DashSwing)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(SwingCombo|Swing)
                ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%)]
                │   ├── ⚔️ UseSkill(Guard) [🛡️Random(rand(100)<=80)]
                │   └── ⚔️ UseSkill(SwingChain|DashSlow|SwingCombo)
                ├── ⚔️ UseSkill(TurnL|TurnR|TurnB)
                ├── ❓ Selector [🛡️UseableTime]
                │   └── ⚔️ UseSkill(DashSlow)
                ├── ❓ Selector
                │   └── ⚔️ UseSkill(DashSwing)
                ├── ❓ Selector [🛡️UseableTime]
                │   └── ⚔️ UseSkill(SwingChain)
                ├── ⚔️ UseSkill(Swing|SwingCombo)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 19 |
| Task/UseSkill | 18 |
| Sequence | 14 |
| Dec/CheckActorEffect | 13 |
| Dec/AggroLevel | 7 |
| Task/MoveToTarget | 7 |
| Dec/Blackboard | 6 |
| Task/Blackboard | 6 |
| Dec/TimeLimit | 4 |
| Task/UseableTimeReset | 4 |
| Dec/Random | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Task/CautionToTarget | 3 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/CheckActorStat | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(DashSwing)
- UseSkill(SwingCombo|Swing)
- UseSkill(Guard)
- UseSkill(SwingChain|DashSlow|SwingCombo)
- UseSkill(TurnL|TurnR|TurnB)
- UseSkill(DashSlow)
- UseSkill(DashSwing)
- UseSkill(SwingChain)
- UseSkill(Swing|SwingCombo)
- UseSkill(DashSwing)
- UseSkill(SwingCombo|Swing)
- UseSkill(Guard)
- UseSkill(SwingChain|DashSlow|SwingCombo)
- UseSkill(TurnL|TurnR|TurnB)
- UseSkill(DashSlow)
- UseSkill(DashSwing)
- UseSkill(SwingChain)
- UseSkill(Swing|SwingCombo)
