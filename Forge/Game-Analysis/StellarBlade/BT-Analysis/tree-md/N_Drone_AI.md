# N_Drone_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    ├── ➡️ Sequence [🛡️CheckStance(P_Eve_Gun) && 🛡️Blackboard(DroneEquip)]
    │   ├── ⚔️ UseSkill(Gun_DroneEquip1_1)
    │   └── 📋 Blackboard(DroneEquip)
    ├── ➡️ Sequence [🛡️CheckStance(NOT P_Eve_Gun) && 🛡️Blackboard(DroneEquip)]
    │   ├── 📋 Blackboard(DroneEquip)
    │   └── ⚔️ UseSkill(Gun_DroneUnequip1_1)
    └── ❓ Selector [🛡️CheckStance(NOT P_Eve_Gun) && 🛡️CheckStance(NOT P_Eve_GunTutorial) && 🛡️CheckStance(NOT P_Eve_GunBlockSword) && 🛡️Blackboard(DroneEquip)]
        ├── ➡️ Sequence [🛡️CheckZoneEnvState && 🛡️Blackboard(BoxOpenShow)]
        │   ├── 🎬 PlayShow
        │   └── 📋 Blackboard(BoxOpenShow)
        ├── ➡️ Sequence [🛡️CheckZoneEnvState(NOT) && 🛡️Blackboard(BoxOpenShow)]
        │   └── 📋 Blackboard(BoxOpenShow)
        ├── 🔗 FollowTarget [🛡️Blackboard(NovaAI)]
        ├── 🔗 FollowTarget [🛡️CheckActorEffect(Owner.Check_LinkSkill ON)]
        ├── 🔗 FollowTarget [🛡️IsBattleMode && 🛡️CheckActorEffect(Owner.Check_LinkSkill)]
        ├── 🔗 FollowTarget [🛡️CheckActorEffect(Owner.P_Eve_FishingRodEquip_Stance ON)]
        ├── 🔗 FollowTarget [🛡️CheckAnimState && 🛡️CheckZoneEnvState(NOT) && 🛡️IsBattleMode(NOT) && 🛡️CheckActorEffect(NOT Owner.P_Eve_FishingRodEquip_Stance ON)]
        ├── 🔗 FollowTarget [🛡️CheckAnimState]
        ├── 🔗 FollowTarget [🛡️CheckAnimState]
        ├── 🔗 FollowTarget [🛡️CheckAnimState]
        ├── 🔗 FollowTarget [🛡️CheckAnimState]
        └── 🔗 FollowTarget [🛡️CheckAnimState(NOT) && 🛡️CheckAnimState(NOT) && 🛡️CheckAnimState(NOT) && 🛡️CheckAnimState(NOT) && 🛡️CheckAnimState(NOT)]
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Task/FollowTarget | 10 |
| Dec/CheckAnimState | 10 |
| Dec/Blackboard | 6 |
| Dec/CheckStance | 5 |
| Sequence | 4 |
| Task/Blackboard | 4 |
| Dec/CheckActorEffect | 4 |
| Dec/CheckZoneEnvState | 3 |
| Selector | 2 |
| Task/UseSkill | 2 |
| Dec/IsBattleMode | 2 |
| Task/PlayShow | 1 |

## 스킬 목록
- UseSkill(Gun_DroneEquip1_1)
- UseSkill(Gun_DroneUnequip1_1)
