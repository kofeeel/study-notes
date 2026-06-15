# M_GrubDash_AI

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
        │       ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
        │       │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
        │       │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
        │       └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
        │           ├── ➡️ Sequence [🛡️Blackboard(BB1)]
        │           │   ├── 🔄 UseableTimeReset
        │           │   ├── 🔄 UseableTimeReset
        │           │   └── 📋 Blackboard(BB1)
        │           ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
        │           │   ├── 📋 Blackboard(BattleStartSkill)
        │           │   └── ❓ Selector
        │           │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
        │           │       │   ├── 🚶 MoveToTarget
        │           │       │   └── ⚔️ UseSkill(ComboAttack_1)
        │           │       └── ❓ Selector
        │           │           └── ⚔️ UseSkill(RushAttack2_1)
        │           ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON)]
        │           │   ├── ⚔️ UseSkill(ShockWaveAttack_1) [🛡️CheckActorEffect(Self.M_Check_WLMonster)]
        │           │   ├── ⚔️ UseSkill(PowerfulHeadAttack_1) [🛡️CheckActorEffect(Self.M_Check_WLMonster)]
        │           │   └── ⚔️ UseSkill(KnockbackAttack1_1) [🛡️DistanceToTarget(dist>400.0)]
        │           ├── ⚔️ UseSkill(JumpStand_1)
        │           ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️DistanceToTarget(dist>300.0)]
        │           │   └── ⚔️ UseSkill(RushAttack2_1)
        │           ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.? ON)]
        │           │   ├── ⚔️ UseSkill(ComboAttack_1|HeadAttack_1)
        │           │   └── ❓ Selector
        │           │       ├── ⚔️ UseSkill(ShockWaveAttack_1) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON) && 🛡️CheckActorEffect(Self.M_Check_WLMonster)]
        │           │       ├── ⚔️ UseSkill(PowerfulHeadAttack_1) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON) && 🛡️CheckActorEffect(Self.M_Check_WLMonster)]
        │           │       └── ⚔️ UseSkill(JumpBack_1)
        │           ├── ⚔️ UseSkill(JumpStand_1)
        │           └── 🚶 MoveToTarget
        └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(Target.?)]
            ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │   ├── 🔄 UseableTimeReset
            │   ├── 🔄 UseableTimeReset
            │   └── 📋 Blackboard(BB1)
            ├── ➡️ Sequence [🛡️Blackboard(BattleStartSkill) && 🛡️TimeLimit(6.0s)]
            │   ├── 📋 Blackboard(BattleStartSkill)
            │   └── ❓ Selector
            │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
            │       │   ├── 🚶 MoveToTarget
            │       │   └── ⚔️ UseSkill(ComboAttack_1)
            │       └── ❓ Selector
            │           └── ⚔️ UseSkill(RushAttack2_1)
            ├── ❓ Selector [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON)]
            │   ├── ⚔️ UseSkill(ShockWaveAttack_1) [🛡️CheckActorEffect(Self.M_Check_WLMonster)]
            │   ├── ⚔️ UseSkill(PowerfulHeadAttack_1) [🛡️CheckActorEffect(Self.M_Check_WLMonster)]
            │   └── ⚔️ UseSkill(KnockbackAttack1_1) [🛡️DistanceToTarget(dist>400.0)]
            ├── ⚔️ UseSkill(JumpStand_1)
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON) && 🛡️DistanceToTarget(dist>300.0)]
            │   └── ⚔️ UseSkill(RushAttack2_1)
            ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Target.? ON)]
            │   ├── ⚔️ UseSkill(ComboAttack_1|HeadAttack_1)
            │   └── ❓ Selector
            │       ├── ⚔️ UseSkill(ShockWaveAttack_1) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON) && 🛡️CheckActorEffect(Self.M_Check_WLMonster)]
            │       ├── ⚔️ UseSkill(PowerfulHeadAttack_1) [🛡️UseableTime && 🛡️CheckActorEffect(NOT Self.M_GrubDash_NoGuardCheck ON) && 🛡️CheckActorEffect(Self.M_Check_WLMonster)]
            │       └── ⚔️ UseSkill(JumpBack_1)
            ├── ⚔️ UseSkill(JumpStand_1)
            └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 31 |
| Task/UseSkill | 24 |
| Selector | 20 |
| Sequence | 13 |
| Dec/AggroLevel | 7 |
| Dec/Blackboard | 6 |
| Task/Blackboard | 6 |
| Dec/UseableTime | 6 |
| Dec/DistanceToTarget | 5 |
| Task/MoveToTarget | 5 |
| Task/PlayShow | 4 |
| Task/UseableTimeReset | 4 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Dec/TimeLimit | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/DetectResult | 2 |
| Task/WaitTimeRandom | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Dec/Random | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/CautionToTarget | 1 |

## 스킬 목록
- UseSkill(ComboAttack_1)
- UseSkill(RushAttack2_1)
- UseSkill(ShockWaveAttack_1)
- UseSkill(PowerfulHeadAttack_1)
- UseSkill(KnockbackAttack1_1)
- UseSkill(JumpStand_1)
- UseSkill(RushAttack2_1)
- UseSkill(ComboAttack_1|HeadAttack_1)
- UseSkill(ShockWaveAttack_1)
- UseSkill(PowerfulHeadAttack_1)
- UseSkill(JumpBack_1)
- UseSkill(JumpStand_1)
- UseSkill(ComboAttack_1)
- UseSkill(RushAttack2_1)
- UseSkill(ShockWaveAttack_1)
- UseSkill(PowerfulHeadAttack_1)
- UseSkill(KnockbackAttack1_1)
- UseSkill(JumpStand_1)
- UseSkill(RushAttack2_1)
- UseSkill(ComboAttack_1|HeadAttack_1)
- UseSkill(ShockWaveAttack_1)
- UseSkill(PowerfulHeadAttack_1)
- UseSkill(JumpBack_1)
- UseSkill(JumpStand_1)
