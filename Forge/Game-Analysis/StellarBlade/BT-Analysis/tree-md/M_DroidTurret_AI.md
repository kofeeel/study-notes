# M_DroidTurret_AI

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
            │   ├── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent) && 🛡️CheckStance(M_DroidTurret_StanbyToNormal)]
            │   │   ├── ⚔️ UseSkill(StanbyToNormal combo=TableCommand)
            │   │   └── 📋 Blackboard(BattleStart)
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent) && 🛡️CheckStance(NOT M_DroidTurret_StanbyToNormal)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(Timer)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(Timer)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ⚔️ UseSkill(ShotDebuff) [🛡️Random(rand(100)<=35)]
            │       │       └── ⚔️ UseSkill(Shot)
            │       ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(Self.M_DroidTurret_CheckRun)]
            │       │   ├── ⚔️ UseSkill(MoveBack combo=TableCommand)
            │       │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=600.0) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️TimeLimit(3.0s [Timer1])]
            │       ├── ❓ Selector
            │       │   ├── ⚔️ UseSkill(ShotDozen combo=TableCommand) [🛡️UseableTime]
            │       │   └── ➡️ Sequence
            │       │       ├── ⚔️ UseSkill(Laser combo=TableCommand) [🛡️UseableTime]
            │       │       └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=400.0) && 🛡️TimeLimit(1.5s [Timer1])]
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(ShotDebuff combo=TableCommand)
            │       │   └── ⏳ Wait(0.2s)
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(Shot combo=TableCommand)
            │       │   └── ⏳ Wait(0.2s)
            │       └── ❓ Selector
            │           ├── ➡️ Sequence
            │           │   ├── ✨ UseEffect(['M_DroidTurret_CheckRun'])
            │           │   ├── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Common_BlockRun)]
            │           │   └── ⏳ Wait(0.9s)
            │           └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer3]) && 🛡️DistanceToTarget(dist<=700.0)]
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(Timer)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(Timer)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ⚔️ UseSkill(ShotDebuff) [🛡️Random(rand(100)<=35)]
                │       └── ⚔️ UseSkill(Shot)
                ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(Self.M_DroidTurret_CheckRun)]
                │   ├── ⚔️ UseSkill(MoveBack combo=TableCommand)
                │   └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist>=600.0) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️TimeLimit(3.0s [Timer1])]
                ├── ❓ Selector
                │   ├── ⚔️ UseSkill(ShotDozen combo=TableCommand) [🛡️UseableTime]
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(Laser combo=TableCommand) [🛡️UseableTime]
                │       └── ⚠️ CautionToTarget [🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=400.0) && 🛡️TimeLimit(1.5s [Timer1])]
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(ShotDebuff combo=TableCommand)
                │   └── ⏳ Wait(0.2s)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(Shot combo=TableCommand)
                │   └── ⏳ Wait(0.2s)
                └── ❓ Selector
                    ├── ➡️ Sequence
                    │   ├── ✨ UseEffect(['M_DroidTurret_CheckRun'])
                    │   ├── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Common_BlockRun)]
                    │   └── ⏳ Wait(0.9s)
                    └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [Timer3]) && 🛡️DistanceToTarget(dist<=1000.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 19 |
| Sequence | 17 |
| Dec/CheckActorEffect | 16 |
| Task/UseSkill | 15 |
| Dec/DistanceToTarget | 10 |
| Task/CautionToTarget | 9 |
| Task/Wait | 8 |
| Dec/TimeLimit | 8 |
| Dec/AggroLevel | 7 |
| Dec/Blackboard | 7 |
| Task/Blackboard | 7 |
| Task/UseableTimeReset | 6 |
| Dec/UseableTime | 6 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Dec/CheckStance | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/Random | 2 |
| Task/UseEffect | 2 |
| Task/MoveToTarget | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(StanbyToNormal combo=TableCommand)
- UseSkill(ShotDebuff)
- UseSkill(Shot)
- UseSkill(MoveBack combo=TableCommand)
- UseSkill(ShotDozen combo=TableCommand)
- UseSkill(Laser combo=TableCommand)
- UseSkill(ShotDebuff combo=TableCommand)
- UseSkill(Shot combo=TableCommand)
- UseSkill(ShotDebuff)
- UseSkill(Shot)
- UseSkill(MoveBack combo=TableCommand)
- UseSkill(ShotDozen combo=TableCommand)
- UseSkill(Laser combo=TableCommand)
- UseSkill(ShotDebuff combo=TableCommand)
- UseSkill(Shot combo=TableCommand)
