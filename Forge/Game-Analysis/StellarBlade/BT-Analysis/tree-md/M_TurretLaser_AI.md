# M_TurretLaser_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleState)]
        │   └── 📋 Blackboard(BattleState)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent) && 🛡️CheckActorEffect(Self.M_TurretLaser_TypeFastA)]
            │       └── ➡️ Sequence [🛡️Blackboard(BattleState)]
            │           ├── 📋 Blackboard(BattleState)
            │           ├── 🎬 PlayShow
            │           └── ✨ UseEffect(['BattleMode'])
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_CheckStoryMode)]
            │   ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_TurretLaser_TypeA ON)]
            │   │   └── ⚔️ UseSkill(Laser)
            │   ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_TurretLaser_TypeFastA ON) && 🛡️CheckActorEffect(Self.M_TurretLaser_Pause)]
            │   │   └── ⚔️ UseSkill(LaserFast)
            │   ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_TurretLaser_TypeB ON)]
            │   │   └── ⚔️ UseSkill(Laser|Laser1|Laser2)
            │   └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_TurretLaser_TypeC ON)]
            │       └── ⚔️ UseSkill(Laser|Laser3)
            └── ❓ Selector [🛡️CheckActorEffect(Self.M_Common_CheckStoryMode ON)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_TurretLaser_TypeA ON) && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
                │   ├── ⚔️ UseSkill(Laser)
                │   └── ⏳ Wait(2.0s)
                ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_TurretLaser_TypeFastA ON) && 🛡️CheckActorEffect(Self.M_TurretLaser_Pause)]
                │   └── ⚔️ UseSkill(LaserFast)
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_TurretLaser_TypeB ON) && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
                │   ├── ⚔️ UseSkill(Laser|Laser1|Laser2)
                │   └── ⏳ Wait(2.0s)
                └── ➡️ Sequence [🛡️CheckActorEffect(Self.M_TurretLaser_TypeC ON) && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
                    ├── ⚔️ UseSkill(Laser|Laser3)
                    └── ⏳ Wait(2.0s)
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 15 |
| Selector | 13 |
| Dec/AggroLevel | 11 |
| Task/UseSkill | 8 |
| Sequence | 5 |
| Task/Wait | 5 |
| Dec/IsAlive | 3 |
| Task/PlayShow | 2 |
| Dec/Blackboard | 2 |
| Task/Blackboard | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/UseEffect | 1 |

## 스킬 목록
- UseSkill(Laser)
- UseSkill(LaserFast)
- UseSkill(Laser|Laser1|Laser2)
- UseSkill(Laser|Laser3)
- UseSkill(Laser)
- UseSkill(LaserFast)
- UseSkill(Laser|Laser1|Laser2)
- UseSkill(Laser|Laser3)
