# M_ClriketBBlitz_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   ├── 📋 Blackboard(SurpriseAttack)
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ➡️ Sequence [🛡️Blackboard(BattleStart)]
            │   └── 📋 Blackboard(BattleStart)
            ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<100.0%)]
            │   └── 📋 Blackboard(SurpriseAttack)
            ├── ➡️ Sequence [🛡️Blackboard(SurpriseAttack)]
            │   ├── ⚔️ UseSkill(SurpriseAttack_1|SurpriseAttack_2)
            │   ├── 📋 Blackboard(SurpriseAttack)
            │   └── ⏳ Wait(0.5s)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️Blackboard(SurpriseAttack)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️CheckStance(M_ClriketB_FuryStance) && 🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
            │       │   ├── ⚔️ UseSkill(ComboAttack_1)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=150.0)]
            │       │   ├── ⚔️ UseSkill(TraceAttackRight_3)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
            │       │   └── ⏳ WaitTimeRandom
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️Blackboard(SurpriseAttack) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckStance(M_ClriketB_FuryStance)]
                │   └── ⚔️ UseSkill(ComboAttack_1)
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=150.0)]
                │   ├── ⚔️ UseSkill(TraceAttackRight_3)
                │   └── ⏳ Wait(0.5s)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
                │   └── ⏳ Wait(0.5s)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 12 |
| Dec/CheckActorEffect | 11 |
| Sequence | 10 |
| Task/UseSkill | 7 |
| Dec/AggroLevel | 5 |
| Dec/Blackboard | 5 |
| Task/Blackboard | 5 |
| Task/Wait | 5 |
| Task/WaitTimeRandom | 4 |
| Dec/IsAlive | 3 |
| Dec/CheckActorStat | 3 |
| Task/CautionToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Dec/TimeLimit | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/CheckStance | 2 |
| Dec/DistanceToTarget | 2 |
| Task/MoveToTarget | 2 |
| Task/PlayShow | 1 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |

## 스킬 목록
- UseSkill(SurpriseAttack_1|SurpriseAttack_2)
- UseSkill(ComboAttack_1)
- UseSkill(TraceAttackRight_3)
- UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
- UseSkill(ComboAttack_1)
- UseSkill(TraceAttackRight_3)
- UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
