# M_ClriketCGrab_AI_TR

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self) && 🛡️CheckActorEffect(NOT Self.TR_SkillStop ON)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(Self.TR_SkillAll ON)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ⚔️ UseSkill(Grab_1) [🛡️Random(rand(100)<=30)]
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1 combo=TableSkillFlag)
            │       │   └── ⏳ WaitTimeRandom
            │       └── 🚶 MoveToTarget
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillAll ON)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │       └── ➡️ Sequence
            │           ├── ⚔️ UseSkill(Grab_1)
            │           ├── ✨ UseEffect(['TR_SkillCoolTimeReset_ClriketCGrab'])
            │           └── ⏳ Wait(3.0s)
            └── ❓ Selector [🛡️CheckActorEffect(Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillAll ON)]
                └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ⚔️ UseSkill(SwingLeft_1|SwingRight_1)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 19 |
| Selector | 12 |
| Dec/AggroLevel | 6 |
| Sequence | 5 |
| Task/UseSkill | 4 |
| Dec/IsAlive | 2 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Task/CautionToTarget | 2 |
| Dec/TimeLimit | 2 |
| Task/WaitTimeRandom | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Dec/CheckActorState | 1 |
| Dec/Random | 1 |
| Task/UseEffect | 1 |
| Task/Wait | 1 |

## 스킬 목록
- UseSkill(Grab_1)
- UseSkill(SwingLeft_1|SwingRight_1 combo=TableSkillFlag)
- UseSkill(Grab_1)
- UseSkill(SwingLeft_1|SwingRight_1)
