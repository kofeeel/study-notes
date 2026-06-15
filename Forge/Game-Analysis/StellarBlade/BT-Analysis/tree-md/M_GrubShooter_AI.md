# M_GrubShooter_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       ├── 🎬 PlayShow
            │       └── ⏳ Wait(1.2s)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ➡️ Sequence [🛡️CheckActorEffect(Target.? ON)]
            │   ├── 🎬 PlayShow
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── 🎬 PlayShow [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(2.0s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ⚔️ UseSkill(VomitAttack_1)
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(SpitAttack_1)
            │       │   ├── ⚔️ UseSkill(JumpBack_1) [🛡️DistanceToTarget(dist<=300.0)]
            │       │   └── ⚠️ CautionToTarget [🛡️Random(rand(100)<=70) && 🛡️TimeLimit(?s)]
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(SpitTrainAttack_1)
            │       │   ├── ⚔️ UseSkill(JumpBack_1) [🛡️DistanceToTarget(dist<=300.0)]
            │       │   └── ⚠️ CautionToTarget [🛡️Random(rand(100)<=70) && 🛡️TimeLimit(?s)]
            │       ├── ⚔️ UseSkill(JumpStand_1)
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ⚔️ UseSkill(VomitAttack_1)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SpitAttack_1)
                │   ├── ⚔️ UseSkill(JumpBack_1) [🛡️DistanceToTarget(dist<=300.0)]
                │   └── ⚠️ CautionToTarget [🛡️Random(rand(100)<=70) && 🛡️TimeLimit(?s)]
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SpitTrainAttack_1)
                │   ├── ⚔️ UseSkill(JumpBack_1) [🛡️DistanceToTarget(dist<=300.0)]
                │   └── ⚠️ CautionToTarget [🛡️Random(rand(100)<=70) && 🛡️TimeLimit(?s)]
                ├── ⚔️ UseSkill(JumpStand_1)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 12 |
| Task/UseSkill | 12 |
| Selector | 9 |
| Sequence | 9 |
| Dec/AggroLevel | 7 |
| Dec/TimeLimit | 5 |
| Task/CautionToTarget | 5 |
| Task/PlayShow | 4 |
| Dec/DistanceToTarget | 4 |
| Dec/Random | 4 |
| Dec/IsAlive | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |

## 스킬 목록
- UseSkill(VomitAttack_1)
- UseSkill(SpitAttack_1)
- UseSkill(JumpBack_1)
- UseSkill(SpitTrainAttack_1)
- UseSkill(JumpBack_1)
- UseSkill(JumpStand_1)
- UseSkill(VomitAttack_1)
- UseSkill(SpitAttack_1)
- UseSkill(JumpBack_1)
- UseSkill(SpitTrainAttack_1)
- UseSkill(JumpBack_1)
- UseSkill(JumpStand_1)
