# 03. GAS 전투 시스템

> Gameplay Ability System 기반 전투 설계 분석

## GameplayAbility (GA_) — 195개

### Item (42개)
- `GA_ConsumeItemBase`
- `GA_PriFunction_1HAxe_L`
- `GA_PriFunction_1HAxe`
- `GA_MeleeAttack_1HBareHands_01`
- `GA_PriFunction_1HDagger_L`
- `GA_PriFunction_1HDagger`
- `GA_PriFunction_1HLBlunt_L`
- `GA_PriFunction_1HHammer`
- `GA_PriFunction_1HLSlash`
- `GA_PriFunction_1HStick_L`
- `GA_PriFunction_1HStick`
- `GA_PriFunction_Brick`
- `GA_PriFunction_2HHammer`
- `GA_PriFunction_2HAxe`
- `GA_PriFunction_2HStick`
- `GA_MeleeAttack_BareHands_01`
- `GA_PriFunction_RAxeLAxe`
- `GA_PriFunction_RAxeLDagger`
- `GA_PriFunction_RAxeLHammer`
- `GA_PriFunction_RAxeLKnife`
- `GA_PriFunction_RAxeLStick`
- `GA_PriFunction_RDaggerLAxe`
- `GA_PriFunction_RDaggerLDagger`
- `GA_PriFunction_RDaggerLHammer`
- `GA_PriFunction_RDaggerLKnife`
- `GA_PriFunction_RDaggerLStick`
- `GA_PriFunction_RHammerLAxe`
- `GA_PriFunction_RHammerLDagger`
- `GA_PriFunction_RHammerLHammer`
- `GA_PriFunction_RHammerLKnife`
- `GA_PriFunction_RHammerLStick`
- `GA_PriFunction_RKnifeLAxe`
- `GA_PriFunction_RKnifeLDagger`
- `GA_PriFunction_RKnifeLHammer`
- `GA_PriFunction_RKnifeLKnife`
- `GA_PriFunction_RKnifeLStick`
- `GA_PriFunction_RStickLAxe`
- `GA_PriFunction_RStickLDagger`
- `GA_PriFunction_RStickLHammer`
- `GA_PriFunction_RStickLKnife`
- `GA_PriFunction_RStickLStick`
- `GA_Guard_1HShield`

### Action/Skill (23개)
- `GA_BoxMode`
- `GA_Decoy`
- `GA_DecoyInstall`
- `GA_DecoyInteraction`
- `GA_DualAttack`
- `GA_EmergencySmoke`
- `GA_Kick`
- `GA_Rolling`
- `GA_Rush`
- `GA_ListenMode`
- `GA_RopeClimb`
- `GA_RopeConstruct`
- `GA_RopeDart`
- `GA_RopeDartHitReaction_Zombie`
- `GA_RopeDartHitReaction`
- `GA_BaitInstall`
- `GA_BaitInteraction`
- `GA_BaitTrap`
- `GA_BearTrap`
- `GA_BearTrapReaction`
- `GA_InGameUI_TrapRadial`
- `GA_TrapInstall`
- `GA_TrapInteraction`

### Weapon (15개)
- `GA_MeleeAttack_Base`
- `GA_MeleeGuard_Base`
- `GA_MeleeWeapon_1HGuard_Left`
- `GA_MeleeWeapon_1HGuard`
- `GA_MeleeWeapon_2HGuard`
- `GA_MeleeWeapon_BareHandsGuard`
- `GA_MeleeWeaponGuard_Base`
- `GA_SelectAttackType`
- `GA_Throw`
- `GA_WeaponThrowable`
- `GA_RangedWeapon_ADS`
- `GA_RangedWeapon_Fire`
- `GA_RangedWeapon_Reload`
- `GA_MeleeAttack_BTTBase`
- `GA_MeleeAttack_Unarmed`

### UI/Tablet (8개)
- `GA_InGameUIWithAnim_Tablet`
- `GA_Tablet_Alarm`
- `GA_Tablet_Base`
- `GA_Tablet_Chat`
- `GA_Tablet_Home`
- `GA_Tablet_Quest`
- `GA_Tablet_Social`
- `GA_Tablet_WorldMap`

