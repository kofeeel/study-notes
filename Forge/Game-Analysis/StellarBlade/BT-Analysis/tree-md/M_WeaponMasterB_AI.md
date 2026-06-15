# M_WeaponMasterB_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
        │   └── ➡️ Sequence [🛡️Blackboard(BattleStart)]
        │       ├── 📋 Blackboard(BattleStart)
        │       ├── 🚶 MoveToTarget [🛡️TimeLimit(3.5s [StartTimer1])]
        │       └── ⚔️ UseSkill(TwinSwordRushChain)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [DownCautionTimer1])]
            └── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful)]
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Blackboard(FirstShot)]
                │   ├── 📋 Blackboard(FirstShot)
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand) [🛡️Random(rand(100)<=50)]
                ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_Common_HitProjectileResult ON) && 🛡️Random(rand(100)<=50)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️AimMe && 🛡️Random(rand(100)<=50) && 🛡️CheckActorStat(ActorStatType_HP<=20.0%)]
                │   └── ⚔️ UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
                ├── ➡️ Sequence [🛡️Blackboard(SK1)]
                │   └── 📋 Blackboard(SK1)
                ├── ❓ Selector [🛡️Blackboard(SK1)]
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(TwinSwordRushChain)
                │   │   └── 📋 Blackboard(SK1)
                │   └── ➡️ Sequence
                │       ├── ⚔️ UseSkill(TwinSwordCombo)
                │       └── 📋 Blackboard(SK1)
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Target.? ON)]
                │   ├── ⚔️ UseSkill(ParrySkill)
                │   ├── ⚔️ UseSkill(CounterSkill) [🛡️CheckActorStat(ActorStatType_HP<=90.0%)]
                │   ├── ➡️ Sequence
                │   │   ├── ⚔️ UseSkill(MoveLeft|MoveRight|MoveBack)
                │   │   └── ⚠️ CautionToTarget [🛡️TimeLimit(2.5s [CautionTimer1]) && 🛡️DistanceToTarget(dist<=1000.0) && 🛡️DistanceToTarget(dist>=200.0)]
                │   ├── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=1500.0) && 🛡️DistanceToTarget(dist>=200.0) && 🛡️CheckActorEffect(Self.M_WeaponMasterB_CheckRun)]
                │   ├── ❓ Selector
                │   │   ├── ⚔️ UseSkill(TwinSwordRushChain) [🛡️CheckActorStat(ActorStatType_HP<=80.0%)]
                │   │   ├── ⚔️ UseSkill(TwinSwordCombo|TwinSwordCombo_2) [🛡️CheckActorStat(ActorStatType_HP<=85.0%)]
                │   │   └── ⚔️ UseSkill(TwinSwordRush)
                │   └── ❓ Selector
                │       ├── ⚔️ UseSkill(TwinSwordSpin)
                │       ├── ⚔️ UseSkill(TwinSwordSwing)
                │       └── ⚠️ CautionToTarget [🛡️TimeLimit(1.5s [CautionTimer2]) && 🛡️DistanceToTarget(dist<=500.0) && 🛡️DistanceToTarget(dist>=200.0)]
                └── ➡️ Sequence
                    ├── ✨ UseEffect(['M_WeaponMasterB_CheckRun'])
                    └── ❓ Selector
                        └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/UseSkill | 14 |
| Selector | 12 |
| Sequence | 9 |
| Dec/DistanceToTarget | 6 |
| Task/Blackboard | 5 |
| Dec/TimeLimit | 5 |
| Dec/CheckActorEffect | 5 |
| Dec/Blackboard | 4 |
| Task/CautionToTarget | 4 |
| Dec/CheckActorStat | 4 |
| Dec/IsAlive | 3 |
| Dec/Random | 3 |
| Dec/AggroLevel | 2 |
| Task/MoveToTarget | 2 |
| Task/Wait | 2 |
| Dec/AimMe | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Task/UseEffect | 1 |

## 스킬 목록
- UseSkill(TwinSwordRushChain)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(EvasionLeft|EvasionRight combo=TableCommand)
- UseSkill(TwinSwordRushChain)
- UseSkill(TwinSwordCombo)
- UseSkill(ParrySkill)
- UseSkill(CounterSkill)
- UseSkill(MoveLeft|MoveRight|MoveBack)
- UseSkill(TwinSwordRushChain)
- UseSkill(TwinSwordCombo|TwinSwordCombo_2)
- UseSkill(TwinSwordRush)
- UseSkill(TwinSwordSpin)
- UseSkill(TwinSwordSwing)
