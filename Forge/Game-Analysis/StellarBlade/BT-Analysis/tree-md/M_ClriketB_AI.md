# M_ClriketB_AI

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
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart)]
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_ClriketB_CeilingStance)]
            │   │       ├── ⚔️ UseSkill(CeilingAttack_1 combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence [🛡️CheckStance(M_ClriketB_DefaultStance)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(TraceAttackRight_2)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
            │       ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=90.0%) && 🛡️CheckStance(M_ClriketB_DefaultStance)]
            │       │   └── ⚔️ UseSkill(StanceChange1_1|StanceChange2_1)
            │       ├── ➡️ Sequence [🛡️CheckStance(M_ClriketB_FuryStance) && 🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
            │       │   ├── ⚔️ UseSkill(ComboAttack_1)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=150.0)]
            │       │   ├── ⚔️ UseSkill(TraceAttackRight_2|TraceAttackRight_3)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
            │       │   └── ⏳ WaitTimeRandom
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(TraceAttackRight_2)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
                ├── ❓ Selector [🛡️CheckStance(M_ClriketB_DefaultStance) && 🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
                │   └── ⚔️ UseSkill(StanceChange1_1|StanceChange2_1)
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckStance(M_ClriketB_FuryStance)]
                │   └── ⚔️ UseSkill(ComboAttack_1)
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=150.0)]
                │   ├── ⚔️ UseSkill(TraceAttackRight_2|SwingLeft_1)
                │   └── ⏳ Wait(0.5s)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
                │   └── ⏳ Wait(0.5s)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 18 |
| Sequence | 16 |
| Task/UseSkill | 13 |
| Dec/CheckActorEffect | 12 |
| Dec/AggroLevel | 7 |
| Dec/CheckStance | 6 |
| Task/MoveToTarget | 6 |
| Dec/Blackboard | 5 |
| Task/Blackboard | 5 |
| Task/Wait | 4 |
| Dec/TimeLimit | 4 |
| Task/WaitTimeRandom | 4 |
| Dec/CheckActorStat | 4 |
| Dec/IsAlive | 3 |
| Task/CautionToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/Random | 2 |
| Dec/DistanceToTarget | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(CeilingAttack_1 combo=TableCommand)
- UseSkill(TraceAttackRight_2)
- UseSkill(SwingLeft_1|SwingRight_1)
- UseSkill(StanceChange1_1|StanceChange2_1)
- UseSkill(ComboAttack_1)
- UseSkill(TraceAttackRight_2|TraceAttackRight_3)
- UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
- UseSkill(TraceAttackRight_2)
- UseSkill(SwingLeft_1|SwingRight_1)
- UseSkill(StanceChange1_1|StanceChange2_1)
- UseSkill(ComboAttack_1)
- UseSkill(TraceAttackRight_2|SwingLeft_1)
- UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
