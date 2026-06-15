# M_BarnacleA_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │       └── 📋 Blackboard(BattleState)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ➡️ Sequence [🛡️CheckActorEffect(Target.? ON)]
            │   ├── ⚔️ UseSkill(MoveBack_1)
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.2s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.2s [Timer_LinkWait])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=20) && 🛡️CheckActorEffect(NOT Self.BarnacleA_LinkSkillOff ON)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(CatchAttack_1) [🛡️CheckActorEffect(Self.Check_ATLMonster)]
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=35)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(BoxingAttack_1)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(TraceAttack_1|PunchAttack_1)
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
            │       │   │   ├── ⚔️ UseSkill(ComboAttack_1 combo=TableCommand)
            │       │   │   └── ⏳ WaitTimeRandom
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(MoveBack_1) [🛡️Random(rand(100)<=30)]
            │       │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s)]
            │       └── ❓ Selector
            │           ├── ➡️ Sequence
            │           │   ├── ⚔️ UseSkill(PunchAttack_1)
            │           │   └── ⏳ WaitTimeRandom
            │           ├── ➡️ Sequence
            │           │   ├── ⚔️ UseSkill(TraceAttack_1)
            │           │   └── ⏳ WaitTimeRandom
            │           ├── ➡️ Sequence
            │           │   ├── ⚔️ UseSkill(BoxingAttack_1)
            │           │   └── ⏳ WaitTimeRandom
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=20) && 🛡️CheckActorEffect(NOT Self.BarnacleA_LinkSkillOff ON)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(CatchAttack_1) [🛡️CheckActorEffect(Self.Check_ATLMonster)]
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=35)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(BoxingAttack_1)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(TraceAttack_1|PunchAttack_1)
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.BarnacleA_LinkSkillOff ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(CatchAttack_Timer)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(CatchAttack_Timer)
                │   └── ➡️ Sequence [🛡️UseableTime]
                │       ├── ⚔️ UseSkill(CatchAttack_1|GrabAttack_1 combo=TableCommand) [🛡️CheckActorEffect(Self.Check_ATLMonster)]
                │       ├── 📋 Blackboard(CatchAttack_Timer)
                │       └── 🎬 PlayShow [🛡️LastSkillHitResult(SkillHitResult_Hit)]
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=70.0%)]
                │   │   ├── ⚔️ UseSkill(ComboAttack_1 combo=TableCommand)
                │   │   └── ⏳ WaitTimeRandom
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(MoveBack_1) [🛡️Random(rand(100)<=30)]
                │       └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s) && 🛡️DistanceToTarget(dist>300.0)]
                └── ❓ Selector
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(PunchAttack_1)
                    │   └── ⏳ Wait(0.3s)
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(TraceAttack_1)
                    │   └── ⏳ Wait(0.3s)
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(BoxingAttack_1)
                    │   └── ⏳ Wait(0.3s)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Sequence | 26 |
| Task/UseSkill | 18 |
| Selector | 17 |
| Dec/CheckActorEffect | 17 |
| Task/MoveToTarget | 8 |
| Dec/AggroLevel | 7 |
| Task/Blackboard | 7 |
| Dec/Blackboard | 6 |
| Dec/TimeLimit | 6 |
| Task/WaitTimeRandom | 6 |
| Dec/Random | 6 |
| Task/Wait | 5 |
| Task/CautionToTarget | 5 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/CheckActorStat | 2 |
| Task/UseableTimeReset | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/UseableTime | 1 |
| Dec/LastSkillHitResult | 1 |
| Dec/DistanceToTarget | 1 |

## 스킬 목록
- UseSkill(MoveBack_1)
- UseSkill(CatchAttack_1)
- UseSkill(BoxingAttack_1)
- UseSkill(TraceAttack_1|PunchAttack_1)
- UseSkill(ComboAttack_1 combo=TableCommand)
- UseSkill(MoveBack_1)
- UseSkill(PunchAttack_1)
- UseSkill(TraceAttack_1)
- UseSkill(BoxingAttack_1)
- UseSkill(CatchAttack_1)
- UseSkill(BoxingAttack_1)
- UseSkill(TraceAttack_1|PunchAttack_1)
- UseSkill(CatchAttack_1|GrabAttack_1 combo=TableCommand)
- UseSkill(ComboAttack_1 combo=TableCommand)
- UseSkill(MoveBack_1)
- UseSkill(PunchAttack_1)
- UseSkill(TraceAttack_1)
- UseSkill(BoxingAttack_1)
