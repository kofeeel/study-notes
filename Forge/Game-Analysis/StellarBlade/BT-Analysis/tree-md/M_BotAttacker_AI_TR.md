# M_BotAttacker_AI_TR

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
            │           └── 📋 Blackboard(BattleState)
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
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(Self.TR_SkillAll ON)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │       │   ├── 📋 Blackboard(FirstShot)
            │       │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
            │       ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON)]
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
            │       └── ❓ Selector [🛡️CheckStance(M_BotAttacker_Default) && 🛡️CheckActorEffect(Self.M_Bot_TypeAttacker ON) && 🛡️CheckActorEffect(Self.M_Bot_TypeDefense)]
            │           ├── ⚔️ UseSkill(MoveBack) [🛡️CheckActorEffect(Self.M_Bot_HitResult ON)]
            │           ├── ⚔️ UseSkill(Dash) [🛡️DistanceToTarget(dist>=500.0)]
            │           ├── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1300.0) && 🛡️DistanceToTarget(dist>=650.0) && 🛡️UseableTime]
            │           ├── ⚔️ UseSkill(MoveBack) [🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
            │           ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
            │           │   └── ⚔️ UseSkill(Stinger)
            │           ├── ➡️ Sequence [🛡️DistanceToTarget(dist>=400.0)]
            │           │   ├── ⚔️ UseSkill(Stamp)
            │           │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist>=500.0)]
            │           ├── ⚔️ UseSkill(SwingRushChain|SwingRush)
            │           ├── ❓ Selector
            │           │   └── ⚔️ UseSkill(Swing) [🛡️DistanceToTarget(dist<=300.0)]
            │           └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Bot_CheckBack ON)]
            │               └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillAll ON)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │       └── ➡️ Sequence
            │           ├── ❓ Selector
            │           │   ├── ➡️ Sequence [🛡️Blackboard(SkillSpecialCheck)]
            │           │   │   ├── ⚔️ UseSkill(Swing)
            │           │   │   └── 📋 Blackboard(SkillSpecialCheck)
            │           │   └── ➡️ Sequence [🛡️Blackboard(SkillSpecialCheck)]
            │           │       ├── ⚔️ UseSkill(Stamp_TR)
            │           │       └── 📋 Blackboard(SkillSpecialCheck)
            │           ├── ✨ UseEffect(['TR_SkillCoolTimeReset_BotAttacker'])
            │           └── ⏳ Wait(3.0s)
            └── ❓ Selector [🛡️CheckActorEffect(Self.TR_SkillNormal ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillSpecial ON) && 🛡️CheckActorEffect(NOT Self.TR_SkillAll ON)]
                └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(Slash|SwingRush)
                    │   └── ⏳ Wait(2.0s)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 27 |
| Selector | 22 |
| Task/UseSkill | 18 |
| Sequence | 17 |
| Dec/DistanceToTarget | 11 |
| Dec/AggroLevel | 9 |
| Task/Blackboard | 8 |
| Dec/Blackboard | 7 |
| Dec/TimeLimit | 5 |
| Task/Wait | 4 |
| Task/CautionToTarget | 4 |
| Dec/IsAlive | 3 |
| Task/MoveToTarget | 3 |
| Dec/CheckActorStat | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/CheckStance | 2 |
| Dec/AimMe | 2 |
| Dec/Random | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Task/PlayShow | 1 |
| Task/UseableTimeReset | 1 |
| Dec/UseableTime | 1 |
| Task/UseEffect | 1 |

## 스킬 목록
- UseSkill(BattleEnd)
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
- UseSkill(Swing)
- UseSkill(Stamp_TR)
- UseSkill(Slash|SwingRush)
