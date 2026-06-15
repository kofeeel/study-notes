# M_Antlion_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ❓ Selector
        │   ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Antlion_MonsterSpawnEvent) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent) && 🛡️CheckStance(NOT M_Antlion_Rolling)]
        │   │   ├── ⏳ WaitTimeRandom [🛡️DetectResult(==)]
        │   │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        │   └── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   ├── ➡️ Sequence [🛡️Blackboard(BattleState) && 🛡️CheckStance(M_Antlion_Rolling)]
            │   │   ├── 📋 Blackboard(BattleState)
            │   │   └── ⚔️ UseSkill(RollingAttack combo=TableCommand)
            │   └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │       ├── 📋 Blackboard(BattleState)
            │       └── 🎬 PlayShow
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(4.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.M_WallWalk ON)]
            │   ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️IsGroupTarget && 🛡️CheckStance(NOT M_Antlion_Rolling) && 🛡️CheckActorEffect(Self.M_Common_CheckWave ON)]
            │   │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │   │       ├── ➡️ Sequence
            │   │       │   ├── ⏳ WaitTimeRandom
            │   │       │   ├── ⚔️ UseSkill(PawAttack_1 combo=TableCommand)
            │   │       │   └── ⏳ WaitTimeRandom
            │   │       ├── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
            │   │       └── ➡️ Sequence
            │   │           └── 🚶 MoveToTarget
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ➡️ Sequence
            │       │   ├── ⚔️ UseSkill(PawAttack_1 combo=TableCommand)
            │       │   └── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
            │       ├── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
            │       └── ➡️ Sequence
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️CheckActorEffect(Self.M_WallWalk ON)]
                └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckTarget]
                    ├── ➡️ Sequence [🛡️IsRunSpiderNav(NOT)]
                    │   └── ✨ UseEffect(['M_WallWalk_Dispel']) [🛡️CheckActorEffect(Self.M_WallWalk ON)]
                    ├── ➡️ Sequence [🛡️IsRunSpiderNav]
                    │   └── 🚶 SpiderMoveToTarget
                    ├── ➡️ Sequence [🛡️IsRunSpiderNav]
                    │   ├── ⚡ SpiderMoveStop
                    │   └── ✨ UseEffect(['M_WallWalk_Dispel']) [🛡️CheckActorEffect(Self.M_WallWalk ON)]
                    └── ⏳ Wait(1.0s)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 18 |
| Selector | 15 |
| Sequence | 13 |
| Dec/AggroLevel | 8 |
| Task/Wait | 6 |
| Dec/DetectResult | 4 |
| Task/WaitTimeRandom | 4 |
| Dec/IsAlive | 3 |
| Dec/Blackboard | 3 |
| Task/Blackboard | 3 |
| Dec/CheckStance | 3 |
| Task/UseSkill | 3 |
| Dec/Random | 3 |
| Dec/IsRunSpiderNav | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/DetectTarget | 2 |
| Task/CautionToTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/MoveToTarget | 2 |
| Task/UseEffect | 2 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/TimeLimit | 1 |
| Dec/IsGroupTarget | 1 |
| Dec/CheckTarget | 1 |
| Task/SpiderMoveToTarget | 1 |
| Task/SpiderMoveStop | 1 |

## 스킬 목록
- UseSkill(RollingAttack combo=TableCommand)
- UseSkill(PawAttack_1 combo=TableCommand)
- UseSkill(PawAttack_1 combo=TableCommand)
