# M_ClriketB_Ground_AI

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
            │   ├── ❓ Selector [🛡️CheckStance(M_ClriketB_GroundStance) && 🛡️CheckActorEffect(Self.M_Common_SurpriseAttackCheck ON)]
            │   │   ├── ⚔️ UseSkill(GroundToAttack2 combo=TableCommand) [🛡️CheckActorEffect(Self.M_ClriketB_GroundToBrokeWindow ON)]
            │   │   └── ⚔️ UseSkill(GroundToAttack combo=TableCommand)
            │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_ClriketB_GroundStance) && 🛡️CheckActorEffect(Self.M_Common_SurpriseAttackCheck)]
            │   │       └── ⚔️ UseSkill(GroundToNormal_1 combo=TableCommand)
            │   └── ❓ Selector [🛡️Blackboard(BattleStart)]
            │       └── ➡️ Sequence [🛡️CheckStance(M_ClriketB_DefaultStance)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckStance(NOT M_ClriketB_GroundStance)]
                ├── ❓ Selector [🛡️IsGroupTarget]
                │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
                │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
                │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                │       ├── ➡️ Sequence [🛡️Blackboard(SurpriseAttack) && 🛡️CheckActorEffect(Self.M_Common_SurpriseAttackCheck ON)]
                │       │   ├── ⚔️ UseSkill(SurpriseAttack_1)
                │       │   └── 📋 Blackboard(SurpriseAttack)
                │       ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_SurpriseAttackCheck)]
                │       │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │       │   │   ├── 📋 Blackboard(BattleStartSkill)
                │       │   │   └── ❓ Selector
                │       │   │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                │       │   │       │   ├── 🚶 MoveToTarget
                │       │   │       │   └── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
                │       │   │       └── ➡️ Sequence
                │       │   │           ├── 🚶 MoveToTarget
                │       │   │           └── ⚔️ UseSkill(TraceAttackRight_2)
                │       │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckStance(M_ClriketB_DefaultStance)]
                │       │   │   └── ⚔️ UseSkill(StanceChange1_1|StanceChange2_1)
                │       │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckStance(M_ClriketB_FuryStance)]
                │       │   │   └── ⚔️ UseSkill(ComboAttack_1)
                │       │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=150.0)]
                │       │   │   ├── ⚔️ UseSkill(TraceAttackRight_2|TraceAttackRight_3)
                │       │   │   └── ⏳ Wait(0.5s)
                │       │   └── ➡️ Sequence
                │       │       ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
                │       │       └── ⏳ Wait(0.5s)
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence [🛡️Blackboard(SurpriseAttack) && 🛡️CheckActorEffect(Self.M_Common_SurpriseAttackCheck ON)]
                    │   ├── ⚔️ UseSkill(SurpriseAttack_1)
                    │   └── 📋 Blackboard(SurpriseAttack)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_SurpriseAttackCheck)]
                    │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                    │   │   ├── 📋 Blackboard(BattleStartSkill)
                    │   │   └── ❓ Selector
                    │   │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                    │   │       │   ├── 🚶 MoveToTarget
                    │   │       │   └── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
                    │   │       └── ➡️ Sequence
                    │   │           ├── 🚶 MoveToTarget
                    │   │           └── ⚔️ UseSkill(TraceAttackRight_2)
                    │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckStance(M_ClriketB_DefaultStance)]
                    │   │   └── ⚔️ UseSkill(StanceChange1_1|StanceChange2_1)
                    │   ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️CheckStance(M_ClriketB_FuryStance)]
                    │   │   └── ⚔️ UseSkill(ComboAttack_1)
                    │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=150.0)]
                    │   │   ├── ⚔️ UseSkill(TraceAttackRight_2|TraceAttackRight_3)
                    │   │   └── ⏳ Wait(0.5s)
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
                    │       └── ⏳ Wait(0.5s)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 23 |
| Dec/CheckActorEffect | 19 |
| Sequence | 17 |
| Task/UseSkill | 17 |
| Dec/CheckStance | 8 |
| Dec/AggroLevel | 7 |
| Dec/Blackboard | 6 |
| Task/Blackboard | 6 |
| Task/Wait | 6 |
| Task/MoveToTarget | 6 |
| Dec/TimeLimit | 4 |
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
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(GroundToAttack2 combo=TableCommand)
- UseSkill(GroundToAttack combo=TableCommand)
- UseSkill(GroundToNormal_1 combo=TableCommand)
- UseSkill(SurpriseAttack_1)
- UseSkill(SwingLeft_1|SwingRight_1)
- UseSkill(TraceAttackRight_2)
- UseSkill(StanceChange1_1|StanceChange2_1)
- UseSkill(ComboAttack_1)
- UseSkill(TraceAttackRight_2|TraceAttackRight_3)
- UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
- UseSkill(SurpriseAttack_1)
- UseSkill(SwingLeft_1|SwingRight_1)
- UseSkill(TraceAttackRight_2)
- UseSkill(StanceChange1_1|StanceChange2_1)
- UseSkill(ComboAttack_1)
- UseSkill(TraceAttackRight_2|TraceAttackRight_3)
- UseSkill(SwingLeft_1|SwingRight_1|SmashAttack_1)