### 기타 (8개)
- `GA_HiveSpawn`
- `GA_ShutterSpawn`
- `GA_SlowCrawlSpawn`
- `GA_ShowLoadoutPrepareStashStorage_Carrier`
- `GA_ShowLoadoutPrepareStashStorage_Closet`
- `GA_ShowLoadoutPrepareStashStorage`
- `GA_PushSceneData`
- `GA_ShowTutoriaTent`

### Action/Rigidify (7개)
- `GA_AdditiveHitReaction`
- `GA_GuardReaction`
- `GA_Knockback`
- `GA_Stun`
- `GA_Zombie_AdditiveHitReaction`
- `GA_Zombie_Knockback`
- `GA_Zombie_Stun`

### Interact/Exit (6개)
- `GA_GateExit_Close`
- `GA_GateExit`
- `GA_Interaction_ActivateLockedSwitchV2_Tutorial`
- `GA_Interaction_ActivateLockedSwitchV2`
- `GA_Interaction_SwitchOnV2`
- `GA_TutorialExit_Extract`

### UI/MainMenu (6개)
- `GA_InGameUIWithAnim_MainMenu`
- `GA_Lobby_BuildMode`
- `GA_MainMenu_Base`
- `GA_MainMenu_PlayerInventory`
- `GA_MainMenu_Skill`
- `GA_MainMenu_Trait`

### Action/VoiceChat (5개)
- `GA_VoiceApplySavedMicState`
- `GA_VoiceApplySavedMode`
- `GA_VoiceChatDebugRefresh`
- `GA_VoiceModeToggle`
- `GA_VoiceToggleV2`

### Interact/Housing (5개)
- `GA_Interaction_Interactive_BuildObject`
- `GA_Interaction_StorageBuildObject_Connect_CounterTop`
- `GA_Interaction_StorageBuildObject_Connect_Crafting`
- `GA_Interaction_StorageBuildObject_Connect`
- `GA_InteractionUpgradeBuildObject`

### Interact/LockedObject (5개)
- `GA_LockedObject_ScrollKeyItem`
- `GA_LockedObject_Unlock_SwitchButtonOpen`
- `GA_LockedObject_Unlock`
- `GA_MiddleExit_Unlock`
- `GA_PassCode_Unlock`

### Interact/ExternalInventory (4개)
- `GA_ExternalInventory_Gather_Hold`
- `GA_ExternalInventory_Search_V2`
- `GA_TriggerSearch_Refrigerator`
- `GA_TriggerSearch`

### Interact/Rope (3개)
- `GA_Interaction_Rope_Climb`
- `GA_Interaction_Rope_Collect`
- `GA_Interaction_Rope_Pick`

### Character (3개)
- `GA_MeleeAttack_Unarmed_PlayerTest`
- `GA_Parkour_Zombie_PlayerTest`
- `GA_Zombie_Bite_Base_PlayerTest`

### Interact/Door (2개)
- `GA_Door_OpenClose`
- `GA_Door_SilentOpenClose`

### Interact/HideAndSeek (2개)
- `GA_Interaction_HS_Hide_Peek`
- `GA_Interaction_HS_Hide_Tap`

### UI/InGameHUD (2개)
- `GA_InGameUI_EmoteRadial`
- `GA_InGameUI_Radial`

### Action/GA_BittenByZombie.uasset (1개)
- `GA_BittenByZombie`

### Action/GA_BoxerStep.uasset (1개)
- `GA_BoxerStep`

### Action/GA_CancelAction.uasset (1개)
- `GA_CancelAction`

### Action/GA_ChangeHoldableSlot.uasset (1개)
- `GA_ChangeHoldableSlot`

### Action/GA_ChangeToBareHand.uasset (1개)
- `GA_ChangeToBareHand`

### Action/GA_Debuff_SpeedReduction.uasset (1개)
- `GA_Debuff_SpeedReduction`

### Action/GA_Dropdown.uasset (1개)
- `GA_Dropdown`

### Action/GA_Emote.uasset (1개)
- `GA_Emote`

### Action/GA_FallFromHeight.uasset (1개)
- `GA_FallFromHeight`

### Action/GA_FreeLook.uasset (1개)
- `GA_FreeLook`

### Action/GA_Groggy.uasset (1개)
- `GA_Groggy`

