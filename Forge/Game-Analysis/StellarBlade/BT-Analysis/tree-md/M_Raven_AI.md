# M_Raven_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.6s [AbnormalTimer])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ➡️ Sequence [🛡️Blackboard(SwordBuffFX)]
                │   ├── 📋 Blackboard(SwordBuffFX)
                │   └── ✨ UseEffect(['M_Raven_BuffFX']) [🛡️CheckActorEffect(NOT Self.M_Raven_BuffFX ON)]
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP>60.0%) && 🛡️CheckStance(M_Raven_Default)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── ✨ UseEffect(['M_Raven_QTETimer'])
                │   │   └── 📋 Blackboard(BB1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Target.P_Eve_Beta_SwordAura ON) && 🛡️DistanceToTarget(dist>=300.0)]
                │   │   └── ⚔️ UseSkill(BetaEvadeLeft|BetaEvadeRight combo=TableCommand)
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Target.P_Eve_Beta_SwordAura2 ON) && 🛡️DistanceToTarget(dist>=300.0)]
                │   │   └── ⚔️ UseSkill(BetaEvadeLeft|BetaEvadeRight combo=TableCommand)
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Raven_RushWaitTime ON) && 🛡️CheckActorEffect(NOT Target.P_Eve_Beta_SwordAura2 ON)]
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [RushWaitTimer1])]
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(FirstTime)]
                │   │   └── ⚔️ UseSkill(ParryPreview1)
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Raven_ParryPreviewChain1 ON) && 🛡️CheckActorEffect(NOT Self.M_Raven_ParryPreviewChain2 ON)]
                │   │   └── ⚔️ UseSkill(Parry)
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Raven_ParryChain ON) && 🛡️CheckActorEffect(NOT Self.M_Raven_ParryPreviewChain2 ON)]
                │   │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Raven_BetaGrabCheck ON)]
                │   │   │   ├── ➡️ Sequence
                │   │   │   │   ├── ⚔️ UseSkill(BetaGrabChain)
                │   │   │   │   └── ⚔️ UseSkill(BetaCounterGrab) [🛡️CheckActorEffect(Self.M_Raven_GrabChain ON)]
                │   │   │   └── ⚔️ UseSkill(BetaGrab) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(BetaRapidCombo|BetaChargeCombo) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(EvadeBackRush|ParryPreview2)
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Raven_ParryPreviewChain2 ON) && 🛡️CheckActorEffect(NOT Self.M_Raven_ParryPreviewChain1 ON)]
                │   │   └── ⚔️ UseSkill(ParryCounterCombo)
                │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=400.0) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   ├── ⚔️ UseSkill(ChaseGrab) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(ChaseCombo)
                │   ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(FirstTime)]
                │   │   ├── ⚔️ UseSkill(BetaRapidCombo|BetaChargeCombo|BetaGrab)
                │   │   └── 📋 Blackboard(FirstTime)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(FirstTime)]
                │   │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Raven_BetaGrabCheck ON)]
                │   │   │   ├── ➡️ Sequence
                │   │   │   │   ├── ⚔️ UseSkill(BetaGrabChain)
                │   │   │   │   └── ⚔️ UseSkill(BetaCounterGrab) [🛡️CheckActorEffect(Self.M_Raven_GrabChain ON)]
                │   │   │   └── ⚔️ UseSkill(BetaGrab) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(BetaRapidCombo|BetaChargeCombo) [🛡️CheckActorEffect(NOT Self.? ON)]
                │   ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   ├── ⚔️ UseSkill(RapidMoveBack)
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1400.0) && 🛡️DistanceToTarget(dist>=250.0) && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(MoveCombo|MoveChainCombo)
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(Slash|SlashChain)
                │   └── ❓ Selector
                │       ├── ⚠️ CautionToTarget [🛡️UseableTime && 🛡️TimeLimit(1.8s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1400.0) && 🛡️DistanceToTarget(dist>=250.0) && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_Raven_Phase2)]
                    ├── ➡️ Sequence [🛡️Blackboard(BB2)]
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(BB2)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                    │   ├── 📋 Blackboard(FirstShot)
                    │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                    │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                    │   └── ⚔️ UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Target.P_Eve_Beta_SwordAura ON) && 🛡️DistanceToTarget(dist>=300.0)]
                    │   └── ⚔️ UseSkill(BetaEvadeLeft|BetaEvadeRight combo=TableCommand)
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Target.P_Eve_Beta_SwordAura2 ON) && 🛡️DistanceToTarget(dist>=300.0)]
                    │   └── ⚔️ UseSkill(BetaEvadeLeft|BetaEvadeRight combo=TableCommand)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Raven_RushWaitTime ON) && 🛡️CheckActorEffect(NOT Target.P_Eve_Beta_SwordAura2 ON)]
                    │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [RushWaitTimer1])]
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(SecondTime)]
                    │   └── ⚔️ UseSkill(ParryPreview1)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Raven_ParryPreviewChain1 ON) && 🛡️CheckActorEffect(NOT Self.M_Raven_ParryPreviewChain2 ON)]
                    │   └── ⚔️ UseSkill(ParryCounterSlash)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(SecondTime)]
                    │   └── ⚔️ UseSkill(EvadeBackSwordAura)
                    ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(EvadeBackRush|ParryPreview2)
                    ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Raven_ParryPreviewChain2 ON) && 🛡️CheckActorEffect(NOT Self.M_Raven_ParryPreviewChain1 ON)]
                    │   └── ⚔️ UseSkill(ParryCounterCombo)
                    ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=30.0%) && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(SlashChainCombo)
                    ├── ❓ Selector [🛡️DistanceToTarget(dist>=1000.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(SwordAuraCombo)
                    ├── ❓ Selector [🛡️DistanceToTarget(dist>=400.0) && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │   ├── ⚔️ UseSkill(ChaseGrab) [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(ChaseCombo)
                    ├── ➡️ Sequence [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(SecondTime)]
                    │   ├── ⚔️ UseSkill(BurstAreaSlash)
                    │   └── 📋 Blackboard(SecondTime)
                    ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(SecondTime)]
                    │   └── ⚔️ UseSkill(BurstSpinCombo|BurstAreaSlash)
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️Blackboard(SecondTime)]
                    │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Raven_BetaGrabCheck ON)]
                    │   │   ├── ➡️ Sequence
                    │   │   │   ├── ⚔️ UseSkill(BetaGrabChain)
                    │   │   │   └── ⚔️ UseSkill(BetaCounterGrab) [🛡️CheckActorEffect(Self.M_Raven_GrabChain ON)]
                    │   │   └── ⚔️ UseSkill(BetaGrab) [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(BetaRapidCombo|BetaChargeCombo) [🛡️CheckActorEffect(NOT Self.? ON)]
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   ├── ➡️ Sequence
                    │   │   ├── ⚔️ UseSkill(BackJumpCombo) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Raven_SwordAuraCheck ON)]
                    │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1400.0) && 🛡️DistanceToTarget(dist>=250.0) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(RapidMoveBack)
                    │       └── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1400.0) && 🛡️DistanceToTarget(dist>=250.0) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   ├── ⚔️ UseSkill(SlashCombo) [🛡️CheckActorEffect(NOT Self.M_Raven_ComboCheck ON)]
                    │   ├── ⚔️ UseSkill(BurstSpinCombo)
                    │   └── ⚔️ UseSkill(MoveCombo|MoveChainCombo) [🛡️CheckActorEffect(NOT Self.M_Raven_MoveComboCheck ON)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   └── ⚔️ UseSkill(Slash|SlashChain)
                    └── ❓ Selector
                        ├── ⚠️ CautionToTarget [🛡️TimeLimit(1.8s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1400.0) && 🛡️DistanceToTarget(dist>=250.0) && 🛡️CheckActorEffect(NOT Self.? ON) && 🛡️CheckActorEffect(NOT Target.P_Eve_Beta_SwordAura ON)]
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 68 |
| Task/UseSkill | 48 |
| Selector | 34 |
| Sequence | 23 |
| Dec/DistanceToTarget | 17 |
| Dec/Blackboard | 13 |
| Dec/UseableTime | 13 |
| Task/UseableTimeReset | 10 |
| Task/CautionToTarget | 8 |
| Dec/TimeLimit | 8 |
| Task/Blackboard | 7 |
| Dec/Random | 5 |
| Dec/CheckActorStat | 4 |
| Dec/IsAlive | 3 |
| Dec/AimMe | 3 |
| Task/Wait | 2 |
| Task/UseEffect | 2 |
| Dec/CheckStance | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Dec/AggroLevel | 1 |

## 스킬 목록
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(BetaEvadeLeft|BetaEvadeRight combo=TableCommand)
- UseSkill(BetaEvadeLeft|BetaEvadeRight combo=TableCommand)
- UseSkill(ParryPreview1)
- UseSkill(Parry)
- UseSkill(BetaGrabChain)
- UseSkill(BetaCounterGrab)
- UseSkill(BetaGrab)
- UseSkill(BetaRapidCombo|BetaChargeCombo)
- UseSkill(EvadeBackRush|ParryPreview2)
- UseSkill(ParryCounterCombo)
- UseSkill(ChaseGrab)
- UseSkill(ChaseCombo)
- UseSkill(BetaRapidCombo|BetaChargeCombo|BetaGrab)
- UseSkill(BetaGrabChain)
- UseSkill(BetaCounterGrab)
- UseSkill(BetaGrab)
- UseSkill(BetaRapidCombo|BetaChargeCombo)
- UseSkill(RapidMoveBack)
- UseSkill(MoveCombo|MoveChainCombo)
- UseSkill(Slash|SlashChain)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(EvadeLeft|EvadeRight combo=TableCommand)
- UseSkill(BetaEvadeLeft|BetaEvadeRight combo=TableCommand)
- UseSkill(BetaEvadeLeft|BetaEvadeRight combo=TableCommand)
- UseSkill(ParryPreview1)
- UseSkill(ParryCounterSlash)
- UseSkill(EvadeBackSwordAura)
- UseSkill(EvadeBackRush|ParryPreview2)
- UseSkill(ParryCounterCombo)
- UseSkill(SlashChainCombo)
- UseSkill(SwordAuraCombo)
- UseSkill(ChaseGrab)
- UseSkill(ChaseCombo)
- UseSkill(BurstAreaSlash)
- UseSkill(BurstSpinCombo|BurstAreaSlash)
- UseSkill(BetaGrabChain)
- UseSkill(BetaCounterGrab)
- UseSkill(BetaGrab)
- UseSkill(BetaRapidCombo|BetaChargeCombo)
- UseSkill(BackJumpCombo)
- UseSkill(RapidMoveBack)
- UseSkill(SlashCombo)
- UseSkill(BurstSpinCombo)
- UseSkill(MoveCombo|MoveChainCombo)
- UseSkill(Slash|SlashChain)
