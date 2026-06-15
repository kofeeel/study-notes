# M_StatueB_AI_TR

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self) && 🛡️CheckActorEffect(NOT Self.TR_SkillStop ON)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence [🛡️CheckStance(M_StatueB_Default)]
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.46s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.46s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(Self.TR_SkillAll ON)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_NoGuard)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB_NoGuard)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=35)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(SlashDouble)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(Slash)
            │       ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=200.0) && 🛡️UseableTime]
            │       │   ├── ⚔️ UseSkill(RushSlash2_KnockDown)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(Slash|SlashDouble|SlashTriple)
            │       │   └── ⏳ WaitTimeRandom
            │       └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=200.0)]
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillAll ON)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │       └── ➡️ Sequence
            │           ├── ⚔️ UseSkill(RushSlash2_KnockDown)
            │           ├── ✨ UseEffect(['TR_SkillCoolTimeReset_StatueB'])
            │           └── ⏳ Wait(3.0s)
            └── ❓ Selector [🛡️CheckActorEffect(Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillAll ON)]
                └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ⚔️ UseSkill(Slash|SlashDouble|SlashTriple)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 19 |
| Selector | 14 |
| Sequence | 10 |
| Dec/AggroLevel | 6 |
| Task/UseSkill | 6 |
| Dec/Blackboard | 4 |
| Task/Blackboard | 4 |
| Task/MoveToTarget | 4 |
| Task/WaitTimeRandom | 3 |
| Dec/IsAlive | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Task/CautionToTarget | 2 |
| Dec/TimeLimit | 2 |
| Task/UseableTimeReset | 2 |
| Dec/DistanceToTarget | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Dec/CheckStance | 1 |
| Dec/CheckActorState | 1 |
| Dec/Random | 1 |
| Dec/UseableTime | 1 |
| Task/UseEffect | 1 |
| Task/Wait | 1 |

## 스킬 목록
- UseSkill(SlashDouble)
- UseSkill(Slash)
- UseSkill(RushSlash2_KnockDown)
- UseSkill(Slash|SlashDouble|SlashTriple)
- UseSkill(RushSlash2_KnockDown)
- UseSkill(Slash|SlashDouble|SlashTriple)