### Action/GA_GuardBroken.uasset (1개)
- `GA_GuardBroken`

### Action/GA_Hero_Death.uasset (1개)
- `GA_Hero_Death`

### Action/GA_HideAndSeek_Exit.uasset (1개)
- `GA_HideAndSeek_Exit`

### Action/GA_HideAndSeek_TogglePeek.uasset (1개)
- `GA_HideAndSeek_TogglePeek`

### Action/GA_HitReaction.uasset (1개)
- `GA_HitReaction`

### Action/GA_Jump.uasset (1개)
- `GA_Jump`

### Action/GA_LastStand_Begin.uasset (1개)
- `GA_LastStand_Begin`

### Action/GA_LastStand.uasset (1개)
- `GA_LastStand`

### Action/GA_Parkour_Character.uasset (1개)
- `GA_Parkour_Character`

### Action/GA_Parkour_Zombie.uasset (1개)
- `GA_Parkour_Zombie`

### Action/GA_Resurrect.uasset (1개)
- `GA_Resurrect`

### Action/GA_Sprint.uasset (1개)
- `GA_Sprint`

### Action/GA_ToggleCrouch.uasset (1개)
- `GA_ToggleCrouch`

### Action/GA_ToggleFlashLight.uasset (1개)
- `GA_ToggleFlashLight`

### Action/GA_Zombie_Bite_Runner.uasset (1개)
- `GA_Zombie_Bite_Runner`

### Action/GA_Zombie_Bite.uasset (1개)
- `GA_Zombie_Bite`

### Action/GA_Zombie_Death.uasset (1개)
- `GA_Zombie_Death`

### Action/GA_Zombie_HitReaction.uasset (1개)
- `GA_Zombie_HitReaction`

### Action/GA_Zombie_Knockdown_Strong.uasset (1개)
- `GA_Zombie_Knockdown_Strong`

### Action/GA_Zombie_Knockdown.uasset (1개)
- `GA_Zombie_Knockdown`

### Action/GA_Zombie_Scream.uasset (1개)
- `GA_Zombie_Scream`

### Action/GA_Zombie_SnapBite.uasset (1개)
- `GA_Zombie_SnapBite`

### Action/GA_Zombie_TakeDown.uasset (1개)
- `GA_Zombie_TakeDown`

### Action/GA_ZombieDropdown.uasset (1개)
- `GA_ZombieDropdown`

### Action/Status (1개)
- `GA_Weary`

### Global/GA_RoundStartReadyBlockAbilities.uasset (1개)
- `GA_RoundStartReadyBlockAbilities`

### Interact/GA_Interaction_AlwaysFail.uasset (1개)
- `GA_Interaction_AlwaysFail`

### Interact/GA_Interaction_Collect.uasset (1개)
- `GA_Interaction_Collect`

### Interact/GA_Interaction_GroggyAttack.uasset (1개)
- `GA_Interaction_GroggyAttack`

### Interact/GA_Interaction_Housing.uasset (1개)
- `GA_Interaction_Housing`

### Interact/GA_Interaction_LastStandHelpGive.uasset (1개)
- `GA_Interaction_LastStandHelpGive`

### Interact/GA_Interaction_Read.uasset (1개)
- `GA_Interaction_Read`

### Interact/GA_Interaction_TakeDown.uasset (1개)
- `GA_Interaction_TakeDown`

### Interact/GA_Interaction.uasset (1개)
- `GA_Interaction`

### Interact/Tutorial (1개)
- `GA_TutorialTent`

### Inventory/GA_InventoryActions.uasset (1개)
- `GA_InventoryActions`

### Phases/GA_FinishPlay.uasset (1개)
- `GA_FinishPlay`

### Tutorial/GA_TutorialBlockMovement.uasset (1개)
- `GA_TutorialBlockMovement`

## GameplayEffect (GE_) — 59개

