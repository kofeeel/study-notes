# M_DollHead_AI

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
            │   └── ⏳ WaitTimeRandom
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   └── ⏳ WaitTimeRandom
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Marionette_SoundBomb_SingnalSuicide ON)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(ChaseBomb)
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Marionette_SoundBomb2_SingnalLaser ON) && 🛡️CheckActorEffect(NOT Self.M_Marionette_SoundBomb_SingnalSuicide ON)]
                │   ├── ⚔️ UseSkill(Laser)
                │   └── ⏳ WaitTimeRandom
                ├── ➡️ Sequence [🛡️CheckActorStat(ActorStatType_HP<=99.0%) && 🛡️CheckActorEffect(NOT Self.M_Marionette_SoundBomb2_SingnalLaser ON)]
                │   └── ⚔️ UseSkill(ChaseBomb)
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(JumpAttack2)
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON) && 🛡️DistanceToTarget(dist>=400.0)]
                │   ├── ⏳ WaitTimeRandom
                │   └── ⚔️ UseSkill(Chase2)
                ├── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON)]
                │   ├── ⏳ WaitTimeRandom
                │   ├── ⚔️ UseSkill(JumpAttack)
                │   └── ⏳ Wait(0.5s) [🛡️Random(rand(100)<=70)]
                └── ➡️ Sequence [🛡️CheckActorEffect(NOT Self.? ON)]
                    ├── 🚶 MoveToTarget
                    └── ⏳ WaitTimeRandom
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 16 |
| Sequence | 11 |
| Selector | 8 |
| Task/WaitTimeRandom | 8 |
| Task/UseSkill | 6 |
| Dec/AggroLevel | 5 |
| Dec/IsAlive | 3 |
| Task/Wait | 3 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Dec/DetectResult | 2 |
| Task/MoveToHome | 1 |
| Task/DetectTarget | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |
| Dec/CheckActorStat | 1 |
| Dec/DistanceToTarget | 1 |
| Dec/Random | 1 |
| Task/MoveToTarget | 1 |

## 스킬 목록
- UseSkill(ChaseBomb)
- UseSkill(Laser)
- UseSkill(ChaseBomb)
- UseSkill(JumpAttack2)
- UseSkill(Chase2)
- UseSkill(JumpAttack)
