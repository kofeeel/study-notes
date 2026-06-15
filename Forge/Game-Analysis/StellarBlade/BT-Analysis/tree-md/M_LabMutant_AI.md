# M_LabMutant_AI

> Auto-generated from FModel JSON export

```
❓ Selector
    └── ❓ Selector [🛡️IsAlive(Self)]
        ├── ❓ Selector
        │   └── ⚔️ UseSkill(SpawnShow_01)
        ├── 👁️ DetectTarget [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️IsActiveSkill(NOT)]
        │   ├── 🎬 PlayShow [🛡️Blackboard(BattleStart)]
        │   └── 📋 Blackboard(BattleStart)
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction) && 🛡️IsActiveSkill(NOT)]
        │   └── 🏠 MoveToHome [🛡️DetectResult(==)]
        ├── ➡️ Sequence [🛡️AggroLevel(AIAggroLevel_Battle) && 🛡️CheckActorEffect(Self.LV_MetaAction ON)]
        │   └── 🧠 MetaAI
        ├── ❓ Selector [🛡️IsAlive(Target)]
        │   └── ⏳ Wait(1.0s)
        └── ❓ Selector [🛡️IsAlive(Target)]
            ├── ❓ Selector [🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Self.M_Common_SpawnEvent)]
            │   ├── ❓ Selector [🛡️Blackboard(BattleStart)]
            │   │   └── ➡️ Sequence [🛡️CheckStance(M_LabMutant_StandbySitting)]
            │   │       ├── ⚔️ UseSkill(StandbyToNormal combo=TableCommand)
            │   │       └── 📋 Blackboard(BattleStart)
            │   └── ➡️ Sequence [🛡️Blackboard(BattleStart) && 🛡️CheckStance(NOT M_LabMutant_StandbySitting) && 🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck) && 🛡️CheckStance(NOT M_LabMutant_StandbyLock)]
            │       ├── 📋 Blackboard(BattleStart)
            │       └── 🎬 PlayShow [🛡️CheckActorState(NOT ActorState_BlockingBehavior)]
            ├── ⏳ Wait(2.0s) [🛡️CheckActorEffect(Target.Item_Resurrection_Ground ON)]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.? ON)]
            │   └── ⚠️ CautionToTarget [🛡️TimeLimit(3.0s [AbnormalTimer])]
            ├── ❓ Selector [🛡️CheckActorEffect(Target.Check_LinkWait ON)]
            │   ├── ⏳ WaitTimeRandom
            │   └── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkSkill ON) && 🛡️TimeLimit(3.0s [Timer_LinkWait])]
            ├── ❓ Selector [🛡️IsGroupTarget && 🛡️AggroLevel(AIAggroLevel_Peaceful)]
            │   ├── ❓ Selector [🛡️IsGroupAttacker(NOT)]
            │   │   ├── ⚠️ CautionToTarget [🛡️CheckActorEffect(Target.Check_LinkWait)]
            │   │   └── 🚶 MoveToTarget [🛡️CheckActorEffect(Self.M_Check_WLMonster ON)]
            │   └── ❓ Selector [🛡️IsGroupAttacker && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
            │       ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.Check_ATLMonster ON)]
            │       │   ├── ➡️ Sequence [🛡️Blackboard(GrabSudden) && 🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck ON)]
            │       │   │   ├── ⚔️ UseSkill(GrabSudden)
            │       │   │   └── 📋 Blackboard(GrabSudden)
            │       │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck)]
            │       │   │   ├── ⚔️ UseSkill(Swing)
            │       │   │   └── ⚔️ UseSkill(Grab) [🛡️CheckActorEffect(Self.M_LabMutant_CheckGrab)]
            │       │   └── 🚶 MoveToTarget
            │       └── ❓ Selector [🛡️CheckActorEffect(Self.Check_ATLMonster ON)]
            │           ├── ➡️ Sequence [🛡️Blackboard(GrabSudden) && 🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck ON)]
            │           │   ├── ⚔️ UseSkill(GrabSudden)
            │           │   └── 📋 Blackboard(GrabSudden)
            │           ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck)]
            │           │   ├── ❓ Selector
            │           │   │   ├── ⚔️ UseSkill(Swing)
            │           │   │   └── ⚔️ UseSkill(Grab) [🛡️CheckActorEffect(Self.M_LabMutant_CheckGrab)]
            │           │   └── ⏳ WaitTimeRandom
            │           └── 🚶 MoveToTarget
            └── ❓ Selector [🛡️IsGroupTarget(NOT) && 🛡️AggroLevel(AIAggroLevel_Peaceful) && 🛡️CheckActorEffect(Target.Check_LinkWait) && 🛡️CheckActorEffect(NOT Target.? ON)]
                ├── ❓ Selector [🛡️CheckActorEffect(NOT Self.Check_ATLMonster ON)]
                │   ├── ➡️ Sequence [🛡️Blackboard(GrabSudden) && 🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck ON)]
                │   │   ├── ⚔️ UseSkill(GrabSudden)
                │   │   └── 📋 Blackboard(GrabSudden)
                │   ├── ❓ Selector [🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck)]
                │   │   ├── ⚔️ UseSkill(Swing)
                │   │   └── ⚔️ UseSkill(Grab) [🛡️CheckActorEffect(Self.M_LabMutant_CheckGrab)]
                │   └── 🚶 MoveToTarget
                └── ❓ Selector [🛡️CheckActorEffect(Self.Check_ATLMonster ON)]
                    ├── ➡️ Sequence [🛡️Blackboard(GrabSudden) && 🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck ON)]
                    │   ├── ⚔️ UseSkill(GrabSudden)
                    │   └── 📋 Blackboard(GrabSudden)
                    ├── ➡️ Sequence [🛡️CheckActorEffect(Self.M_LabMutant_GrabSuddenCheck)]
                    │   ├── ❓ Selector
                    │   │   ├── ⚔️ UseSkill(Swing)
                    │   │   └── ⚔️ UseSkill(Grab) [🛡️CheckActorEffect(Self.M_LabMutant_CheckGrab)]
                    │   └── ⏳ WaitTimeRandom
                    └── 🚶 MoveToTarget
```

## 노드 통계
| 타입 | 수 |
|------|---|
| Dec/CheckActorEffect | 30 |
| Selector | 21 |
| Task/UseSkill | 14 |
| Sequence | 11 |
| Dec/AggroLevel | 7 |
| Dec/Blackboard | 7 |
| Task/Blackboard | 7 |
| Task/MoveToTarget | 5 |
| Dec/IsAlive | 3 |
| Dec/CheckStance | 3 |
| Task/CautionToTarget | 3 |
| Task/WaitTimeRandom | 3 |
| Dec/DetectResult | 2 |
| Dec/IsActiveSkill | 2 |
| Task/PlayShow | 2 |
| Task/Wait | 2 |
| Dec/TimeLimit | 2 |
| Dec/IsGroupTarget | 2 |
| Dec/IsGroupAttacker | 2 |
| Task/DetectTarget | 1 |
| Task/MoveToHome | 1 |
| Task/MetaAI | 1 |
| Dec/CheckActorState | 1 |

## 스킬 목록
- UseSkill(SpawnShow_01)
- UseSkill(StandbyToNormal combo=TableCommand)
- UseSkill(GrabSudden)
- UseSkill(Swing)
- UseSkill(Grab)
- UseSkill(GrabSudden)
- UseSkill(Swing)
- UseSkill(Grab)
- UseSkill(GrabSudden)
- UseSkill(Swing)
- UseSkill(Grab)
- UseSkill(GrabSudden)
- UseSkill(Swing)
- UseSkill(Grab)