- `GE_Cooldown_Infinite` — `BluePrints/AbilitySystem/Abilities/Action/GE_Cooldown_Infinite.uasset`
- `GE_Damage_Bite` — `BluePrints/AbilitySystem/Abilities/Action/GE_Damage_Bite.uasset`
- `GE_Death` — `BluePrints/AbilitySystem/Abilities/Action/GE_Death.uasset`
- `GE_ListenModeStaminaTickCost` — `BluePrints/AbilitySystem/Abilities/Action/Skill/ListenMode/GE_ListenModeStaminaTickCost.uasset`
- `GE_TrapDamage` — `BluePrints/AbilitySystem/Abilities/Action/Skill/Trap/GE_TrapDamage.uasset`
- `GE_AttackCost_Instant` — `BluePrints/AbilitySystem/GameplayEffects/Cost/GE_AttackCost_Instant.uasset`
- `GE_MeleeAttackActiveSkillCost_Instant` — `BluePrints/AbilitySystem/GameplayEffects/Cost/GE_MeleeAttackActiveSkillCost_Instant.uasset`
- `GE_Eat_Basic` — `BluePrints/AbilitySystem/GameplayEffects/Food/Eat/GE_Eat_Basic.uasset`
- `GE_Eat_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Food/Eat/GE_Eat_SetByCaller.uasset`
- `GE_Food_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Food/GE_Food_SetByCaller.uasset`
- `GE_BaitTrapCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_BaitTrapCooldown.uasset`
- `GE_BearTrapCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_BearTrapCooldown.uasset`
- `GE_Blind` — `BluePrints/AbilitySystem/GameplayEffects/GE_Blind.uasset`
- `GE_BoxerStepCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_BoxerStepCooldown.uasset`
- `GE_BoxModeCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_BoxModeCooldown.uasset`
- `GE_DecoyCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_DecoyCooldown.uasset`
- `GE_DefaultCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_DefaultCooldown.uasset`
- `GE_DynamicTag` — `BluePrints/AbilitySystem/GameplayEffects/GE_DynamicTag.uasset`
- `GE_EmergencySmokeCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_EmergencySmokeCooldown.uasset`
- `GE_KickCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_KickCooldown.uasset`
- `GE_ListenModeCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_ListenModeCooldown.uasset`
- `GE_ProtectionNotify` — `BluePrints/AbilitySystem/GameplayEffects/GE_ProtectionNotify.uasset`
- `GE_RollingCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_RollingCooldown.uasset`
- `GE_RopeConstructCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_RopeConstructCooldown.uasset`
- `GE_RopeDartCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_RopeDartCooldown.uasset`
- `GE_RushCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_RushCooldown.uasset`
- `GE_SprintCooldown` — `BluePrints/AbilitySystem/GameplayEffects/GE_SprintCooldown.uasset`
- `GE_SuperArmor` — `BluePrints/AbilitySystem/GameplayEffects/GE_SuperArmor.uasset`
- `GE_ToggleFlashLight_On` — `BluePrints/AbilitySystem/GameplayEffects/GE_ToggleFlashLight_On.uasset`
- `GE_Damage_Basic_Instant` — `BluePrints/AbilitySystem/GameplayEffects/Health/Damage/GE_Damage_Basic_Instant.uasset`
- `GE_Damage_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Damage/GE_Damage_Basic_SetByCaller.uasset`
- `GE_Damage_Melee` — `BluePrints/AbilitySystem/GameplayEffects/Health/Damage/GE_Damage_Melee.uasset`
- `GE_Damage_Throw` — `BluePrints/AbilitySystem/GameplayEffects/Health/Damage/GE_Damage_Throw.uasset`
- `GE_MaxHealth_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/GE_MaxHealth_SetByCaller.uasset`
- `GE_Duration_Heal_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Heal/GE_Duration_Heal_Basic_SetByCaller.uasset`
- `GE_Heal_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Heal/GE_Heal_Basic_SetByCaller.uasset`
- `GE_Infect_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Infection/GE_Infect_Basic_SetByCaller.uasset`
- `GE_Infected_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Infection/GE_Infected_Basic_SetByCaller.uasset`
- `GE_BlackOutLimit_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Injury/GE_BlackOutLimit_Basic_SetByCaller.uasset`
- `GE_BlackoutRecoverRate_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Injury/GE_BlackoutRecoverRate_Basic_SetByCaller.uasset`
- `GE_BlackoutRecoverRateWithHeal_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Injury/GE_BlackoutRecoverRateWithHeal_Basic_SetByCaller.uasset`
- `GE_Duration_BlackoutRecoverValue_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Injury/GE_Duration_BlackoutRecoverValue_Basic_SetByCaller.uasset`
- `GE_Duration_BlackoutRecoverValueWithHeal_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Injury/GE_Duration_BlackoutRecoverValueWithHeal_Basic_SetByCaller.uasset`
- `GE_InjuryRate_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Injury/GE_InjuryRate_Basic_SetByCaller.uasset`
- `GE_Mental_Basic_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Mental/GE_Mental_Basic_SetByCaller.uasset`
- `GE_Mental_Override_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Health/Mental/GE_Mental_Override_SetByCaller.uasset`
- `GE_ConsumeDurationItem_MoveStamina_Duration` — `BluePrints/AbilitySystem/GameplayEffects/Stamina/GE_ConsumeDurationItem_MoveStamina_Duration.uasset`
- `GE_ConsumeItem_MoveStamina_Instant` — `BluePrints/AbilitySystem/GameplayEffects/Stamina/GE_ConsumeItem_MoveStamina_Instant.uasset`
- `GE_DecreaseMoveStamina_Infinite` — `BluePrints/AbilitySystem/GameplayEffects/Stamina/GE_DecreaseMoveStamina_Infinite.uasset`
- `GE_GroggyAttack` — `BluePrints/AbilitySystem/GameplayEffects/Stamina/GE_GroggyAttack.uasset`
- `GE_IncreaseMoveStamina__Infinite` — `BluePrints/AbilitySystem/GameplayEffects/Stamina/GE_IncreaseMoveStamina__Infinite.uasset`
- `GE_Stamina_SetByCaller_Infinite` — `BluePrints/AbilitySystem/GameplayEffects/Stamina/GE_Stamina_SetByCaller_Infinite.uasset`
- `GE_Stamina_SetByCaller_Instant` — `BluePrints/AbilitySystem/GameplayEffects/Stamina/GE_Stamina_SetByCaller_Instant.uasset`
- `GE_Stat_Max_Infection_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/StatModifiers/GE_Stat_Max_Infection_SetByCaller.uasset`
- `GE_Stat_Max_MoveStamina` — `BluePrints/AbilitySystem/GameplayEffects/StatModifiers/GE_Stat_Max_MoveStamina.uasset`
- `GE_Stat_SightAggroScale_BoxMode` — `BluePrints/AbilitySystem/GameplayEffects/StatModifiers/GE_Stat_SightAggroScale_BoxMode.uasset`
- `GE_Stat_SightAggroScale_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/StatModifiers/GE_Stat_SightAggroScale_SetByCaller.uasset`
- `GE_StatusEffectAttributeModifier` — `BluePrints/AbilitySystem/GameplayEffects/StatModifiers/GE_StatusEffectAttributeModifier.uasset`
- `GE_Weight_SetByCaller` — `BluePrints/AbilitySystem/GameplayEffects/Weight/GE_Weight_SetByCaller.uasset`

