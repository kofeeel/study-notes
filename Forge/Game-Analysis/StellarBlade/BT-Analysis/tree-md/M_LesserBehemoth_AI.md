# M_LesserBehemoth_AI

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
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── 🎬 PlayShow [🛡️TimeLimit(3.066s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── 🎬 PlayShow [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.066s [AbnormalTimer])]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   └── 🎬 PlayShow [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   ├── 🔄 UseableTimeReset
            │       │   └── 📋 Blackboard(BB1)
            │       ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │       │   ├── 📋 Blackboard(BattleStartSkill)
            │       │   └── ❓ Selector
            │       │       ├── ❓ Selector [🛡️Random(rand(100)<=20) && 🛡️DistanceToTarget(dist>=600.0)]
            │       │       │   └── ⚔️ UseSkill(JumpAttackLow)
            │       │       ├── ➡️ Sequence [🛡️Random(rand(100)<=20) && 🛡️DistanceToTarget(dist>=600.0)]
            │       │       │   ├── 🚶 MoveToTarget
            │       │       │   └── ⚔️ UseSkill(JumpAttackLow)
            │       │       └── ➡️ Sequence
            │       │           ├── 🚶 MoveToTarget
            │       │           └── ⚔️ UseSkill(ScratchLeft|ScratchRight)
            │       ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️UseableTime]
            │       │   └── ⚔️ UseSkill(JumpAttackLow)
            │       ├── ❓ Selector
            │       │   ├── ⚔️ UseSkill(SpinAttackLeft|SpinAttackRight)
            │       │   ├── ⚔️ UseSkill(TurnAttackLeft|TurnAttackRight)
            │       │   └── ⚔️ UseSkill(SideStepLeft|SideStepRight) [🛡️DistanceToTarget(dist>=300.0)]
            │       ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_LesserBehemoth_NoGuardCheck ON)]
            │       │   └── ⚔️ UseSkill(RoarShort|ChargeSmash)
            │       ├── ❓ Selector [🛡️UseableTime]
            │       │   └── ⚔️ UseSkill(ScratchCombo1)
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(ScratchLeft|ScratchRight)
            │       │   └── ⏳ WaitTimeRandom
            │       └── ❓ Selector
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️Blackboard(BB1)]
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   ├── 🔄 UseableTimeReset
                │   └── 📋 Blackboard(BB1)
                ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
                │   ├── 📋 Blackboard(BattleStartSkill)
                │   └── ❓ Selector
                │       ├── ❓ Selector [🛡️Random(rand(100)<=20) && 🛡️DistanceToTarget(dist>=600.0)]
                │       │   └── ⚔️ UseSkill(JumpAttackLow)
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=20) && 🛡️DistanceToTarget(dist>=600.0)]
                │       │   ├── 🚶 MoveToTarget
                │       │   └── ⚔️ UseSkill(JumpAttackLow)
                │       └── ➡️ Sequence
                │           ├── 🚶 MoveToTarget
                │           └── ⚔️ UseSkill(ScratchLeft|ScratchRight)
                ├── ❓ Selector [🛡️DistanceToTarget(dist>=600.0) && 🛡️UseableTime]
                │   └── ⚔️ UseSkill(JumpAttackLow)
                ├── ❓ Selector
                │   ├── ⚔️ UseSkill(SpinAttackLeft|SpinAttackRight)
                │   ├── ⚔️ UseSkill(TurnAttackLeft|TurnAttackRight)
                │   └── ⚔️ UseSkill(SideStepLeft|SideStepRight) [🛡️DistanceToTarget(dist>=300.0)]
                ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_LesserBehemoth_NoGuardCheck ON)]
                │   └── ⚔️ UseSkill(RoarShort|ChargeSmash)
                ├── ❓ Selector [🛡️UseableTime]
                │   └── ⚔️ UseSkill(ScratchCombo1)
                ├── ➡️ Sequence
                │   ├── ⚔️ UseSkill(ScratchLeft|ScratchRight)
                │   └── ⏳ WaitTimeRandom
                └── ❓ Selector
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Selector | 25 |
| Task/UseSkill | 20 |
| Sequence | 14 |
| Dec/CheckActorEffect | 14 |
| Dec/DistanceToTarget | 8 |
| Dec/AggroLevel | 7 |
| Dec/Blackboard | 6 |
| Task/Blackboard | 6 |
| Task/UseableTimeReset | 6 |
| Task/MoveToTarget | 6 |
| Dec/UseableTime | 6 |
| Task/PlayShow | 5 |
| Task/WaitTimeRandom | 4 |
| Dec/TimeLimit | 4 |
| Dec/Random | 4 |
| Dec/IsAlive | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(JumpAttackLow)
- UseSkill(JumpAttackLow)
- UseSkill(ScratchLeft|ScratchRight)
- UseSkill(JumpAttackLow)
- UseSkill(SpinAttackLeft|SpinAttackRight)
- UseSkill(TurnAttackLeft|TurnAttackRight)
- UseSkill(SideStepLeft|SideStepRight)
- UseSkill(RoarShort|ChargeSmash)
- UseSkill(ScratchCombo1)
- UseSkill(ScratchLeft|ScratchRight)
- UseSkill(JumpAttackLow)
- UseSkill(JumpAttackLow)
- UseSkill(ScratchLeft|ScratchRight)
- UseSkill(JumpAttackLow)
- UseSkill(SpinAttackLeft|SpinAttackRight)
- UseSkill(TurnAttackLeft|TurnAttackRight)
- UseSkill(SideStepLeft|SideStepRight)
- UseSkill(RoarShort|ChargeSmash)
- UseSkill(ScratchCombo1)
- UseSkill(ScratchLeft|ScratchRight)
