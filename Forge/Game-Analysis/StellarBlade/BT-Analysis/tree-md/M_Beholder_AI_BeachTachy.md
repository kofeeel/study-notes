# M_Beholder_AI_BeachTachy

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        └── ❓ Selector
            ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
            │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
            │   └── 📋 Blackboard(BattleStart)
            ├── ❓ Selector
            │   └── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackTachyNPC ON)]
            │       └── 👁️ DetectTarget [🛡️DetectResult(==)]
            ├── 👁️ DetectTarget [🛡️CheckActorEffect(Self.Check_AttackTachyNPC) && 🛡️DetectResult(==)]
            └── ❓ Selector [🛡️IsAlive(Target)]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [AbnormalTimer])]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
                └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence [🛡️Blackboard(BattleStart)]
                    │   ├── 📋 Blackboard(BattleStart)
                    │   └── ⏳ Wait(0.5s)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackPC)]
                    │   ├── ⚔️ UseSkill(SwingDoubleTachyTarget)
                    │   └── ⚔️ UseSkill(SwingTachyTarget|SpinSwingTachyTarget)
                    └── ❓ Selector
                        ├── 🚶 MoveToTarget [🛡️CheckActorEffect(Target.Check_TachyNPC ON)]
                        └── 👁️ DetectTarget [🛡️CheckActorEffect(NOT Target.Check_TachyNPC ON)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 11 |
| Dec/CheckActorEffect | 10 |
| Task/DetectTarget | 3 |
| Dec/IsAlive | 2 |
| Sequence | 2 |
| Dec/AggroLevel | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Dec/DetectResult | 2 |
| Task/CautionToTarget | 2 |
| Dec/TimeLimit | 2 |
| Task/UseSkill | 2 |
| Dec/IsActiveSkill | 1 |
| Task/PlayShow | 1 |
| Task/WaitTimeRandom | 1 |
| Task/Wait | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(SwingDoubleTachyTarget)
- UseSkill(SwingTachyTarget|SpinSwingTachyTarget)
