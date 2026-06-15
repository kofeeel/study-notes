# M_SunFlower_AI

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
            │   └── ❓ Selector [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence
            │           ├── 📋 Blackboard(BattleStart)
            │           └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=50) && 🛡️TimeLimit(4.0s [Timer_BattleStartSkill]) && 🛡️DistanceToTarget(dist>=400.0)]
            │       │       ├── ⚔️ UseSkill(Swing)
            │       │       └── ⚔️ UseSkill(Spit combo=TableCommand)
            │       ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │       │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [DownCautionTimer1])]
            │       ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>600.0) && 🛡️DistanceToTarget(dist<800.0) && 🛡️Random(rand(100)<=70)]
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(Spit)
            │       │   └── ⏳ WaitTimeRandom
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(Swing)
            │       │   └── ⏳ WaitTimeRandom
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️Random(rand(100)<=50) && 🛡️TimeLimit(4.0s [Timer_BattleStartSkill]) && 🛡️DistanceToTarget(dist>=400.0)]
                │       ├── ⚔️ UseSkill(Swing)
                │       └── ⚔️ UseSkill(Spit combo=TableCommand)
                ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [DownCautionTimer1])]
                ├── ➡️ Sequence [🛡️UseableTime && 🛡️DistanceToTarget(dist>600.0)]
                │   ├── ⚔️ UseSkill(JumpSlam)
                │   └── ⏳ WaitTimeRandom
                ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>600.0) && 🛡️DistanceToTarget(dist<1100.0) && 🛡️Random(rand(100)<=60)]
                ├── ➡️ Sequence [🛡️DistanceToTarget(dist>600.0)]
                │   ├── ⚔️ UseSkill(Spit)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(SwingChain)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(Swing)
                │   └── ⏳ WaitTimeRandom
                └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>400.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 16 |
| Dec/CheckActorEffect | 15 |
| Sequence | 13 |
| Task/UseSkill | 10 |
| Task/CautionToTarget | 9 |
| Dec/DistanceToTarget | 9 |
| Dec/TimeLimit | 8 |
| Dec/AggroLevel | 7 |
| Task/WaitTimeRandom | 7 |
| Dec/Blackboard | 5 |
| Task/Blackboard | 5 |
| Dec/Random | 4 |
| Dec/IsAlive | 3 |
| Task/MoveToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/UseableTimeReset | 1 |
| Dec/UseableTime | 1 |

## 스킬 목록
- UseSkill(Swing)
- UseSkill(Spit combo=TableCommand)
- UseSkill(Spit)
- UseSkill(Swing)
- UseSkill(Swing)
- UseSkill(Spit combo=TableCommand)
- UseSkill(JumpSlam)
- UseSkill(Spit)
- UseSkill(SwingChain)
- UseSkill(Swing)
