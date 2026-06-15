# N_DummyBeach_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ➡️ Sequence [🛡️Blackboard(BB1)]
            │   ├── 🔄 UseableTimeReset
            │   └── 📋 Blackboard(BB1)
            ├── ⚔️ UseSkill(BeachExplosionFront|BeachExplosionEve|BeachExplosionBack) [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.N_Dummy_Chasing ON) && 🛡️UseableTime]
            ├── ⚔️ UseSkill(BeachExplosion) [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(NOT Self.N_Dummy_Chasing ON) && 🛡️CheckActorEffect(NOT Self.N_Dummy_ExplosionSelf ON)]
            └── ⚔️ UseSkill(BeachExplosionSelf) [🛡️CheckActorEffect(Self.N_Dummy_ExplosionSelf ON) && 🛡️CheckActorEffect(Self.N_Dummy_ExplosionSelfReady ON)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 5 |
| Selector | 3 |
| Task/UseSkill | 3 |
| Dec/IsAlive | 2 |
| Dec/AggroLevel | 2 |
| Task/DetectTarget | 1 |
| Dec/DetectResult | 1 |
| Sequence | 1 |
| Dec/Blackboard | 1 |
| Task/UseableTimeReset | 1 |
| Task/Blackboard | 1 |
| Dec/UseableTime | 1 |

## 스킬 목록
- UseSkill(BeachExplosionFront|BeachExplosionEve|BeachExplosionBack)
- UseSkill(BeachExplosion)
- UseSkill(BeachExplosionSelf)
