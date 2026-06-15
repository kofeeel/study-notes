# M_VipGuard_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    ├── ❓ Selector [🛡️CheckActorEffect(Self.LV_Check_Roaming ON)]
    │   └── ➡️ Sequence
    │       └── 🏠 MoveToHome
    └── ❓ Selector [🛡️CheckActorEffect(NOT Self.LV_Check_Roaming ON)]
        └── ❓ Selector [🛡️IsAlive(Self)]
            ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
            │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
            │   └── 📋 Blackboard(BattleStart)
            ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction)]
            │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
            ├── 👁️ DetectTarget [🛡️DetectResult(==)]
            ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
            │   └── 🧠 MetaAI
            ├── ❓ Selector [🛡️IsAlive(Target)]
            │   └── ⏳ Wait(1.0s)
            └── ❓ Selector [🛡️IsAlive(Target)]
                ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
                │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
                ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
                │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
                │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️Random(rand(100)<=75)]
                │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
                │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=25)]
                │       │   ├── ⚔️ UseSkill(LightAttack_01)
                │       │   ├── ⚔️ UseSkill(LightAttack_02)
                │       │   ├── ⚔️ UseSkill(LightAttack_03)
                │       │   ├── ⚔️ UseSkill(FinishAttack_03)
                │       │   └── ⚔️ UseSkill(FinishAttack_04)
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=30)]
                │       │   ├── ⚔️ UseSkill(LightAttack_01)
                │       │   ├── ⚔️ UseSkill(FinishAttack_01)
                │       │   ├── ⚔️ UseSkill(StrongAttack_09)
                │       │   └── ⚔️ UseSkill(StrongAttack_10)
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=45)]
                │       │   ├── ⚔️ UseSkill(LightAttack_01)
                │       │   ├── ⚔️ UseSkill(LightAttack_02)
                │       │   └── ⚔️ UseSkill(FinishAttack_02)
                │       ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                │       │   ├── ⚔️ UseSkill(StrongAttack_01)
                │       │   └── ⚔️ UseSkill(StrongAttack_02)
                │       ├── ➡️ Sequence
                │       │   ├── ⚔️ UseSkill(LightAttack_01)
                │       │   ├── ⚔️ UseSkill(LightAttack_02)
                │       │   ├── ⚔️ UseSkill(LightAttack_03)
                │       │   └── ⚔️ UseSkill(LightAttack_04)
                │       └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence [🛡️Random(rand(100)<=25)]
                    │   ├── ⚔️ UseSkill(LightAttack_01)
                    │   ├── ⚔️ UseSkill(LightAttack_02)
                    │   ├── ⚔️ UseSkill(LightAttack_03)
                    │   ├── ⚔️ UseSkill(FinishAttack_03)
                    │   └── ⚔️ UseSkill(FinishAttack_04)
                    ├── ➡️ Sequence [🛡️Random(rand(100)<=30)]
                    │   ├── ⚔️ UseSkill(LightAttack_01)
                    │   ├── ⚔️ UseSkill(FinishAttack_01)
                    │   ├── ⚔️ UseSkill(StrongAttack_09)
                    │   └── ⚔️ UseSkill(StrongAttack_10)
                    ├── ➡️ Sequence [🛡️Random(rand(100)<=45)]
                    │   ├── ⚔️ UseSkill(LightAttack_01)
                    │   ├── ⚔️ UseSkill(LightAttack_02)
                    │   └── ⚔️ UseSkill(FinishAttack_02)
                    ├── ➡️ Sequence [🛡️Random(rand(100)<=50)]
                    │   ├── ⚔️ UseSkill(StrongAttack_01)
                    │   └── ⚔️ UseSkill(StrongAttack_02)
                    ├── ➡️ Sequence
                    │   ├── ⚔️ UseSkill(LightAttack_01)
                    │   ├── ⚔️ UseSkill(LightAttack_02)
                    │   ├── ⚔️ UseSkill(LightAttack_03)
                    │   └── ⚔️ UseSkill(LightAttack_04)
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 36 |
| Dec/CheckActorEffect | 14 |
| Sequence | 14 |
| Selector | 12 |
| Dec/Random | 9 |
| Dec/AggroLevel | 6 |
| Task/CautionToTarget | 4 |
| Dec/IsAlive | 3 |
| Task/MoveToHome | 2 |
| Dec/DetectResult | 2 |
| Task/Wait | 2 |
| Dec/TimeLimit | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToTarget | 2 |
| Dec/IsActiveSkill | 1 |
| Task/PlayShow | 1 |
| Dec/Blackboard | 1 |
| Task/Blackboard | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(LightAttack_01)
- UseSkill(LightAttack_02)
- UseSkill(LightAttack_03)
- UseSkill(FinishAttack_03)
- UseSkill(FinishAttack_04)
- UseSkill(LightAttack_01)
- UseSkill(FinishAttack_01)
- UseSkill(StrongAttack_09)
- UseSkill(StrongAttack_10)
- UseSkill(LightAttack_01)
- UseSkill(LightAttack_02)
- UseSkill(FinishAttack_02)
- UseSkill(StrongAttack_01)
- UseSkill(StrongAttack_02)
- UseSkill(LightAttack_01)
- UseSkill(LightAttack_02)
- UseSkill(LightAttack_03)
- UseSkill(LightAttack_04)
- UseSkill(LightAttack_01)
- UseSkill(LightAttack_02)
- UseSkill(LightAttack_03)
- UseSkill(FinishAttack_03)
- UseSkill(FinishAttack_04)
- UseSkill(LightAttack_01)
- UseSkill(FinishAttack_01)
- UseSkill(StrongAttack_09)
- UseSkill(StrongAttack_10)
- UseSkill(LightAttack_01)
- UseSkill(LightAttack_02)
- UseSkill(FinishAttack_02)
- UseSkill(StrongAttack_01)
- UseSkill(StrongAttack_02)
- UseSkill(LightAttack_01)
- UseSkill(LightAttack_02)
- UseSkill(LightAttack_03)
- UseSkill(LightAttack_04)
