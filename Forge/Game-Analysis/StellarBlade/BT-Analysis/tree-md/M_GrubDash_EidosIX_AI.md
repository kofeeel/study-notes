# M_GrubDash_EidosIX_AI

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
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   │   └── ➡️ Sequence [🛡️DistanceToTarget(dist>100.0) && 🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
        │   │       ├── 📋 Blackboard(BattleStart)
        │   │       ├── 🎬 PlayShow
        │   │       └── ⏳ Wait(1.0s)
        │   ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
        │   ├── ➡️ Sequence [🛡️CheckActorEffect(Target.? ON)]
        │   │   ├── 🎬 PlayShow
        │   │   └── ⏳ WaitTimeRandom
        │   ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
        │   │   ├── 🎬 PlayShow [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(2.0s [Timer_LinkWait])]
        │   │   └── ⏳ WaitTimeRandom
        │   └── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
        └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(Target.?)]
            ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │   ├── 🔄 UseableTimeReset
            │   ├── 🔄 UseableTimeReset
            │   └── 📋 Blackboard(BB1)
            ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │   ├── 📋 Blackboard(BattleStartSkill)
            │   └── ❓ Selector
            │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
            │       │   ├── 🚶 MoveToTarget
            │       │   └── ⚔️ UseSkill(ComboAttack_1_DEDA)
            │       └── ❓ Selector
            │           └── ⚔️ UseSkill(RushAttack2_1_DEDA)
            ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON)]
            │   ├── ⚔️ UseSkill(ShockWaveAttack_1)
            │   ├── ⚔️ UseSkill(PowerfulHeadAttack_1)
            │   └── ⚔️ UseSkill(KnockbackAttack1_1) [🛡️DistanceToTarget(dist>400.0)]
            ├── ⚔️ UseSkill(JumpStand_1)
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️DistanceToTarget(dist>300.0)]
            │   └── ⚔️ UseSkill(RushAttack2_1_DEDA)
            ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.? ON)]
            │   ├── ⚔️ UseSkill(ComboAttack_1_DEDA|HeadAttack_1_DEDA)
            │   └── ❓ Selector
            │       ├── ⚔️ UseSkill(ShockWaveAttack_1) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON)]
            │       ├── ⚔️ UseSkill(PowerfulHeadAttack_1) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON)]
            │       └── ⚔️ UseSkill(JumpBack_1)
            ├── ⚔️ UseSkill(JumpStand_1)
            └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 14 |
| Selector | 13 |
| Task/UseSkill | 12 |
| Sequence | 9 |
| Dec/AggroLevel | 6 |
| Task/PlayShow | 4 |
| Dec/Blackboard | 4 |
| Task/Blackboard | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Dec/DistanceToTarget | 3 |
| Dec/UseableTime | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/TimeLimit | 2 |
| Task/UseableTimeReset | 2 |
| Task/MoveToTarget | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/IsGroupTarget | 1 |
| Dec/Random | 1 |

## 스킬 목록
- UseSkill(ComboAttack_1_DEDA)
- UseSkill(RushAttack2_1_DEDA)
- UseSkill(ShockWaveAttack_1)
- UseSkill(PowerfulHeadAttack_1)
- UseSkill(KnockbackAttack1_1)
- UseSkill(JumpStand_1)
- UseSkill(RushAttack2_1_DEDA)
- UseSkill(ComboAttack_1_DEDA|HeadAttack_1_DEDA)
- UseSkill(ShockWaveAttack_1)
- UseSkill(PowerfulHeadAttack_1)
- UseSkill(JumpBack_1)
- UseSkill(JumpStand_1)
