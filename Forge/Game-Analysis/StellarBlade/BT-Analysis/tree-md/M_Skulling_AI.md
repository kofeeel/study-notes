# M_Skulling_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Skulling_CheckAll ON)]
        │   ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        │   ├── ✨ UseEffect(['KeepDetectTarget_10s']) [🛡️IsAlive(SubTarget)]
        │   └── ❓ Selector [🛡️IsAlive(SubTarget)]
        │       ├── ➡️ Sequence [🛡️Blackboard(OnceCheck)]
        │       │   ├── 📋 Blackboard(OnceCheck)
        │       │   ├── ⏳ WaitTimeRandom
        │       │   └── ⚔️ UseSkill(Move combo=TableCommand subTarget) [🛡️Random(rand(100)<=80) && 🛡️DistanceToTarget(dist>=300.0)]
        │       ├── 🚶 MoveToTarget
        │       ├── ⚔️ UseSkill(CombinationToSword1 combo=TableCommand subTarget) [🛡️CheckActorEffect(SubTarget.M_SkullSwordBody_CombiReady ON)]
        │       ├── ⚔️ UseSkill(CombinationToSword2 combo=TableCommand subTarget) [🛡️CheckActorEffect(SubTarget.M_SkullSwordBody_CombiReady ON)]
        │       ├── ⚔️ UseSkill(CombinationToSpear1 combo=TableCommand subTarget) [🛡️CheckActorEffect(SubTarget.M_SkullSpearBody_CombiReady ON)]
        │       ├── ⚔️ UseSkill(CombinationToSpear2 combo=TableCommand subTarget) [🛡️CheckActorEffect(SubTarget.M_SkullSpearBody_CombiReady ON)]
        │       ├── ⚔️ UseSkill(CombinationToGunner1 combo=TableCommand subTarget) [🛡️CheckActorEffect(SubTarget.M_SkullGunnerBody_CombiReady ON)]
        │       └── ⚔️ UseSkill(CombinationToGunner2 combo=TableCommand subTarget) [🛡️CheckActorEffect(SubTarget.M_SkullGunnerBody_CombiReady ON)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ❓ Selector
        │   ├── ⏳ WaitTimeRandom [🛡️CheckActorEffect(Self.M_Skulling_CombinationEffectDispel) && 🛡️DetectResult(==)]
        │   └── ➡️ Sequence
        │       └── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ➡️ Sequence
            ├── 👁️ DetectTarget [🛡️DetectResult(==)]
            └── ❓ Selector [🛡️IsAlive(Target)]
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Skulling_Summoned_CheckEffect1 ON)]
                │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
                │       └── 📋 Blackboard(BattleState)
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Skulling_CombinationEffectDispel ON)]
                │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
                │       ├── 📋 Blackboard(BattleState)
                │       └── ⏳ WaitTimeRandom
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Self.M_Skulling_CheckAll ON) && 🛡️CheckActorEffect(NOT Self.BattleMode ON)]
                │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
                │       ├── 📋 Blackboard(BattleState)
                │       └── 🎬 PlayShow
                ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
                ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
                │   └── ⏳ WaitTimeRandom
                └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckTarget && 🛡️CheckActorEffect(NOT Target.? ON)]
                    ├── ➡️ Sequence
                    │   ├── ⏳ WaitTimeRandom
                    │   ├── ⚔️ UseSkill(MeleeAttack combo=TableCommand)
                    │   └── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
                    ├── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
                    └── ➡️ Sequence
                        ├── 🚶 MoveToTarget
                        └── ⏳ Wait(0.7s)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 18 |
| Sequence | 12 |
| Selector | 11 |
| Task/UseSkill | 8 |
| Dec/AggroLevel | 7 |
| Dec/IsAlive | 5 |
| Dec/DetectResult | 5 |
| Dec/Blackboard | 5 |
| Task/Blackboard | 5 |
| Task/WaitTimeRandom | 5 |
| Task/Wait | 5 |
| Task/DetectTarget | 3 |
| Dec/Random | 3 |
| Task/MoveToTarget | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/UseEffect | 1 |
| Dec/DistanceToTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckTarget | 1 |

## 스킬 목록
- UseSkill(Move combo=TableCommand subTarget)
- UseSkill(CombinationToSword1 combo=TableCommand subTarget)
- UseSkill(CombinationToSword2 combo=TableCommand subTarget)
- UseSkill(CombinationToSpear1 combo=TableCommand subTarget)
- UseSkill(CombinationToSpear2 combo=TableCommand subTarget)
- UseSkill(CombinationToGunner1 combo=TableCommand subTarget)
- UseSkill(CombinationToGunner2 combo=TableCommand subTarget)
- UseSkill(MeleeAttack combo=TableCommand)
