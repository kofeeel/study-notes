# M_Scarlet_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Self.? ON)]
        │   └── ❓ Selector [🛡️Blackboard(BattleStart)]
        │       └── ➡️ Sequence
        │           ├── ⏳ Wait(0.1s)
        │           ├── 📋 Blackboard(BattleStart)
        │           ├── 🚶 MoveToTarget
        │           └── ⚔️ UseSkill(AirDashCut)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [DownCautionTimer1])]
            ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Scarlet_CounterKnockDown ON)]
            │   └── ⚔️ UseSkill(MoveBack2)
            ├── ❓ Selector
            │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Scarlet_Phase2 ON) && 🛡️CheckStance(M_Scarlet_Default) && 🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckActorEffect(Self.M_Scarlet_PhaseChangeStart ON)]
            │   │   ├── ⚔️ UseSkill(PhaseChange1S combo=TableCommand) [🛡️CheckActorEffect(NOT Self.? ON)]
            │   │   ├── ⚔️ UseSkill(PhaseChange1_Attack4) [🛡️CheckActorEffect(Self.M_Scarlet_SkillMiddleEnd ON) && 🛡️CheckActorEffect(NOT Self.? ON)]
            │   │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Scarlet_SkillEnd ON)]
            │   │   │   ├── ⚔️ UseSkill(PhaseChange1_EndShieldZero combo=TableCommand) [🛡️CheckActorStat(ActorStatType_Shield<=?)]
            │   │   │   └── ⚔️ UseSkill(PhaseChange1_EndShieldOwn combo=TableCommand) [🛡️CheckActorStat(ActorStatType_Shield>?)]
            │   │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(Self.M_Scarlet_SkillCount ON)]
            │   │       ├── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │   │       │   ├── 📋 Blackboard(PhaseChange2) [🛡️Random(rand(100)<=50)]
            │   │       │   ├── 📋 Blackboard(PhaseChange2) [🛡️Random(rand(100)<=60)]
            │   │       │   └── 📋 Blackboard(PhaseChange2)
            │   │       └── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │   │           ├── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │   │           │   ├── ➡️ Sequence
            │   │           │   │   ├── ⚔️ UseSkill(PhaseChange1_Attack3 combo=TableCommand)
            │   │           │   │   └── ✨ UseEffect(['M_Scarlet_PhaseChangeSkillPercent1'])
            │   │           │   ├── ⚔️ UseSkill(PhaseChange1_Attack1 combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_Scarlet_PhaseChangeSkillPercent1 ON)]
            │   │           │   └── ⚔️ UseSkill(PhaseChange1_Attack2 combo=TableCommand)
            │   │           ├── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │   │           │   ├── ➡️ Sequence
            │   │           │   │   ├── ⚔️ UseSkill(PhaseChange1_Attack2 combo=TableCommand)
            │   │           │   │   └── ✨ UseEffect(['M_Scarlet_PhaseChangeSkillPercent1'])
            │   │           │   ├── ⚔️ UseSkill(PhaseChange1_Attack3 combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_Scarlet_PhaseChangeSkillPercent1 ON)]
            │   │           │   └── ⚔️ UseSkill(PhaseChange1_Attack1 combo=TableCommand)
            │   │           └── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │   │               ├── ➡️ Sequence
            │   │               │   ├── ⚔️ UseSkill(PhaseChange1_Attack1 combo=TableCommand)
            │   │               │   └── ✨ UseEffect(['M_Scarlet_PhaseChangeSkillPercent1'])
            │   │               ├── ⚔️ UseSkill(PhaseChange1_Attack3 combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_Scarlet_PhaseChangeSkillPercent1 ON)]
            │   │               └── ⚔️ UseSkill(PhaseChange1_Attack2 combo=TableCommand)
            │   └── ❓ Selector
            │       └── ❓ Selector [🛡️CheckActorEffect(Self.M_Scarlet_Phase2 ON) && 🛡️CheckActorStat(ActorStatType_HP<=5.0%)]
            │           ├── ⚔️ UseSkill(PhaseChange1S combo=TableCommand) [🛡️CheckActorEffect(NOT Self.? ON)]
            │           ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Scarlet_SkillMiddleEnd ON) && 🛡️CheckActorEffect(NOT Self.? ON)]
            │           │   ├── ⚔️ UseSkill(PhaseChange2_AttackRange)
            │           │   └── ⚔️ UseSkill(PhaseChange1_Attack4)
            │           ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Scarlet_SkillEnd ON)]
            │           │   ├── ⚔️ UseSkill(PhaseChange1_EndShieldZero combo=TableCommand) [🛡️CheckActorStat(ActorStatType_Shield<=?)]
            │           │   └── ⚔️ UseSkill(PhaseChange1_EndShieldOwn combo=TableCommand) [🛡️CheckActorStat(ActorStatType_Shield>?)]
            │           └── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(Self.M_Scarlet_SkillCount ON)]
            │               ├── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │               │   ├── 📋 Blackboard(PhaseChange2) [🛡️Random(rand(100)<=50)]
            │               │   ├── 📋 Blackboard(PhaseChange2) [🛡️Random(rand(100)<=60)]
            │               │   └── 📋 Blackboard(PhaseChange2)
            │               └── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │                   ├── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │                   │   ├── ➡️ Sequence
            │                   │   │   ├── ⚔️ UseSkill(PhaseChange1_Attack3 combo=TableCommand)
            │                   │   │   └── ✨ UseEffect(['M_Scarlet_PhaseChangeSkillPercent1'])
            │                   │   ├── ⚔️ UseSkill(PhaseChange1_Attack1 combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_Scarlet_PhaseChangeSkillPercent1 ON)]
            │                   │   └── ⚔️ UseSkill(PhaseChange1_Attack2 combo=TableCommand)
            │                   ├── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │                   │   ├── ➡️ Sequence
            │                   │   │   ├── ⚔️ UseSkill(PhaseChange1_Attack2 combo=TableCommand)
            │                   │   │   └── ✨ UseEffect(['M_Scarlet_PhaseChangeSkillPercent1'])
            │                   │   ├── ⚔️ UseSkill(PhaseChange1_Attack3 combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_Scarlet_PhaseChangeSkillPercent1 ON)]
            │                   │   └── ⚔️ UseSkill(PhaseChange1_Attack1 combo=TableCommand)
            │                   └── ❓ Selector [🛡️Blackboard(PhaseChange2)]
            │                       ├── ➡️ Sequence
            │                       │   ├── ⚔️ UseSkill(PhaseChange1_Attack1 combo=TableCommand)
            │                       │   └── ✨ UseEffect(['M_Scarlet_PhaseChangeSkillPercent1'])
            │                       ├── ⚔️ UseSkill(PhaseChange1_Attack3 combo=TableCommand) [🛡️CheckActorEffect(NOT Self.M_Scarlet_PhaseChangeSkillPercent1 ON)]
            │                       └── ⚔️ UseSkill(PhaseChange1_Attack2 combo=TableCommand)
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorStat(ActorStatType_HP>60.0%) && 🛡️CheckActorEffect(NOT Self.M_Scarlet_Phase2 ON) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(NOT Self.? ON)]
            │   ├── ➡️ Sequence [🛡️Blackboard(AITimer)]
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   ├── 🔄 UseableTimeReset
            │   │   └── 📋 Blackboard(AITimer)
            │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
            │   │   ├── 📋 Blackboard(FirstShot)
            │   │   └── ⚔️ UseSkill(MoveSideL|MoveSideR combo=TableCommand) [🛡️Random(rand(100)<=70)]
            │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
            │   │   ├── ⚔️ UseSkill(ParryRange)
            │   │   └── ⚔️ UseSkill(MoveSideL|MoveSideR combo=TableCommand)
            │   ├── ➡️ Sequence [🛡️UseableTime]
            │   │   └── ⚔️ UseSkill(JumpStampSlash)
            │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Scarlet_BehindSkillKeep ON)]
            │       ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │       │   └── ❓ Selector
            │       │       ├── ⚔️ UseSkill(MoveSideL|MoveSideR combo=TableCommand) [🛡️DistanceToTarget(dist>=300.0)]
            │       │       └── ⚔️ UseSkill(ParryRangeShot2|ParryRangeDash2 combo=TableCommand) [🛡️DistanceToTarget(dist>=300.0) && 🛡️CheckActorEffect(Target.? ON)]
            │       └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
            │           ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_Shield?100.0%)]
            │           │   └── ➡️ Sequence
            │           │       ├── ⚔️ UseSkill(Airspin)
            │           │       └── ✨ UseEffect(['M_Scarlet_ShieldZero'])
            │           ├── ❓ Selector
            │           │   ├── ⚔️ UseSkill(ParryRange) [🛡️CheckActorEffect(Self.M_Scarlet_RangeHitStart ON)]
            │           │   └── ❓ Selector
            │           │       ├── ⚔️ UseSkill(ParryRangeShot)
            │           │       └── ⚔️ UseSkill(ParryRangeDash)
            │           ├── ❓ Selector
            │           │   ├── ⚔️ UseSkill(ParryMelee)
            │           │   └── ❓ Selector
            │           │       ├── ❓ Selector [🛡️DistanceToTarget(dist<=400.0)]
            │           │       │   ├── ⚔️ UseSkill(MoveBackAttack) [🛡️CheckActorEffect(NOT Self.? ON)]
            │           │       │   └── ⚔️ UseSkill(MoveBack)
            │           │       └── ⚔️ UseSkill(SwingFast) [🛡️CheckActorStat(ActorStatType_HP<=95.0%)]
            │           ├── ❓ Selector [🛡️UseableTime]
            │           │   ├── ⚔️ UseSkill(BlinkCut) [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorEffect(NOT Target.Down ON)]
            │           │   ├── ⚔️ UseSkill(BackDashSpaceCut)
            │           │   └── ⚔️ UseSkill(AirDashCut)
            │           ├── ❓ Selector [🛡️DistanceToTarget(dist<=600.0) && 🛡️UseableTime]
            │           │   └── ⚔️ UseSkill(MoveSideL|MoveSideR|Stinger)
            │           ├── ❓ Selector
            │           │   ├── ⚔️ UseSkill(MoveBackShot) [🛡️CheckActorStat(ActorStatType_HP<=95.0%)]
            │           │   ├── ⚔️ UseSkill(MoveBack)
            │           │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.0s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=3000.0) && 🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(NOT Self.M_Common_HitProjectileResult ON)]
            │           ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=93.0%)]
            │           │   └── ⚔️ UseSkill(AreaSlash)
            │           ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
            │           │   ├── ⚔️ UseSkill(KickStamp)
            │           │   └── ⚔️ UseSkill(SwingCombo)
            │           ├── ❓ Selector
            │           │   ├── ⚔️ UseSkill(Stinger) [🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
            │           │   ├── ⚔️ UseSkill(SwingFast)
            │           │   └── ⚔️ UseSkill(SwingApproach)
            │           ├── ❓ Selector
            │           │   ├── ❓ Selector
            │           │   │   ├── ⚔️ UseSkill(ParryRange)
            │           │   │   └── ❓ Selector
            │           │   │       ├── ⚔️ UseSkill(ParryRangeShot)
            │           │   │       └── ⚔️ UseSkill(ParryRangeDash)
            │           │   └── ⚔️ UseSkill(ParryRangeShot2|ParryRangeDash2 combo=TableCommand) [🛡️DistanceToTarget(dist>=700.0) && 🛡️UseableTime]
            │           ├── ❓ Selector
            │           │   ├── ⚔️ UseSkill(Swing)
            │           │   └── ⚔️ UseSkill(SwingTriple) [🛡️DistanceToTarget(dist>=250.0)]
            │           ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
            │           │   ├── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=500.0)]
            │           │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1200.0) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_Common_HitProjectileResult ON)]
            │           └── ❓ Selector
            │               ├── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=100.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
            │               └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Scarlet_Phase2 ON) && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(NOT Target.? ON) && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorStat(ActorStatType_HP>5.0%)]
                ├── ➡️ Sequence [🛡️Blackboard(SideMoveReset) && 🛡️CheckActorEffect(Target.? ON)]
                │   ├── ✨ UseEffect(['M_Scarlet_ResetCoolTime_SideMove'])
                │   ├── ⚔️ UseSkill(MoveSideL|MoveSideR combo=TableCommand) [🛡️DistanceToTarget(dist>=300.0)]
                │   └── 📋 Blackboard(SideMoveReset)
                ├── ➡️ Sequence [🛡️Blackboard(AITimer2)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(AITimer2)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(MoveSideL|MoveSideR combo=TableCommand) [🛡️Random(rand(100)<=70)]
                ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   ├── ⚔️ UseSkill(ParryRange)
                │   └── ⚔️ UseSkill(MoveSideL|MoveSideR combo=TableCommand)
                ├── ❓ Selector
                │   ├── ⚔️ UseSkill(AreaCombo) [🛡️CheckActorStat(ActorStatType_HP<=50.0%) && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   └── ⚔️ UseSkill(JumpStampSlash) [🛡️UseableTime]
                └── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Scarlet_BehindSkillKeep ON)]
                    ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(MoveSideL|MoveSideR combo=TableCommand) [🛡️DistanceToTarget(dist>=300.0)]
                    │       └── ⚔️ UseSkill(ParryRangeShot2|ParryRangeDash2 combo=TableCommand) [🛡️DistanceToTarget(dist>=300.0) && 🛡️CheckActorEffect(Target.? ON)]
                    └── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                        ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_Shield?100.0%)]
                        │   └── ➡️ Sequence
                        │       ├── ⚔️ UseSkill(Airspin)
                        │       └── ✨ UseEffect(['M_Scarlet_ShieldZero'])
                        ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Scarlet_LinkBreakEnd ON) && 🛡️CheckActorStat(ActorStatType_HP<=30.0%)]
                        │   └── ⚔️ UseSkill(LinkBreakChanceAttack)
                        ├── ❓ Selector
                        │   ├── ⚔️ UseSkill(ParryRange) [🛡️CheckActorEffect(Self.M_Scarlet_RangeHitStart ON)]
                        │   └── ❓ Selector
                        │       ├── ⚔️ UseSkill(ParryRangeShot)
                        │       └── ⚔️ UseSkill(ParryRangeDash)
                        ├── ❓ Selector
                        │   ├── ⚔️ UseSkill(ParryMeleeCounter)
                        │   ├── ⚔️ UseSkill(ParryMelee) [🛡️CheckActorEffect(NOT Self.? ON)]
                        │   └── ❓ Selector [🛡️DistanceToTarget(dist<=400.0)]
                        │       ├── ⚔️ UseSkill(MoveBackAttack) [🛡️CheckActorEffect(NOT Self.? ON)]
                        │       └── ⚔️ UseSkill(MoveBack)
                        ├── ❓ Selector
                        │   ├── ⚔️ UseSkill(BlinkCut) [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorEffect(NOT Target.Down ON)]
                        │   └── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                        │       ├── ⚔️ UseSkill(BackDashSpaceCut)
                        │       ├── ⚔️ UseSkill(AirDashCut)
                        │       └── ⚔️ UseSkill(AirDashSpaceCut)
                        ├── ❓ Selector
                        │   ├── ⚔️ UseSkill(MoveBack) [🛡️CheckActorEffect(Self.M_Scarlet_HitResult ON) && 🛡️DistanceToTarget(dist<=450.0)]
                        │   └── ⚔️ UseSkill(MoveSideL|MoveSideR|Stinger) [🛡️DistanceToTarget(dist<=600.0)]
                        ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=50.0%)]
                        │   ├── ⚔️ UseSkill(BlinkCombo)
                        │   └── ⚔️ UseSkill(BlinkShot) [🛡️DistanceToTarget(dist<=600.0)]
                        ├── ❓ Selector
                        │   ├── ⚔️ UseSkill(KickStamp)
                        │   └── ⚔️ UseSkill(SwingCombo)
                        ├── ❓ Selector
                        │   ├── ⚔️ UseSkill(Stinger)
                        │   ├── ⚔️ UseSkill(SwingApproach)
                        │   └── ⚔️ UseSkill(SwingFast)
                        ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                        │   ├── ⚔️ UseSkill(AreaSlash) [🛡️CheckActorStat(ActorStatType_HP>=45.0%) && 🛡️UseableTime]
                        │   └── ⚔️ UseSkill(AreaSlash2) [🛡️CheckActorStat(ActorStatType_HP<=45.0%)]
                        ├── ➡️ Sequence
                        │   ├── ⚔️ UseSkill(MoveBackShot2)
                        │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.2s [CautionTimer3]) && 🛡️DistanceToTarget(dist<=3000.0) && 🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(NOT Self.M_Common_HitProjectileResult ON)]
                        ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                        │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=3500.0) && 🛡️DistanceToTarget(dist>=500.0) && 🛡️CheckActorEffect(NOT Self.M_Common_HitProjectileResult ON)]
                        ├── ❓ Selector
                        │   ├── ❓ Selector
                        │   │   ├── ⚔️ UseSkill(ParryRange)
                        │   │   └── ❓ Selector
                        │   │       ├── ⚔️ UseSkill(ParryRangeShot)
                        │   │       └── ⚔️ UseSkill(ParryRangeDash)
                        │   └── ⚔️ UseSkill(ParryRangeShot2|ParryRangeDash2 combo=TableCommand) [🛡️DistanceToTarget(dist>=700.0) && 🛡️UseableTime]
                        ├── ❓ Selector
                        │   ├── ⚔️ UseSkill(Swing)
                        │   └── ⚔️ UseSkill(SwingTriple) [🛡️DistanceToTarget(dist>=250.0)]
                        └── ❓ Selector
                            ├── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=100.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
                            └── 🚶 MoveToTarget [🛡️DistanceToTarget(dist>=400.0)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 100 |
| Selector | 74 |
| Dec/CheckActorEffect | 65 |
| Dec/DistanceToTarget | 32 |
| Sequence | 19 |
| Dec/CheckActorStat | 19 |
| Dec/Blackboard | 16 |
| Task/Blackboard | 12 |
| Task/UseEffect | 9 |
| Dec/Random | 8 |
| Task/UseableTimeReset | 8 |
| Dec/UseableTime | 8 |
| Task/CautionToTarget | 7 |
| Dec/TimeLimit | 7 |
| Task/MoveToTarget | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Dec/AggroLevel | 3 |
| Dec/AimMe | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/CheckStance | 1 |

## 스킬 목록
- UseSkill(AirDashCut)
- UseSkill(MoveBack2)
- UseSkill(PhaseChange1S combo=TableCommand)
- UseSkill(PhaseChange1_Attack4)
- UseSkill(PhaseChange1_EndShieldZero combo=TableCommand)
- UseSkill(PhaseChange1_EndShieldOwn combo=TableCommand)
- UseSkill(PhaseChange1_Attack3 combo=TableCommand)
- UseSkill(PhaseChange1_Attack1 combo=TableCommand)
- UseSkill(PhaseChange1_Attack2 combo=TableCommand)
- UseSkill(PhaseChange1_Attack2 combo=TableCommand)
- UseSkill(PhaseChange1_Attack3 combo=TableCommand)
- UseSkill(PhaseChange1_Attack1 combo=TableCommand)
- UseSkill(PhaseChange1_Attack1 combo=TableCommand)
- UseSkill(PhaseChange1_Attack3 combo=TableCommand)
- UseSkill(PhaseChange1_Attack2 combo=TableCommand)
- UseSkill(PhaseChange1S combo=TableCommand)
- UseSkill(PhaseChange2_AttackRange)
- UseSkill(PhaseChange1_Attack4)
- UseSkill(PhaseChange1_EndShieldZero combo=TableCommand)
- UseSkill(PhaseChange1_EndShieldOwn combo=TableCommand)
- UseSkill(PhaseChange1_Attack3 combo=TableCommand)
- UseSkill(PhaseChange1_Attack1 combo=TableCommand)
- UseSkill(PhaseChange1_Attack2 combo=TableCommand)
- UseSkill(PhaseChange1_Attack2 combo=TableCommand)
- UseSkill(PhaseChange1_Attack3 combo=TableCommand)
- UseSkill(PhaseChange1_Attack1 combo=TableCommand)
- UseSkill(PhaseChange1_Attack1 combo=TableCommand)
- UseSkill(PhaseChange1_Attack3 combo=TableCommand)
- UseSkill(PhaseChange1_Attack2 combo=TableCommand)
- UseSkill(MoveSideL|MoveSideR combo=TableCommand)
- UseSkill(ParryRange)
- UseSkill(MoveSideL|MoveSideR combo=TableCommand)
- UseSkill(JumpStampSlash)
- UseSkill(MoveSideL|MoveSideR combo=TableCommand)
- UseSkill(ParryRangeShot2|ParryRangeDash2 combo=TableCommand)
- UseSkill(Airspin)
- UseSkill(ParryRange)
- UseSkill(ParryRangeShot)
- UseSkill(ParryRangeDash)
- UseSkill(ParryMelee)
- UseSkill(MoveBackAttack)
- UseSkill(MoveBack)
- UseSkill(SwingFast)
- UseSkill(BlinkCut)
- UseSkill(BackDashSpaceCut)
- UseSkill(AirDashCut)
- UseSkill(MoveSideL|MoveSideR|Stinger)
- UseSkill(MoveBackShot)
- UseSkill(MoveBack)
- UseSkill(AreaSlash)
- UseSkill(KickStamp)
- UseSkill(SwingCombo)
- UseSkill(Stinger)
- UseSkill(SwingFast)
- UseSkill(SwingApproach)
- UseSkill(ParryRange)
- UseSkill(ParryRangeShot)
- UseSkill(ParryRangeDash)
- UseSkill(ParryRangeShot2|ParryRangeDash2 combo=TableCommand)
- UseSkill(Swing)
- UseSkill(SwingTriple)
- UseSkill(MoveSideL|MoveSideR combo=TableCommand)
- UseSkill(MoveSideL|MoveSideR combo=TableCommand)
- UseSkill(ParryRange)
- UseSkill(MoveSideL|MoveSideR combo=TableCommand)
- UseSkill(AreaCombo)
- UseSkill(JumpStampSlash)
- UseSkill(MoveSideL|MoveSideR combo=TableCommand)
- UseSkill(ParryRangeShot2|ParryRangeDash2 combo=TableCommand)
- UseSkill(Airspin)
- UseSkill(LinkBreakChanceAttack)
- UseSkill(ParryRange)
- UseSkill(ParryRangeShot)
- UseSkill(ParryRangeDash)
- UseSkill(ParryMeleeCounter)
- UseSkill(ParryMelee)
- UseSkill(MoveBackAttack)
- UseSkill(MoveBack)
- UseSkill(BlinkCut)
- UseSkill(BackDashSpaceCut)
- UseSkill(AirDashCut)
- UseSkill(AirDashSpaceCut)
- UseSkill(MoveBack)
- UseSkill(MoveSideL|MoveSideR|Stinger)
- UseSkill(BlinkCombo)
- UseSkill(BlinkShot)
- UseSkill(KickStamp)
- UseSkill(SwingCombo)
- UseSkill(Stinger)
- UseSkill(SwingApproach)
- UseSkill(SwingFast)
- UseSkill(AreaSlash)
- UseSkill(AreaSlash2)
- UseSkill(MoveBackShot2)
- UseSkill(ParryRange)
- UseSkill(ParryRangeShot)
- UseSkill(ParryRangeDash)
- UseSkill(ParryRangeShot2|ParryRangeDash2 combo=TableCommand)
- UseSkill(Swing)
- UseSkill(SwingTriple)
