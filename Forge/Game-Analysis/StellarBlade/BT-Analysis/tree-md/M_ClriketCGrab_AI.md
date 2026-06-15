# M_ClriketCGrab_AI

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
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   ├── 🚶 MoveToTarget
            │       │   └── ⚔️ UseSkill(ChopChain_1)
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence
            │       │   │   ├── ⚔️ UseSkill(Grab_1) [🛡️CheckActorEffect(NOT Target.M_ClriketCGrab_Grab ON) && 🛡️LastSkillHitResult(NOT SkillHitResult_Parry) && 🛡️Random(rand(100)<=80)]
            │       │   │   └── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(ChopChain_1)
            │       │       ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │       │       └── ⏳ Wait(0.5s)
            │       ├── ❓ Selector
            │       │   ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=250.0) && 🛡️DistanceToTarget(dist<700.0) && 🛡️TimeLimit(3.0s)]
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(MeleeAttack combo=TableSkillFlag)
            │       │       └── ⏳ WaitTimeRandom
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   ├── 🚶 MoveToTarget
                │   └── ⚔️ UseSkill(ChopChain_1)
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.M_ClriketCGrab_Grab ON) && 🛡️LastSkillHitResult(NOT SkillHitResult_Parry) && 🛡️Random(rand(100)<=80)]
                │   │   ├── ⚔️ UseSkill(Grab_1)
                │   │   └── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                │   └── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.M_ClriketCGrab_Grab ON)]
                │       ├── ⚔️ UseSkill(ChopChain_1)
                │       ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                │       └── ⏳ Wait(0.5s)
                ├── ❓ Selector
                │   ├── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=250.0) && 🛡️DistanceToTarget(dist<700.0) && 🛡️TimeLimit(3.0s)]
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(MeleeAttack combo=TableSkillFlag)
                │       └── ⏳ WaitTimeRandom
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 15 |
| Dec/CheckActorEffect | 14 |
| Sequence | 12 |
| Task/UseSkill | 8 |
| Dec/AggroLevel | 7 |
| Task/PlayShow | 6 |
| Dec/TimeLimit | 6 |
| Dec/LastSkillHitResult | 6 |
| Task/CautionToTarget | 5 |
| Dec/Blackboard | 4 |
| Task/Blackboard | 4 |
| Task/Wait | 4 |
| Task/MoveToTarget | 4 |
| Dec/DistanceToTarget | 4 |
| Dec/IsAlive | 3 |
| Task/WaitTimeRandom | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/Random | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(ChopChain_1)
- UseSkill(Grab_1)
- UseSkill(ChopChain_1)
- UseSkill(MeleeAttack combo=TableSkillFlag)
- UseSkill(ChopChain_1)
- UseSkill(Grab_1)
- UseSkill(ChopChain_1)
- UseSkill(MeleeAttack combo=TableSkillFlag)
