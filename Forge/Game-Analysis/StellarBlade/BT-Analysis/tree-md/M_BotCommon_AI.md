# M_BotCommon_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── ⚔️ UseSkill(BattleEnd) [🛡️Blackboard(BattleState)]
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
            │   ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent ON)]
            │   │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │   │       ├── 📋 Blackboard(BattleState)
            │   │       └── 🚶 MoveToTarget [🛡️TimeLimit(4.0s [StartTimer1])]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │           ├── 📋 Blackboard(BattleState)
            │           ├── 🎬 PlayShow
            │           ├── 🚶 MoveToTarget [🛡️TimeLimit(4.0s [StartTimer1])]
            │           └── ❓ Selector
            │               ├── ⚔️ UseSkill(Slash) [🛡️Random(rand(100)<=50)]
            │               └── ⚔️ UseSkill(SwingCombo)
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [DownCautionTimer1])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️CheckStance(M_BotUpperBody_Default) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(Swing)
            │       │   └── ⏳ WaitTimeRandom
            │       └── 🚶 MoveToTarget
            ├── ❓ Selector [🛡️CheckStance(NOT M_BotUpperBody_Default) && 🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── 🔧 MountingEquipment [🛡️IsEmptyEquipment && 🛡️DistanceToTarget(dist>=500.0)]
            │   │   ├── ⏳ Wait(1.5s)
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_Weapon)]
            │       │   ├── 🔧 MountingEquipment [🛡️CheckLastAttackedTime && 🛡️IsEmptyEquipment && 🛡️DistanceToTarget(dist>=500.0) && 🛡️Random(rand(100)<=40)]
            │       │   ├── ⏳ Wait(1.5s)
            │       │   └── 📋 Blackboard(BB_Weapon)
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │       │   ├── 📋 Blackboard(FirstShot)
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=85)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ⚔️ UseSkill(TurnSkill)
            │       ├── ❓ Selector [🛡️CheckStance(M_BotAttacker_Default) && 🛡️CheckActorEffect(Self.M_Bot_TypeAttacker ON) && 🛡️CheckActorEffect(Self.M_Bot_TypeDefense)]
            │       │   ├── ⚔️ UseSkill(MoveBack) [🛡️CheckActorEffect(Self.M_Bot_HitResult ON)]
            │       │   ├── ⚔️ UseSkill(Dash) [🛡️DistanceToTarget(dist>=500.0)]
            │       │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1300.0) && 🛡️DistanceToTarget(dist>=650.0) && 🛡️UseableTime]
            │       │   ├── ⚔️ UseSkill(MoveBack) [🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
            │       │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
            │       │   │   └── ⚔️ UseSkill(Stinger)
            │       │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0)]
            │       │   │   ├── ⚔️ UseSkill(Stamp)
            │       │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=500.0)]
            │       │   ├── ⚔️ UseSkill(SwingRushChain|SwingRush)
            │       │   ├── ❓ Selector
            │       │   │   └── ⚔️ UseSkill(Swing) [🛡️DistanceToTarget(dist<=300.0)]
            │       │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Bot_CheckBack ON)]
            │       │       └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
            │       ├── ❓ Selector [🛡️CheckStance(M_BotCommon_Default) && 🛡️CheckActorEffect(Self.M_Bot_TypeDefense) && 🛡️CheckActorEffect(Self.M_Bot_TypeAttacker)]
            │       │   ├── ❓ Selector
            │       │   │   ├── ⚔️ UseSkill(SwingChain) [🛡️DistanceToTarget(dist>=250.0)]
            │       │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1300.0) && 🛡️DistanceToTarget(dist>=300.0) && 🛡️UseableTime]
            │       │   ├── ❓ Selector
            │       │   │   ├── ⚔️ UseSkill(SwingCombo) [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️Random(rand(100)<=10)]
            │       │   │   ├── ⚔️ UseSkill(Slash)
            │       │   │   └── ⚔️ UseSkill(Swing) [🛡️DistanceToTarget(dist<=300.0)]
            │       │   └── ❓ Selector
            │       │       └── 🚶 MoveToTarget
            │       └── ❓ Selector [🛡️CheckStance(M_BotDefense_Default) && 🛡️CheckActorEffect(Self.M_Bot_TypeDefense ON) && 🛡️CheckActorEffect(Self.M_Bot_TypeAttacker)]
            │           ├── ❓ Selector
            │           │   ├── ⚔️ UseSkill(GuardCounter) [🛡️DistanceToTarget(dist>=300.0)]
            │           │   └── ⚔️ UseSkill(ShieldRush)
            │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1300.0) && 🛡️DistanceToTarget(dist>=650.0)]
            │           ├── ➡️ Sequence
            │           │   └── ⚔️ UseSkill(SwingChain|SwingRush)
            │           ├── ❓ Selector
            │           │   ├── ⚔️ UseSkill(Swing)
            │           │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=600.0) && 🛡️DistanceToTarget(dist>=100.0)]
            │           └── ❓ Selector
            │               └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️CheckStance(NOT M_BotUpperBody_Default) && 🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB_Weapon)]
                │   ├── 🔧 MountingEquipment [🛡️CheckLastAttackedTime && 🛡️IsEmptyEquipment && 🛡️DistanceToTarget(dist>=500.0) && 🛡️Random(rand(100)<=40)]
                │   ├── ⏳ Wait(1.5s)
                │   └── 📋 Blackboard(BB_Weapon)
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileCheck ON) && 🛡️Random(rand(100)<=85)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=40.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ⚔️ UseSkill(TurnSkill)
                ├── ❓ Selector [🛡️CheckStance(M_BotAttacker_Default) && 🛡️CheckActorEffect(Self.M_Bot_TypeAttacker ON) && 🛡️CheckActorEffect(Self.M_Bot_TypeDefense)]
                │   ├── ⚔️ UseSkill(MoveBack) [🛡️CheckActorEffect(Self.M_Bot_HitResult ON)]
                │   ├── ⚔️ UseSkill(Dash) [🛡️DistanceToTarget(dist>=500.0)]
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1300.0) && 🛡️DistanceToTarget(dist>=650.0) && 🛡️UseableTime]
                │   ├── ⚔️ UseSkill(MoveBack) [🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
                │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                │   │   └── ⚔️ UseSkill(Stinger)
                │   ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0)]
                │   │   ├── ⚔️ UseSkill(Stamp)
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=500.0)]
                │   ├── ⚔️ UseSkill(SwingRushChain|SwingRush)
                │   ├── ❓ Selector
                │   │   └── ⚔️ UseSkill(Swing) [🛡️DistanceToTarget(dist<=300.0)]
                │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Bot_CheckBack ON)]
                │       └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
                ├── ❓ Selector [🛡️CheckStance(M_BotCommon_Default) && 🛡️CheckActorEffect(Self.M_Bot_TypeDefense) && 🛡️CheckActorEffect(Self.M_Bot_TypeAttacker)]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(SwingChain) [🛡️DistanceToTarget(dist>=250.0)]
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1300.0) && 🛡️DistanceToTarget(dist>=300.0) && 🛡️UseableTime]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(SwingCombo) [🛡️CheckActorStat(ActorStatType_HP<=70.0%) && 🛡️Random(rand(100)<=10)]
                │   │   ├── ⚔️ UseSkill(Slash)
                │   │   └── ⚔️ UseSkill(Swing) [🛡️DistanceToTarget(dist<=300.0)]
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckStance(M_BotDefense_Default) && 🛡️CheckActorEffect(Self.M_Bot_TypeDefense ON) && 🛡️CheckActorEffect(Self.M_Bot_TypeAttacker)]
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(GuardCounter) [🛡️DistanceToTarget(dist>=300.0)]
                    │   └── ⚔️ UseSkill(ShieldRush)
                    ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1300.0) && 🛡️DistanceToTarget(dist>=650.0)]
                    ├── ➡️ Sequence
                    │   └── ⚔️ UseSkill(SwingChain|SwingRush)
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(Swing)
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=600.0) && 🛡️DistanceToTarget(dist>=100.0)]
                    └── ❓ Selector
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 42 |
| Selector | 38 |
| Dec/DistanceToTarget | 37 |
| Dec/CheckActorEffect | 32 |
| Sequence | 22 |
| Dec/TimeLimit | 14 |
| Task/CautionToTarget | 13 |
| Dec/Random | 11 |
| Dec/AggroLevel | 10 |
| Dec/Blackboard | 9 |
| Task/Blackboard | 9 |
| Task/MoveToTarget | 9 |
| Dec/CheckStance | 9 |
| Dec/CheckActorStat | 8 |
| Task/Wait | 5 |
| Dec/AimMe | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Task/MountingEquipment | 3 |
| Dec/IsEmptyEquipment | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/CheckLastAttackedTime | 2 |
| Task/UseableTimeReset | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/PlayShow | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(Slash)
- UseSkill(SwingCombo)
- UseSkill(Swing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(TurnSkill)
- UseSkill(MoveBack)
- UseSkill(Dash)
- UseSkill(MoveBack)
- UseSkill(Stinger)
- UseSkill(Stamp)
- UseSkill(SwingRushChain|SwingRush)
- UseSkill(Swing)
- UseSkill(SwingChain)
- UseSkill(SwingCombo)
- UseSkill(Slash)
- UseSkill(Swing)
- UseSkill(GuardCounter)
- UseSkill(ShieldRush)
- UseSkill(SwingChain|SwingRush)
- UseSkill(Swing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(TurnSkill)
- UseSkill(MoveBack)
- UseSkill(Dash)
- UseSkill(MoveBack)
- UseSkill(Stinger)
- UseSkill(Stamp)
- UseSkill(SwingRushChain|SwingRush)
- UseSkill(Swing)
- UseSkill(SwingChain)
- UseSkill(SwingCombo)
- UseSkill(Slash)
- UseSkill(Swing)
- UseSkill(GuardCounter)
- UseSkill(ShieldRush)
- UseSkill(SwingChain|SwingRush)
- UseSkill(Swing)
