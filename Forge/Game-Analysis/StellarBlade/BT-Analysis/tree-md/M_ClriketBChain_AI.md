# M_ClriketBChain_AI

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
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence [🛡️CheckStance(M_ClriketB_ChainStance)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s) && 🛡️CheckActorEffect(Target.Check_LinkSkill ON)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=30)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   ├── ⚔️ UseSkill(StrongChain_1)
            │       │       │   ├── ⚔️ UseSkill(SmashRun_1)
            │       │       │   └── ⏳ Wait(0.5s)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
            │       ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
            │       │   ├── ⚔️ UseSkill(ComboAttack_1)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(StrongChain_1)
            │       │   │   ├── ⚔️ UseSkill(SmashRun_1)
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(SwingForward_1)
            │       │       ├── ⚔️ UseSkill(SmashRun_1)
            │       │       └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=150.0) && 🛡️TimeLimit(3.0s)]
            │           ├── ➡️ Sequence
            │           │   ├── ⚔️ UseSkill(TraceAttackRight_1)
            │           │   └── ⏳ WaitTimeRandom
            │           ├── ➡️ Sequence
            │           │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1|)
            │           │   └── ⏳ WaitTimeRandom
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=30)]
                │       │   ├── 🚶 MoveToTarget
                │       │   ├── ⚔️ UseSkill(StrongChain_1)
                │       │   ├── ⚔️ UseSkill(SmashRun_1)
                │       │   └── ⏳ Wait(0.5s)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
                ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
                │   ├── ⚔️ UseSkill(ComboAttack_1)
                │   └── ⏳ WaitTimeRandom
                ├── ❓ Selector
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(StrongChain_1)
                │   │   ├── ⚔️ UseSkill(SmashRun_1)
                │   │   └── ⏳ Wait(0.5s)
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(SwingForward_1)
                │       ├── ⚔️ UseSkill(SmashRun_1)
                │       └── ⏳ Wait(0.5s)
                └── ❓ Selector
                    ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=150.0) && 🛡️TimeLimit(3.0s)]
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(TraceAttackRight_1)
                    │   └── ⏳ Wait(0.1s)
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1|)
                    │   └── ⏳ Wait(0.1s)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 20 |
| Task/UseSkill | 20 |
| Selector | 18 |
| Dec/CheckActorEffect | 12 |
| Task/Wait | 8 |
| Dec/AggroLevel | 7 |
| Task/WaitTimeRandom | 7 |
| Dec/TimeLimit | 6 |
| Task/MoveToTarget | 6 |
| Task/CautionToTarget | 5 |
| Dec/Blackboard | 4 |
| Task/Blackboard | 4 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/Random | 2 |
| Dec/CheckActorStat | 2 |
| Dec/DistanceToTarget | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckStance | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(StrongChain_1)
- UseSkill(SmashRun_1)
- UseSkill(SwingLeft_1|SwingRight_1)
- UseSkill(ComboAttack_1)
- UseSkill(StrongChain_1)
- UseSkill(SmashRun_1)
- UseSkill(SwingForward_1)
- UseSkill(SmashRun_1)
- UseSkill(TraceAttackRight_1)
- UseSkill(SwingLeft_1|SwingRight_1|)
- UseSkill(StrongChain_1)
- UseSkill(SmashRun_1)
- UseSkill(SwingLeft_1|SwingRight_1)
- UseSkill(ComboAttack_1)
- UseSkill(StrongChain_1)
- UseSkill(SmashRun_1)
- UseSkill(SwingForward_1)
- UseSkill(SmashRun_1)
- UseSkill(TraceAttackRight_1)
- UseSkill(SwingLeft_1|SwingRight_1|)
