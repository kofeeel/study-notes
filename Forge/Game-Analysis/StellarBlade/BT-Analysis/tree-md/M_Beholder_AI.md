# M_Beholder_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ❓ Selector
        │   ├── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackPC ON)]
        │   │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        │   └── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackTachyNPC ON)]
        │       └── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🏠 MoveToHome [🛡️CheckActorEffect(Self.Check_AttackPC ON) && 🛡️DetectResult(==)]
        │   ├── 🏠 MoveToHome [🛡️CheckActorEffect(Self.Check_AttackTachyNPC ON) && 🛡️DetectResult(==)]
        │   └── 🏠 MoveToHome [🛡️CheckActorEffect(Self.Check_AttackTachyNPC) && 🛡️DetectResult(==)]
        ├── 👁️ DetectTarget [🛡️CheckActorEffect(Self.Check_AttackTachyNPC) && 🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── ⚔️ UseSkill(BattleStart) [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(5.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s)]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackPC ON)]
            │       │   ├── ⚔️ UseSkill(SwingDoubleGuide)
            │       │   └── ⚔️ UseSkill(SwingGuide|SpinSwingGuide)
            │       ├── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackPC)]
            │       │   ├── ⚔️ UseSkill(SwingDouble)
            │       │   └── ⚔️ UseSkill(Swing|SpinSwing)
            │       └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Target.Check_LinkWait ON) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackPC ON)]
                │   ├── ⚔️ UseSkill(SwingDoubleGuide)
                │   └── ⚔️ UseSkill(SwingGuide|SpinSwingGuide)
                ├── ❓ Selector [🛡️CheckActorEffect(Self.Check_AttackPC)]
                │   ├── ⚔️ UseSkill(SwingDouble)
                │   └── ⚔️ UseSkill(Swing|SpinSwing)
                └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 23 |
| Selector | 19 |
| Task/UseSkill | 9 |
| Dec/AggroLevel | 7 |
| Dec/DetectResult | 6 |
| Dec/IsAlive | 3 |
| Sequence | 3 |
| Task/DetectTarget | 3 |
| Task/MoveToHome | 3 |
| Task/CautionToTarget | 3 |
| Task/MoveToTarget | 3 |
| Dec/IsActiveSkill | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Task/Wait | 2 |
| Dec/TimeLimit | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/PlayShow | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Task/WaitTimeRandom | 1 |

## 스킬 목록
- UseSkill(BattleStart)
- UseSkill(SwingDoubleGuide)
- UseSkill(SwingGuide|SpinSwingGuide)
- UseSkill(SwingDouble)
- UseSkill(Swing|SpinSwing)
- UseSkill(SwingDoubleGuide)
- UseSkill(SwingGuide|SpinSwingGuide)
- UseSkill(SwingDouble)
- UseSkill(Swing|SpinSwing)