## GameplayCue (GC_) — 9개

- `GC_MetalDoorOpenCloseExample` — `BluePrints/AbilitySystem/GameplayCues/Door/GC_MetalDoorOpenCloseExample.uasset`
- `GC_MetalDoorOpenedClosedExample` — `BluePrints/AbilitySystem/GameplayCues/Door/GC_MetalDoorOpenedClosedExample.uasset`
- `GC_WoodDoorCrackedExample` — `BluePrints/AbilitySystem/GameplayCues/Door/GC_WoodDoorCrackedExample.uasset`
- `GC_WoodDoorOpenCloseExample` — `BluePrints/AbilitySystem/GameplayCues/Door/GC_WoodDoorOpenCloseExample.uasset`
- `GC_WoodDoorOpenedClosedExample` — `BluePrints/AbilitySystem/GameplayCues/Door/GC_WoodDoorOpenedClosedExample.uasset`
- `GC_GatheringSound` — `BluePrints/AbilitySystem/GameplayCues/GatheringSpawner/GC_GatheringSound.uasset`
- `GC_SearchingSound` — `BluePrints/AbilitySystem/GameplayCues/SearchableContainer/GC_SearchingSound.uasset`
- `GC_UnlockingFailSound` — `BluePrints/AbilitySystem/GameplayCues/SearchableContainer/GC_UnlockingFailSound.uasset`
- `GC_UnlockingSound` — `BluePrints/AbilitySystem/GameplayCues/SearchableContainer/GC_UnlockingSound.uasset`

