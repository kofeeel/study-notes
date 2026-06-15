# M_BotAttacker_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── ⚔️ UseSkill(BattleEnd) [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │           ├── 🎬 PlayShow
            │           ├── 📋 Blackboard(BattleState)
            │           └── ❓ Selector
            │               ├── ⚔️ UseSkill(Missile) [🛡️Random(rand(100)<=40)]
            │               ├── ⚔️ UseSkill(Stamp) [🛡️Random(rand(100)<=30)]
            │               └── ⚔️ UseSkill(Dash) [🛡️Random(rand(100)<=30)]
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
            │   │   ├── 🔧 MountingEquipment [🛡️IsEmptyEquipment && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckLastAttackedTime]
            │   │   ├── ⏳ Wait(1.5s)
            │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB_Weapon)]
            │       │   ├── 🔧 MountingEquipment [🛡️IsEmptyEquipment && 🛡️DistanceToTarget(dist>=500.0) && 🛡️Random(rand(100)<=40) && 🛡️CheckLastAttackedTime]
            │       │   ├── ⏳ Wait(1.5s)
            │       │   └── 📋 Blackboard(BB_Weapon)
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │       │   ├── 📋 Blackboard(FirstShot)
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=35.0%)]
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
            │       ├── ⚔️ UseSkill(TurnSkill)
            │       ├── ❓ Selector
            │       │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=300.0)]
            │       │   │   ├── 📋 Blackboard(DistanceCheck1)
            │       │   │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_Bot_CheckRangeSkill)]
            │       │   │       ├── ⚔️ UseSkill(Missile) [🛡️DistanceToTarget(dist>=600.0)]
            │       │   │       └── ⚔️ UseSkill(MissileLaunch)
            │       │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
            │       │       └── 📋 Blackboard(DistanceCheck1)
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
            │       │   ├── ⚔️ UseSkill(MissileLaunch)
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
            │           │   ├── ⚔️ UseSkill(Swing) [🛡️DistanceToTarget(dist<=300.0)]
            │           │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=600.0) && 🛡️DistanceToTarget(dist>=100.0)]
            │           └── ❓ Selector
            │               └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️CheckStance(NOT M_BotUpperBody_Default) && 🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB_Weapon)]
                │   ├── 🔧 MountingEquipment [🛡️IsEmptyEquipment && 🛡️DistanceToTarget(dist>=500.0) && 🛡️Random(rand(100)<=40) && 🛡️CheckLastAttackedTime]
                │   ├── ⏳ Wait(1.5s)
                │   └── 📋 Blackboard(BB_Weapon)
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=35.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ⚔️ UseSkill(TurnSkill)
                ├── ❓ Selector
                │   ├── ➡️ Sequence [🛡️Blackboard(DistanceCheck1) && 🛡️DistanceToTarget(dist>=300.0)]
                │   │   ├── 📋 Blackboard(DistanceCheck1)
                │   │   └── ❓ Selector [🛡️CheckActorEffect(Self.M_Bot_CheckRangeSkill)]
                │   │       ├── ⚔️ UseSkill(Missile) [🛡️DistanceToTarget(dist>=600.0)]
                │   │       └── ⚔️ UseSkill(MissileLaunch)
                │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0) && 🛡️TimeLimit(-1.0s [TimerProjectile])]
                │       └── 📋 Blackboard(DistanceCheck1)
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
                │   ├── ⚔️ UseSkill(MissileLaunch)
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
                    │   ├── ⚔️ UseSkill(Swing) [🛡️DistanceToTarget(dist<=300.0)]
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=600.0) && 🛡️DistanceToTarget(dist>=100.0)]
                    └── ❓ Selector
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 49 |
| Dec/DistanceToTarget | 45 |
| Selector | 41 |
| Dec/CheckActorEffect | 33 |
| Sequence | 25 |
| Dec/TimeLimit | 14 |
| Dec/Random | 13 |
| Task/CautionToTarget | 13 |
| Task/Blackboard | 12 |
| Dec/Blackboard | 10 |
| Dec/AggroLevel | 9 |
| Dec/CheckStance | 9 |
| Dec/CheckActorStat | 8 |
| Task/MoveToTarget | 7 |
| Task/Wait | 5 |
| Dec/AimMe | 4 |
| Dec/UseableTime | 4 |
| Dec/IsAlive | 3 |
| Task/MountingEquipment | 3 |
| Dec/IsEmptyEquipment | 3 |
| Dec/CheckLastAttackedTime | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/UseableTimeReset | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Task/PlayShow | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
- UseSkill(Missile)
- UseSkill(Stamp)
- UseSkill(Dash)
- UseSkill(Swing)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(TurnSkill)
- UseSkill(Missile)
- UseSkill(MissileLaunch)
- UseSkill(MoveBack)
- UseSkill(Dash)
- UseSkill(MoveBack)
- UseSkill(Stinger)
- UseSkill(Stamp)
- UseSkill(SwingRushChain|SwingRush)
- UseSkill(Swing)
- UseSkill(MissileLaunch)
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
- UseSkill(Missile)
- UseSkill(MissileLaunch)
- UseSkill(MoveBack)
- UseSkill(Dash)
- UseSkill(MoveBack)
- UseSkill(Stinger)
- UseSkill(Stamp)
- UseSkill(SwingRushChain|SwingRush)
- UseSkill(Swing)
- UseSkill(MissileLaunch)
- UseSkill(SwingChain)
- UseSkill(SwingCombo)
- UseSkill(Slash)
- UseSkill(Swing)
- UseSkill(GuardCounter)
- UseSkill(ShieldRush)
- UseSkill(SwingChain|SwingRush)
- UseSkill(Swing)
