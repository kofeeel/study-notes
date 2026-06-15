# N_TachyNPC_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        └── ❓ Selector
            ├── 👁️ DetectTarget [🛡️DetectResult(==)]
            └── ❓ Selector [🛡️IsAlive(Target)]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkSkillHit_Skill)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB1)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(SlashChain combo=TableCommand) [🛡️DistanceToTarget(dist<=250.0) && 🛡️Random(rand(100)<=70) && 🛡️UseableTime]
                │   │   └── ⏳ WaitTimeRandom
                │   ├── ❓ Selector
                │   │   ├── ➡️ Sequence [🛡️Random(rand(100)<=55)]
                │   │   │   ├── ⚔️ UseSkill(LightAttack01 combo=TableCommand) [🛡️DistanceToTarget(dist<=250.0)]
                │   │   │   ├── ⚔️ UseSkill(LightAttack02 combo=TableCommand) [🛡️DistanceToTarget(dist<=250.0)]
                │   │   │   ├── ⚔️ UseSkill(LightAttack03 combo=TableCommand) [🛡️DistanceToTarget(dist<=250.0)]
                │   │   │   ├── ⚔️ UseSkill(LightAttack04 combo=TableCommand) [🛡️DistanceToTarget(dist<=250.0) && 🛡️Random(rand(100)<=70)]
                │   │   │   └── ⏳ WaitTimeRandom
                │   │   └── ➡️ Sequence
                │   │       ├── ⚔️ UseSkill(StrongAttack01 combo=TableCommand) [🛡️DistanceToTarget(dist<=250.0)]
                │   │       ├── ⚔️ UseSkill(StrongAttack02 combo=TableCommand) [🛡️DistanceToTarget(dist<=250.0)]
                │   │       ├── ⚔️ UseSkill(StrongAttack03 combo=TableCommand) [🛡️DistanceToTarget(dist<=250.0) && 🛡️Random(rand(100)<=70)]
                │   │       └── ⏳ WaitTimeRandom
                │   └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkSkillHit_Skill ON)]
                    └── ⏳ Wait(4.73s)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 8 |
| Dec/DistanceToTarget | 8 |
| Selector | 7 |
| Sequence | 4 |
| Dec/Random | 4 |
| Task/WaitTimeRandom | 3 |
| Dec/IsAlive | 2 |
| Dec/CheckActorEffect | 2 |
| Task/UseableTimeReset | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/Blackboard | 1 |
| Task/Blackboard | 1 |
| Dec/UseableTime | 1 |
| Task/MoveToTarget | 1 |
| Task/Wait | 1 |

## 스킬 목록
- UseSkill(SlashChain combo=TableCommand)
- UseSkill(LightAttack01 combo=TableCommand)
- UseSkill(LightAttack02 combo=TableCommand)
- UseSkill(LightAttack03 combo=TableCommand)
- UseSkill(LightAttack04 combo=TableCommand)
- UseSkill(StrongAttack01 combo=TableCommand)
- UseSkill(StrongAttack02 combo=TableCommand)
- UseSkill(StrongAttack03 combo=TableCommand)
