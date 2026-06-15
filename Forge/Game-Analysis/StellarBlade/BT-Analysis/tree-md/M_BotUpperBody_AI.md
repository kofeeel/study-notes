# M_BotUpperBody_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction)]
        │   ├── 🏠 MoveToHome [🛡️DetectResult(==)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── ⏳ WaitTimeRandom
            ├── ➡️ Sequence [🛡️Blackboard(GrabSudden) && 🛡️CheckActorEffect(Self.M_BotUpperBody_GrabSudden ON)]
            │   ├── ⚔️ UseSkill(GrabSudden)
            │   └── 📋 Blackboard(GrabSudden)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(Self.M_BotUpperBody_GrabSudden)]
            │   ├── ❓ Selector
            │   │   ├── ➡️ Sequence [🛡️Blackboard(Grab_Timer)]
            │   │   │   ├── 🔄 UseableTimeReset
            │   │   │   └── 📋 Blackboard(Grab_Timer)
            │   │   └── ➡️ Sequence [🛡️UseableTime]
            │   │       ├── ⚔️ UseSkill(GrabBomb combo=TableCommand) [🛡️CheckActorEffect(NOT Target.M_BotUpperBody_CheckGrab ON)]
            │   │       └── ⏳ WaitTimeRandom
            │   ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │   │   ├── 📋 Blackboard(BattleStartSkill)
            │   │   └── ❓ Selector
            │   │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
            │   │       │   ├── 🚶 MoveToTarget
            │   │       │   ├── ⏳ WaitTimeRandom
            │   │       │   └── ⚔️ UseSkill(Swing)
            │   │       └── ➡️ Sequence
            │   │           ├── 🚶 MoveToTarget
            │   │           ├── ⏳ WaitTimeRandom
            │   │           └── ⚔️ UseSkill(StandAttack)
            │   ├── ➡️ Sequence
            │   │   ├── ⏳ WaitTimeRandom
            │   │   ├── ⚔️ UseSkill(StandAttack)
            │   │   └── ⏳ WaitTimeRandom
            │   ├── ➡️ Sequence
            │   │   ├── ⏳ WaitTimeRandom
            │   │   ├── ⚔️ UseSkill(Swing)
            │   │   └── ⏳ WaitTimeRandom
            │   └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON) && 🛡️IsGroupTarget(NOT)]
                └── ⏳ WaitTimeRandom
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 11 |
| Selector | 10 |
| Sequence | 10 |
| Task/WaitTimeRandom | 9 |
| Task/UseSkill | 6 |
| Task/Blackboard | 4 |
| Dec/IsAlive | 3 |
| Dec/AggroLevel | 3 |
| Dec/Blackboard | 3 |
| Task/MoveToTarget | 3 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/TimeLimit | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Task/CautionToTarget | 1 |
| Task/UseableTimeReset | 1 |
| Dec/UseableTime | 1 |
| Dec/Random | 1 |
| Dec/IsGroupTarget | 1 |

## 스킬 목록
- UseSkill(GrabSudden)
- UseSkill(GrabBomb combo=TableCommand)
- UseSkill(Swing)
- UseSkill(StandAttack)
- UseSkill(StandAttack)
- UseSkill(Swing)
