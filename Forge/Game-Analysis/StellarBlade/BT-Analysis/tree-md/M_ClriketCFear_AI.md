# M_ClriketCFear_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   ├── 📋 Blackboard(BattleStart)
        │   └── 📋 Blackboard(BattleStartSkill)
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
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │   │   ├── 📋 Blackboard(BattleStartSkill)
            │   │   └── ⏳ WaitTimeRandom
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   │   ├── ⚔️ UseSkill(MoveForward_1)
            │       │   │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │       │   │   └── ⏳ Wait(0.5s)
            │       │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   │   ├── ⚔️ UseSkill(MoveForward_2)
            │       │   │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │       │   │   └── ⏳ Wait(0.5s)
            │       │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   │   ├── ⚔️ UseSkill(SwoopForward_1)
            │       │   │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │       │   │   └── ⏳ Wait(0.5s)
            │       │   └── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │       └── 📋 Blackboard(BattleStartSkill)
            │       └── ❓ Selector [🛡️Blackboard(BattleStartSkill)]
            │           ├── ➡️ Sequence [🛡️Blackboard(BB2)]
            │           │   ├── 🔄 UseableTimeReset
            │           │   └── 📋 Blackboard(BB2)
            │           ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
            │           │   ├── ⚔️ UseSkill(ComboAttack_1 combo=TableCommand)
            │           │   └── ⏳ WaitTimeRandom
            │           ├── ❓ Selector [🛡️UseableTime]
            │           │   ├── ➡️ Sequence
            │           │   │   ├── ⚔️ UseSkill(MoveForward_2)
            │           │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │           │   │   └── ⏳ Wait(0.5s)
            │           │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(NOT Target.M_ClriketCFear_Swoop ON)]
            │           │   │   ├── ⚔️ UseSkill(SwoopForward_2)
            │           │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │           │   │   └── ⏳ Wait(0.5s)
            │           │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.M_ClriketCFear_Swoop ON)]
            │           │   │   ├── ⚔️ UseSkill(SwoopForward_1)
            │           │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │           │   │   └── ⏳ Wait(0.5s)
            │           │   └── ➡️ Sequence
            │           │       ├── ⚔️ UseSkill(MoveForward_1)
            │           │       ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │           │       └── ⏳ Wait(0.5s)
            │           └── ❓ Selector
            │               ├── ➡️ Sequence
            │               │   ├── ⚔️ UseSkill(WildSwingChain_1)
            │               │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
            │               │   └── ⏳ Wait(0.5s)
            │               ├── ➡️ Sequence
            │               │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
            │               │   └── ⏳ WaitTimeRandom
            │               ├── 🎬 PlayShow [🛡️TimeLimit(2.0s) && 🛡️DistanceToTarget(dist>=100.0)]
            │               └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   │   ├── ⚔️ UseSkill(MoveForward_1)
                │   │   ├── 📋 Blackboard(BattleStartSkill)
                │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                │   │   └── ⏳ Wait(0.5s)
                │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   │   ├── ⚔️ UseSkill(MoveForward_2)
                │   │   ├── 📋 Blackboard(BattleStartSkill)
                │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                │   │   └── ⏳ Wait(0.5s)
                │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   │   ├── ⚔️ UseSkill(SwoopForward_1)
                │   │   ├── 📋 Blackboard(BattleStartSkill)
                │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                │   │   └── ⏳ Wait(0.5s)
                │   └── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │       └── 📋 Blackboard(BattleStartSkill)
                └── ❓ Selector [🛡️Blackboard(BattleStartSkill)]
                    ├── ➡️ Sequence [🛡️Blackboard(BB2)]
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(BB2)
                    ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
                    │   ├── ⚔️ UseSkill(ComboAttack_1 combo=TableCommand)
                    │   └── ⏳ WaitTimeRandom
                    ├── ❓ Selector [🛡️UseableTime]
                    │   ├── ➡️ Sequence
                    │   │   ├── ⚔️ UseSkill(MoveForward_2)
                    │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                    │   │   └── ⏳ Wait(0.5s)
                    │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(NOT Target.M_ClriketCFear_Swoop ON)]
                    │   │   ├── ⚔️ UseSkill(SwoopForward_2)
                    │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                    │   │   └── ⏳ Wait(0.5s)
                    │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.M_ClriketCFear_Swoop ON)]
                    │   │   ├── ⚔️ UseSkill(SwoopForward_1)
                    │   │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                    │   │   └── ⏳ Wait(0.5s)
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(MoveForward_1)
                    │       ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                    │       └── ⏳ Wait(0.5s)
                    └── ❓ Selector
                        ├── ➡️ Sequence
                        │   ├── ⚔️ UseSkill(WildSwingChain_1)
                        │   ├── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                        │   └── ⏳ Wait(0.5s)
                        ├── ➡️ Sequence
                        │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
                        │   └── ⏳ WaitTimeRandom
                        ├── 🎬 PlayShow [🛡️TimeLimit(2.0s) && 🛡️DistanceToTarget(dist>=100.0)]
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 29 |
| Task/PlayShow | 20 |
| Task/UseSkill | 20 |
| Selector | 19 |
| Task/Wait | 18 |
| Dec/LastSkillHitResult | 16 |
| Dec/Blackboard | 15 |
| Dec/CheckActorEffect | 15 |
| Task/Blackboard | 14 |
| Dec/AggroLevel | 7 |
| Task/WaitTimeRandom | 6 |
| Dec/DistanceToTarget | 4 |
| Dec/IsAlive | 3 |
| Dec/TimeLimit | 3 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 2 |
| Task/CautionToTarget | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/UseableTimeReset | 2 |
| Dec/CheckActorStat | 2 |
| Dec/UseableTime | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(MoveForward_1)
- UseSkill(MoveForward_2)
- UseSkill(SwoopForward_1)
- UseSkill(ComboAttack_1 combo=TableCommand)
- UseSkill(MoveForward_2)
- UseSkill(SwoopForward_2)
- UseSkill(SwoopForward_1)
- UseSkill(MoveForward_1)
- UseSkill(WildSwingChain_1)
- UseSkill(SwingLeft_1|SwingRight_1)
- UseSkill(MoveForward_1)
- UseSkill(MoveForward_2)
- UseSkill(SwoopForward_1)
- UseSkill(ComboAttack_1 combo=TableCommand)
- UseSkill(MoveForward_2)
- UseSkill(SwoopForward_2)
- UseSkill(SwoopForward_1)
- UseSkill(MoveForward_1)
- UseSkill(WildSwingChain_1)
- UseSkill(SwingLeft_1|SwingRight_1)
