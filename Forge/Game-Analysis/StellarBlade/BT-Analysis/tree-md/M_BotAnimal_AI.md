# M_BotAnimal_AI

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
            │   └── 🎬 PlayShow [🛡️TimeLimit(2.93s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── 🎬 PlayShow [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(2.93s [Timer_LinkWait])]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── 🎬 PlayShow [🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️TimeLimit(2.93s)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(10.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(LeftMoveSlash|RightMoveSlash)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(SlashLeft|SlashRight|TripleSlashDash)
            │       ├── ➡️ Sequence [🛡️UseableTime]
            │       │   ├── ⚔️ UseSkill(LeftMoveSlash|RightMoveSlash)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(SlashLeft|SlashRight)
            │       │   └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(10.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(LeftMoveSlash|RightMoveSlash)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(SlashLeft|SlashRight|TripleSlashDash)
                ├── ➡️ Sequence [🛡️UseableTime]
                │   ├── ⏳ WaitTimeRandom
                │   ├── ⚔️ UseSkill(Stinger)
                │   └── ⏳ WaitTimeRandom
                ├── ⚔️ UseSkill(EvasionAttack) [🛡️CheckActorEffect(Self.M_BotAnimal_HitResult ON)]
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(LeftMoveSlash|RightMoveSlash)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(TripleSlashDash) [🛡️Random(rand(100)<=60)]
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SlashLeft|SlashRight)
                │   └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 18 |
| Selector | 15 |
| Dec/CheckActorEffect | 13 |
| Task/UseSkill | 11 |
| Dec/AggroLevel | 7 |
| Task/WaitTimeRandom | 7 |
| Dec/Blackboard | 6 |
| Task/Blackboard | 6 |
| Task/MoveToTarget | 6 |
| Task/PlayShow | 5 |
| Dec/TimeLimit | 5 |
| Dec/IsAlive | 3 |
| Task/UseableTimeReset | 3 |
| Dec/Random | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/UseableTime | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(LeftMoveSlash|RightMoveSlash)
- UseSkill(SlashLeft|SlashRight|TripleSlashDash)
- UseSkill(LeftMoveSlash|RightMoveSlash)
- UseSkill(SlashLeft|SlashRight)
- UseSkill(LeftMoveSlash|RightMoveSlash)
- UseSkill(SlashLeft|SlashRight|TripleSlashDash)
- UseSkill(Stinger)
- UseSkill(EvasionAttack)
- UseSkill(LeftMoveSlash|RightMoveSlash)
- UseSkill(TripleSlashDash)
- UseSkill(SlashLeft|SlashRight)