## AbilitySystem 폴더 트리
```
BluePrints/AbilitySystem/Abilities/Action
BluePrints/AbilitySystem/Abilities/Action/Rigidify
BluePrints/AbilitySystem/Abilities/Action/Skill
BluePrints/AbilitySystem/Abilities/Action/Skill/BoxMode
BluePrints/AbilitySystem/Abilities/Action/Skill/Decoy
BluePrints/AbilitySystem/Abilities/Action/Skill/EmergencySmoke
BluePrints/AbilitySystem/Abilities/Action/Skill/ListenMode
BluePrints/AbilitySystem/Abilities/Action/Skill/Rope
BluePrints/AbilitySystem/Abilities/Action/Skill/RopeDart
BluePrints/AbilitySystem/Abilities/Action/Skill/Trap
BluePrints/AbilitySystem/Abilities/Action/Status
BluePrints/AbilitySystem/Abilities/Action/VoiceChat
BluePrints/AbilitySystem/Abilities/Global
BluePrints/AbilitySystem/Abilities/Interact
BluePrints/AbilitySystem/Abilities/Interact/Door
BluePrints/AbilitySystem/Abilities/Interact/Exit
BluePrints/AbilitySystem/Abilities/Interact/Exit/Legacy
BluePrints/AbilitySystem/Abilities/Interact/ExternalInventory/Gathering
BluePrints/AbilitySystem/Abilities/Interact/ExternalInventory/Searching
BluePrints/AbilitySystem/Abilities/Interact/HideAndSeek
BluePrints/AbilitySystem/Abilities/Interact/Housing
BluePrints/AbilitySystem/Abilities/Interact/LockedObject
BluePrints/AbilitySystem/Abilities/Interact/Rope
BluePrints/AbilitySystem/Abilities/Interact/Tutorial
BluePrints/AbilitySystem/Abilities/Inventory
BluePrints/AbilitySystem/Abilities/Phases
BluePrints/AbilitySystem/Abilities/Tutorial
BluePrints/AbilitySystem/Abilities/UI/InGameHUD
BluePrints/AbilitySystem/Abilities/UI/MainMenu
BluePrints/AbilitySystem/Abilities/UI/Tablet
BluePrints/AbilitySystem/AbilitySet
BluePrints/AbilitySystem/GameplayCues
BluePrints/AbilitySystem/GameplayCues/Character
BluePrints/AbilitySystem/GameplayCues/Character/AttackTypes/Air
BluePrints/AbilitySystem/GameplayCues/Character/AttackTypes/Basic1
BluePrints/AbilitySystem/GameplayCues/Character/AttackTypes/Basic2
BluePrints/AbilitySystem/GameplayCues/Door
BluePrints/AbilitySystem/GameplayCues/GatheringSpawner
BluePrints/AbilitySystem/GameplayCues/Gear
BluePrints/AbilitySystem/GameplayCues/Gear/AttackTypes/Air
BluePrints/AbilitySystem/GameplayCues/Gear/AttackTypes/Basic1
BluePrints/AbilitySystem/GameplayCues/SearchableContainer
BluePrints/AbilitySystem/GameplayCues/ThrownActor
BluePrints/AbilitySystem/GameplayEffects
BluePrints/AbilitySystem/GameplayEffects/Cost
BluePrints/AbilitySystem/GameplayEffects/Food
BluePrints/AbilitySystem/GameplayEffects/Food/Eat
BluePrints/AbilitySystem/GameplayEffects/Health
BluePrints/AbilitySystem/GameplayEffects/Health/Damage
BluePrints/AbilitySystem/GameplayEffects/Health/Heal
BluePrints/AbilitySystem/GameplayEffects/Health/Infection
BluePrints/AbilitySystem/GameplayEffects/Health/Injury
BluePrints/AbilitySystem/GameplayEffects/Health/Mental
BluePrints/AbilitySystem/GameplayEffects/Stamina
BluePrints/AbilitySystem/GameplayEffects/Stamina/Execution
BluePrints/AbilitySystem/GameplayEffects/StatModifiers
BluePrints/AbilitySystem/GameplayEffects/Weight
```
