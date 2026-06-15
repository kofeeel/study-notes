# M_Behemoth_AI

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
            │   ├── 🎬 PlayShow [🛡️TimeLimit(3.066s [AbnormalTimer])]
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ❓ Selector
                │   └── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_Behemoth_Default)]
                │       └── ⚔️ UseSkill(RoarPhaseChange)
                ├── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP>60.0%) && 🛡️CheckStance(M_Behemoth_Default) && 🛡️CheckActorEffect(NOT Target.? ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   ├── 🔄 UseableTimeReset
                │   │   └── 📋 Blackboard(BB1)
                │   ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   │   ├── 📋 Blackboard(FirstShot)
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                │   ├── ❓ Selector [🛡️UseableTime]
                │   │   └── ⚔️ UseSkill(SmashBackflip)
                │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=800.0) && 🛡️UseableTime]
                │   │   └── ⚔️ UseSkill(ShotSingle)
                │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Behemoth_NoGuardCheck ON)]
                │   │   └── ⚔️ UseSkill(JumpAttackHigh)
                │   ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️UseableTime]
                │   │   └── ⚔️ UseSkill(JumpAttackLow)
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(RushTurnSmash)
                │   │   ├── ⚔️ UseSkill(SpinAttackLeft|SpinAttackRight)
                │   │   ├── ⚔️ UseSkill(TurnAttackLeft|TurnAttackRight)
                │   │   └── ⚔️ UseSkill(SideStepLeft|SideStepRight) [🛡️DistanceToTarget(dist>=300.0)]
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(HeadButtCombo2)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(Roar1)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                │   │   └── ⚔️ UseSkill(HeadButtCombo1)
                │   ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_Behemoth_ComboCoolTime ON)]
                │   │   └── ⚔️ UseSkill(SmashCombo1|ScratchCombo1)
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(ScratchLeft|ScratchRight)
                │   │   └── ❓ Selector
                │   │       ├── ⚔️ UseSkill(HeadButtCombo1) [🛡️CheckActorEffect(NOT Self.M_Behemoth_HeadButtComboCoolTime ON)]
                │   │       └── ⏳ WaitTimeRandom
                │   └── ❓ Selector
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckActorStat(ActorStatType_HP<=60.0%) && 🛡️CheckStance(M_Behemoth_Phase2) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence [🛡️Blackboard(BB2)]
                    │   ├── 🔄 UseableTimeReset
                    │   ├── 🔄 UseableTimeReset
                    │   └── 📋 Blackboard(BB2)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                    │   ├── 📋 Blackboard(FirstShot)
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                    ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                    │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                    ├── ❓ Selector
                    │   └── ⚔️ UseSkill(Parry)
                    ├── ❓ Selector [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(NOT Self.M_Behemoth_ProjectileCoolTime ON)]
                    │   └── ⚔️ UseSkill(ShotMulti|ShotMulti2)
                    ├── ❓ Selector [🛡️UseableTime]
                    │   └── ⚔️ UseSkill(SmashBackflip)
                    ├── ❓ Selector [🛡️DistanceToTarget(dist>=800.0) && 🛡️CheckActorEffect(NOT Self.M_Behemoth_ProjectileCoolTime ON)]
                    │   └── ⚔️ UseSkill(ShotSingle2)
                    ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️CheckActorEffect(NOT Self.M_Behemoth_NoGuardCheck ON)]
                    │   └── ⚔️ UseSkill(JumpAttackHigh)
                    ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0)]
                    │   └── ⚔️ UseSkill(JumpAttackLow)
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(RushTurnSmash)
                    │   ├── ⚔️ UseSkill(SpinAttackLeft|SpinAttackLeft2|SpinAttackRight|SpinAttackRight2)
                    │   ├── ⚔️ UseSkill(TurnAttackLeft|TurnAttackRight)
                    │   └── ⚔️ UseSkill(SideStepLeft|SideStepRight) [🛡️DistanceToTarget(dist>=300.0)]
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(Parry)
                    │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   │   └── ⚔️ UseSkill(HeadButtCombo2)
                    │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   │   └── ⚔️ UseSkill(SmashCombo2)
                    │   └── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.? ON)]
                    │       └── ⚔️ UseSkill(Roar2)
                    ├── ❓ Selector
                    │   ├── ⚔️ UseSkill(Parry)
                    │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.? ON)]
                    │   │   └── ⚔️ UseSkill(HeadButtCombo1)
                    │   ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_Behemoth_ComboCoolTime ON)]
                    │   │   └── ⚔️ UseSkill(ScratchCombo2)
                    │   └── ➡️ Sequence
                    │       ├── ⚔️ UseSkill(ScratchLeft|ScratchRight)
                    │       └── ⏳ WaitTimeRandom
                    └── ❓ Selector
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 38 |
| Selector | 35 |
| Dec/CheckActorEffect | 20 |
| Sequence | 10 |
| Dec/UseableTime | 10 |
| Dec/DistanceToTarget | 9 |
| Task/UseableTimeReset | 6 |
| Dec/Random | 5 |
| Dec/CheckActorStat | 4 |
| Dec/Blackboard | 4 |
| Task/Blackboard | 4 |
| Dec/IsAlive | 3 |
| Task/WaitTimeRandom | 3 |
| Dec/CheckStance | 3 |
| Dec/AimMe | 3 |
| Task/Wait | 2 |
| Task/MoveToTarget | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/PlayShow | 1 |
| Dec/TimeLimit | 1 |
| Dec/AggroLevel | 1 |

## 스킬 목록
- UseSkill(RoarPhaseChange)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(SmashBackflip)
- UseSkill(ShotSingle)
- UseSkill(JumpAttackHigh)
- UseSkill(JumpAttackLow)
- UseSkill(RushTurnSmash)
- UseSkill(SpinAttackLeft|SpinAttackRight)
- UseSkill(TurnAttackLeft|TurnAttackRight)
- UseSkill(SideStepLeft|SideStepRight)
- UseSkill(HeadButtCombo2)
- UseSkill(Roar1)
- UseSkill(HeadButtCombo1)
- UseSkill(SmashCombo1|ScratchCombo1)
- UseSkill(ScratchLeft|ScratchRight)
- UseSkill(HeadButtCombo1)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(Parry)
- UseSkill(ShotMulti|ShotMulti2)
- UseSkill(SmashBackflip)
- UseSkill(ShotSingle2)
- UseSkill(JumpAttackHigh)
- UseSkill(JumpAttackLow)
- UseSkill(RushTurnSmash)
- UseSkill(SpinAttackLeft|SpinAttackLeft2|SpinAttackRight|SpinAttackRight2)
- UseSkill(TurnAttackLeft|TurnAttackRight)
- UseSkill(SideStepLeft|SideStepRight)
- UseSkill(Parry)
- UseSkill(HeadButtCombo2)
- UseSkill(SmashCombo2)
- UseSkill(Roar2)
- UseSkill(Parry)
- UseSkill(HeadButtCombo1)
- UseSkill(ScratchCombo2)
- UseSkill(ScratchLeft|ScratchRight)
