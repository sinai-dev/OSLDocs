# SL_Effect

An <b>SL_Effect</b> object can be defined on an [SL_EffectTransform](API/SL_EffectTransform).

These effects work closely with [SL_EffectConditions](API/SL_EffectCondition).

## How to define one

<!-- tabs:start -->

#### ** XML **

```xml
<!-- replace 'EffectType' with the actual effect type -->
<SL_Effect xsi:type="EffectType">
    <!-- put fields and values for this effect here -->
</SL_Effect>
```

For example:

```xml
<SL_Effect xsi:type="SL_AddStatusEffectBuildUp">
    <StatusEffect>Doom</StatusEffect>
    <Buildup>60</Buildup>
	  <!-- etc ... -->
</SL_Effect>
```

#### ** C# **

Define any sub-class of SL_Effect (most likely on an [SL_EffectTransform](API/SL_EffectTransform)).

```csharp
var effect = new SL_AddStatusEffectBuildUp
{
  StatusEffect = "Doom",
  Buildup = 60f,
  // etc...
}
```

<!-- tabs:end -->

## Fields

<b>All SL_Effect classes have these fields.</b>

`Delay` (float)
* The time, in seconds, after which the effects will be applied. Default is 0.

`SyncType` (enum)
* Must be exactly one of: `OwnerSync`, `MasterSync` or `Everyone`
* <b>OwnerSync</b> means only the owner of the effect (person activating it) will execute the effects
* <b>MasterSync</b> means only the host character will execute it
* <b>Everyone</b> means everyone will execute the effect

`OverrideCategory` (enum)
* Overrides the TransformName to a specific effect category
* Must be exactly one of: `None`, `Normal`, `Activation`, `Restauration`, `Hit`, `Reference` or `Block`
* See "TransformName" on the [Effect Transforms](API/SL_EffectTransform) article for more information.

# SL_Effect sub-classes

Below are all the subclasses of SL_Effect. These contain the 3 fields above, as well as extra fields.

?> <b>Note:</b> There are a lot of subclasses. Use Ctrl+F to find the effects you are interested in.

## SL_AchievementOnEffect : SL_Effect

Simply gives the player an Achievement when this effect is used.

`UnlockedAchievement` (enum)
* The achievement which will be unlocked
* See [this list](API/Enums/Achievements) for possible values.

## SL_AchievementSetStatOnEffect : SL_Effect

Sets an Achievement Stat. These are used to track the player's progress for various Achievements.

`StatToChange` (enum)
* The AchievementStat to change. Must be one of: `NewRecipeLearned`, `LanternThrown`, `ItemEnchanted`, `SecretBossDefeated`

`IncreaseAmount` (int)
* The amount to increase the stat value by

## SL_AddAbsorbHealth : SL_Effect
Used to add health absorption to the character, for example by Infuse Blood.

`HealthRatio` (float)
* The ratio of health restored to the character based on all damage dealt

## SL_AddChargeInstrument : SL_Effect
Used to add charges to an instrument on hit.

`Charges` (int)
* Nummber of charges to add

## SL_AddStatusEffect : SL_Effect
Used for simply adding a status effect on activation.

`StatusEffect` (string)
* Must use a <b>Status Identifier</b>, not the actual name of the status effect.
* For a list of Statuses listed by their Identifier, see [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1969601658)
* For example, for Extreme Bleeding, the Identifier is `Bleeding +`.

`ChanceToContract` (float)
* Usually this is `100` for 100%, but it can be between 0 and 100.

`AffectController` (boolean)
* Default `false`
* If true, would override the affected character to the owner of this effect (ie. person creating it)

`AdditionalLevel` (int)
* Default `0`
* For Level Status Effects (ie Alert), adds extra levels

`NoDealer` (boolean)
* Default `false`
* If true, prevents this effect from knowing who applied it

## SL_AddStatusEffectRandom : SL_AddStatusEffect
Used by Jinx to apply a random Status Effect from a list of possible values.

`StatusIdentifiers` (list of string)
* The list of Status identifiers to choose from
* Each value is a `<string>IdentifierName</string>`

`ForceID` (int)
* If you want to override the result to a certain value, set this
* This is the index of the StatusIdentifiers list (starting at 0) to choose.

## SL_AddBoonEffect : SL_AddStatusEffect
This is used for Boons, for the Amplified Effect (with Shamanic Resonance).

It's exactly the same as `SL_AddStatusEffect`, except that you have one additional field.

`AmplifiedEffect`
* The Status Identifier for the effect received when you have Shamanic Resonance
* The game just adds ` Amplified` to the identifier for most boons, eg `Bless Amplified`

## SL_AddPukeOnEat : SL_Effect
Contains no extra fields, used by Indigestion to give the character a chance of puking on eating.

## SL_AddStatusEffectBuildUp : SL_Effect
For adding a build-up effect, usually used by Hit effects.

`StatusEffect` (string)
* Must use a <b>Status Identifier</b>, not the actual name of the status effect. (Same as StatusEffect on SL_AddStatusEffect)

`Buildup` (float)
* The effect build-up value, between 0 and 100.

`BuildUpMultiplier` (float)
* Multiplier on Buildup %. By default this should be 1.0.

`BypassCounter` (bool)
* Does this effect ignore counters?

## SL_AddStatusEffectBuildUpInstrument : SL_AddStatusEffectBuildUp
Inherits from `SL_AddStatusEffectBuildUp`, and contains one extra field.

`ChancesPerCharge` (float)
* How much extra buildup % the effect gains per charge on the instrument

## SL_AddStatusEffectIfDead : SL_AddStatusEffect
Inherits from `SL_AddStatusEffect`. Contains no extra fields, but gives the requirement that the target must have died at the same time this effect is checked.

## SL_AffectBurntHealth : SL_Effect
For affecting burnt health.

`AffectQuantity` (float)
* How much burnt health is affected

`IsModifier` (boolean)
* Should this be applied as a modifier (multiplier) value?

## SL_AffectBurntMana : SL_Effect
For affecting burnt mana.

`AffectQuantity` (float)
* How much mana burn is affected

`IsModifier` (boolean)
* Whether the value is applied as a % or flat value

## SL_AffectBurntStamina : SL_Effect
For affecting burnt stamina.

`AffectQuantity` (float)
* How much burnt stamina is affected

`IsModifier` (boolean)
* Should this be applied as a modifier (multiplier) value?

## SL_AffectCorruption : SL_Effect
For affecting the character's Corruption stat (The Soroboreans DLC).

`AffectQuantity` (float)
* The amount corruption is affected by

`IsRaw` (boolean)
* Is this value "Raw"?

## SL_AffectDrink : SL_Effect
For affecting the player's Thirst/Drink stat.

`AffectQuantity` (float)
* The amount the player's thirst is affected by

## SL_AffectFatigue : SL_Effect
For affecting the player's Fatigue (Rest) stat.

* `AffectQuantity` (float)
* The amount the player's fatigue is affected by.

## SL_AffectFood : SL_Effect
For affecting the player's hunger (Food) stat.

`AffectQuantity` (float)
* The amount the player's hunger is affected by.

## SL_AffectHealth : SL_Effect
For restoring or affecting current Health.

`AffectQuantity` (float)
* How much current health is affected

`IsModifier` (boolean)
* Should this be applied as a modifier (multiplier) value?

`AffectQuantityOnAI`
* If you want a separate value for AI, set that here. Otherwise set to -1.

## SL_AffectHealthParentOwner : SL_Effect
For healing the "owner" of the effect. Used by Blood Bullet, for example.

`AffectQuantity` (float)
* How much current health is affected

`IsModifier` (boolean)
* Should this be applied as a modifier (multiplier) value?

`Requires_AffectedChar` (boolean)
* Does it require a successful affected character (ie. on-hit effect, etc)?

## SL_AffectMana : SL_Effect
For affecting current Mana.

`AffectQuantity` (float)
* How much current mana is affected

`IsModifier` (boolean)
* Should this be applied as a modifier (multiplier) value?

`AffectType` (enum)
* Determines whether it restores or uses mana.
* One of: `Restaure` or `Use`

## SL_AffectStability : SL_Effect
For affecting the owner character's stability. Used by Juggernaught, for example.

`AffectQuantity` (float)
* How much current stability is affected

`SetStability` (boolean)
* Should this be applied as a direct value? (ie, sets Stability to this exact value)

## SL_AffectStamina : SL_Effect
For affecting current stamina.

`AffectQuantity` (float)
* How much current stamina is affected

## SL_AffectStat : SL_Effect
For affecting a character's <b>Stat</b> attribute for the given `Stat_Tag`.

* See [this google sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1840819680) for a list of tags.
* Stat Tags are mostly between IDs <b>76</b> to <b>125</b>, but they are elsewhere too, look around.
* Don't include the number or hyphen, just the tag. Ie, just put "Weapon", not "1-Weapon".

`Stat_Tag` (string)
* The affected stat tag

`AffectQuantity` (float)
* The quantity the stat is affected by

`IsModifier` (boolean)
* Should this be applied as a modifier (multiplier) value?

`Tags` (list of string)
* Extra Tag restrictions on the affected Stat. This is used in special cases like Damage Bonuses that only affect a certain type of skill or effect.

## SL_AffectStatusEffectBuildUpResistance : SL_Effect
Inherits from `SL_Effect`. Adds Status Effect Build Up Resistance for a specific Status Effect to the character.

`StatusEffectIdentifier` (string)
* The Status Effect Identifier to add resistance for

`Value` (float)
* The amount of resistance for this effect

`Duration` (float)
* How long does the resistance last?

## SL_AutoKnock : SL_Effect
This effect is quite simple, it just guarantees a stagger or knockdown on the affected character. Use HitEffects to apply to enemies.

`KnockDown` (boolean)
* Should the affected character be knocked down, or just staggered?

## SL_CallSquadMembers : SL_Effect
This class uses no fields, other than the base SL_Effect fields.

This is used for AI (non-player) enemies to call over their squad members, it has no effect when used by a player character.

## SL_Cough : SL_Effect
Used by the "Cold" disease, interrupts the player's actions. 

`ChanceToTrigger` (integer)
* Value from 0 to 100, what is the general chance for the Cough to trigger when this effect is activated?

## SL_CreateItemEffect : SL_Effect
Used to add an item to the caster's inventory.

`ItemToCreate` (int)
* The Item ID to create

`Quantity` (int)
* Quantity of the item received

## SL_CureDisease : SL_Effect
This effect inherits from `SL_RemoveStatusEffect`. There are no custom fields on this effect other than the ones inherited from SL_RemoveStatusEffect, it's just that the game knows you are removing a Disease instead of a regular Status Effect.

## SL_Death : SL_Effect
Another simple effect, the name tells you what happens. Guaranteed death.

## SL_DetachParasite : SL_Effect
Used by enemy AIs. Probably related to some DLC content.

## SL_GiveOrder : SL_Effect
Used by enemy AIs, related to DLC content.

`OrderID` (integer)
* The "order" given to AI squad members.

## SL_ImbueObject : SL_Effect

Abstract base class used by SL_ImbueWeapon and SL_ImbueProjectile.

`Lifespan` (float)
* Lifespan of the imbue from this source

`ImbueEffect_Preset_ID` (integer)
* The ID of the Imbue Preset you want to apply.
* You can find preset IDs with [this google sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1969601658) - use the "ID" of the effect.

## SL_ImbueWeapon : SL_ImbueObject
Inherits from SL_ImbueObject, for adding an <b>Imbue Preset</b> to a character's main weapon (and dagger).

`Imbue_Slot` (enum)
* Must be exactly one of: `MainHand` or `OffHand` 
* The slot which this imbue preset will be applied to.

## SL_ImbueProjectile : SL_ImbueObject
For applying an Imbue to a projectile.

`UnloadProjectile` (boolean)
* Unload the current projectile?

## SL_InstrumentTriggerBubble : SL_Effect
Used by Nurturing Echo to trigger the healing effect on nearby instruments.

`Range` (float)
* The max range of instruments to trigger healing for

## SL_LearnSkillEffect : SL_Effect

Simply teaches the player character a skill.

`SkillID` (int)
* The Item ID of the skill to teach.

## SL_LightLantern : SL_Effect
This effect lights or un-lights your lantern, if equipped.

`Light` (boolean)
* Should this light the lantern? `false` would un-light the lantern.

## SL_LoadWeapon : SL_Effect
This effects loads a weapon (ie. Pistol).

`UnloadFirst` (boolean)
* Should it unload the weapon before loading?

`WeaponSlot` (enum)
* Which slot to affect. Must be exactly one of: `MainHand` or `OffHand`

## SL_PackDeployable : SL_Effect
Used by Reverberation to pack instruments back to their packed state prefab.

Contains no fields, but requires a `SL_PrePackDeployableCondition` SL_EffectCondition on the same transform.

## SL_Petrify : SL_Effect
Used by Full Petrification to Petrify the target character. Contains no extra fields.

## SL_PlayAnimation : SL_Effect
This is a custom effect used to manually trigger an animation on the affected character.

`Animation` (enum)
* The animation to play.
* Must be one of [these values](API/Enums/SpellCastType.md).

`Modifier` (enum)
* A modifier on the character when the animation plays.
* Must be exactly one of: `Immobilized`, `Mobile`, `Attack` or `NONE`

`SheatheRequired` (integer)
* Sets the type of weapon sheathing behavior when the animations plays
* `0` or `-1` = not set, do nothing
* `1` = unsheathe required
* `2` = sheathe required

## SL_PlaySoundEffect : SL_Effect
This one is obviously for playing a sound effect. You can pick any sound from the global list.

`Sound` (GlobalAudioManager.Sounds)
* Pick any sound of [this list](API/Enums/Sounds.md).

`Follow` (boolean)
* Should the sound follow the affected character?

`MinPitch` (float)
* Minimum pitch (from distance)

`MaxPitch` (float)
* Maximum pitch (from distance)

## SL_PlayVFX : SL_Effect
Used to play a VFX System (visual effects).

`VFXPrefab` (SL_PlayVFX.VFXPrefabs)
* Can pick any of [these values](API/Enums/VFXPrefabs.md)
* Determines the VFX prefab that will play

`HitPos` (boolean)
* Determines whether the VFX will play at the position provided by the effect activation

`ParentMode` (enum)
* Must be exactly one of: `This` or `FXWorld`
* If `FXWorld`, it will set the parent to the global FX transform holder

`DontInstantiateNew` (boolean)
* If `false`, it will instantiate a new prefab each activation

## SL_PlayTimedVFX : SL_PlayVFX
Inherits from `SL_PlayVFX`, and just contains one extra field which allows you to stop the VFX after a delay.

`AutoStopTime` (float)
* Delay in seconds after which the effects will be stopped. If 0 or less, VFX will play until it stops itself (maybe never).

## SL_Puke : SL_Effect
Similar to SL_Cough, used by the Indigestion disease.

`ChanceToTrigger` (integer)
* The chance for the Puke to trigger when this effect is activated.

## SL_PunctualDamage : SL_Effect
PunctualDamage is a type of damage applied by skills or effects. It requires no weapon.

The `Damage` is a list of SL_Damage objects. This is similar to the `BaseDamage` field on a SL_Weapon template.
* Each entry has a `Damage` and `Type` value.
* For the "Type", you must set one of: `Physical`, `Ethereal`, `Decay`, `Electric` (<b>not</b> Lightning), `Frost`, or `Fire`.

The XML should look like:
```xml
<Damage>
  <SL_Damage>
    <Damage>20</Damage>
    <Type>Physical</Type>
  </SL_Damage>
  <!-- etc... -->
</Damage>
```

`Damages_AI` (list of SL_Damage)
* The same as `Damage`, but its separate values applied to AI characters. You don't have to set this.

`Knockback` (float)
* The Impact damage of the effect

`HitInventory` (boolean)
* Does this effect damage the durability of items in the affected character's backpack?

## SL_PunctualDamageAoE : SL_PunctualDamage
This class inherits from SL_PunctualDamage, and contains a few extra fields.

`Radius` (float)
* Radius of the AoE damage

`TargetType` (enum)
* Must be one of: `Allies` or `Enemies`

`IgnoreShooter` (boolean)
* Should the shooter be ignored?

`IgnoreHalfResistances` (bool)
* Does this attack ignore 50% of target's resistances?

## SL_PunctualDamageInstrument : SL_PunctualDamage
Inherits from `SL_PunctualDamage`, and contains two extra fields.

`DamageCap` (float)
* Max damage that can be dealt

`DamagePerCharge` (float)
* Damage gained per charge on the instrument (overrides damage values on base PunctualDamage)

## SL_PutBackItemVisual : SL_Effect
Special effect used by certain skills, contains no extra fields.

## SL_ReduceDurability : SL_Effect
Simple effect which reduces durability of some equipment.

`Durability` (float)
* How much durability is lost (flat amount)

`EquipmentSlot` (enum)
* Which slot to affect. Must be one of [these values](API/Enums/EquipmentSlotIDs.md).

`Percentage` (bool)
* Does this effect reduce durability by a %, not a flat value?

## SL_ReduceStatusLevel : SL_Effect
For Status Effects which <b>levels</b> (eg the new Alertness status in the DLC).

`StatusIdentifierToReduce` (string)
* The <b>Identifier Name</b> of the Status Effect.
* See [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1969601658) for a list of Status Effect Identifiers.

## SL_RemoveImbueEffects : SL_Effect
Simply removes the Imbue from a given Equipment Slot.

`AffectSlot` (enum)
* Must be one of [these values](API/Enums/EquipmentSlotIDs.md).

## SL_RemoveStatusEffect : SL_Effect
Used to remove a status effect from the affected character.

You can either use the <b>Identifier Name</b> of the status, or a <b>Tag</b> that the status might have on it.

`Status_Name` (string)
* <b>Only valid if you are NOT using `StatusType` for the CleanseType!</b>
* If you want to remove a specific status by the identifier, set this.
* For a list of Statuses listed by their Identifier, see [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1969601658)

`Status_Tag` (string)
* <b>Only valid if you ARE using `StatusType` for the CleanseType!</b>
* If there's a tag you want to search for on the status and remove all statuses with that tag, set it here.
* See [this google sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1840819680) for a list of tags.

`CleanseType` (enum)
* Exactly one of: `StatusNameContains`, `StatusType`, `StatusFamily`, `StatusSpecific` or `NegativeStatuses`
* I recommend using <b>StatusNameContains</b> and using the <b>Status_Name</b> field, or <b>StatusType</b> and using the <b>Status_Tag</b> field.
* The `NegativeStatuses` value will remove ALL negative (purgeable) effects.

## SL_RunicBlade : SL_Effect
This effect is uniquely used by the Runic Blade skill. However, it could be used for other purposes too.

`WeaponID` (integer)
* The Weapon ID which should be summoned for the player.

`GreaterWeaponID` (integer)
* If the player has Arcane Syntax, the weapon from the amplified version of the spell.

`PrefixImbueID` (integer)
* If the player has Runic Prefix, the Preset ID of the Imbue you want to give for the WeaponID weapon.

`PrefixGreaterImbueID` (integer)
* If the player has Runic Prefix, the Preset ID of the Imbue you want to give for the GreaterWeaponID weapon.

## SL_Shooter : SL_Effect
The base class used by SL_ShootBlast and SL_ShootProjectile. You cannot use this class directly.

ShootBlast and ShootProjectile are rather complex effects, they are almost as involved as a Skill or Item by itself.

`CastPosition` (enum)
* Determines the cast-position type.
* Must be exactly one of: `Character`, `Given`, `Local`, `ProximitySkillItem`, `Target` or `Transform`.

`LocalPositionAdd` (Vector3)
* Adds position to the start-point of the shooter projectile or blast.
* A Vector3 has `x`, `y` and `z` offsets you can set.

`NoAim` (boolean)
* Force "no-aim" behaviour.

`TargetType` (enum)
* Determiens the target-type behaviour.
* Must be exactly one of: `Allies` or `Enemies`

## SL_ShootBlast : SL_Shooter
ShootBlast inherits from SL_Shooter, and contains some extra fields.

`BaseBlast` (enum)
* This determines the base visuals for your blast.
* You can pick any value from [this list](API/Enums/BlastPrefabs.md) (there are a lot).

`Radius` (float)
* The radius of the blast. Generally between 1 to 5.

`RefreshTime` (float)
* How frequently the blast refreshes (re-applies damage/effects).

`BlastLifespan` (float)
* Lifespan of the blast, generally 0.1 for one-and-done blasts.

`InstantiatedAmount` (integer)
* Determines the <b>maximum copies of the blast</b> which can be active at once.
* Think about how many you would need (per caster) at any one time, and set to that.

`Interruptible` (boolean)
* Can the blast be interrupted?

`MaxHitTargetCount` (integer)
* Maximum targets that can be hit by the blast, if any.

`AffectHitTargetCenter` (boolean)
* Should the blast affect the target from their center position?

`HitOnShoot` (boolean)
* Should the hit effects be applied automatically on shoot?

`IgnoreShooter` (boolean)
* Should the shooter be ignored?

`IgnoreStop` (boolean)
* Should stopping be ignored?

`NoTargetForwardMultiplier` (float)
* If the shooter has no target, this increases the forward speed.

`ParentToShootTransform` (boolean)
* Determines the shooting behaviour, should the parent transform be the shooter?

`UseTargetCharacterPositionType` (boolean)
* Should the target character's position-type be used for the targeting mode?

`ImpactSoundMaterial` (enum)
* The sound on impact.
* Must be one of [these values](API/Enums/EquipmentSoundMaterials.md).

`DontPlayHitSound` (boolean)
* Whether to play the hit sound on hit

`FXIsWorld` (boolean)
* Are the effects "world effects"?

`EffectBehaviour` (enum)
* Determines the behaviour for your BlastEffects.
* Must be exactly one of: `Override`, `Destroy` or `NONE`.

`BlastEffects` (List of SL_EffectTransform)
* List of actual effects on the blast.
* See [the Effects article](API/SL_EffectTransform) for more information.

`PlaySounOnRefresh` (bool)
* Play sound each refresh tick?

`DelayFirstShoot` (bool)
* Delay the first shot?

`PlayFXOnRefresh` (bool)
* Play VFX on refresh tick?

## SL_ShootBlastHornetControl : SL_ShootBlast
Inherits from `SL_ShootBlast`. Used by Hive AI enemies to shoot a lingering blast which tracks to the target.

<b>Notes:</b>
* Set the `BlastLifespan` (a ShootBlast field) to control how long the effect lasts. 
* You should only have an `InstantiatedAmount` of <b>1</b> at most. This effect doesn't expect more than 1 at a time.

`BurstSkillID` (int)
* The skill Item ID for the burst (detonation) effect

`HealSkillID` (int)
* The skill Item ID for the heal effect (used by the AI)

`Acceleration` (float)
* The acceleration speed of the tracking

`Speed` (float)
* The default speed of the tracking

`SpeedDistLerpWhenCloseMult`
* Multiplier to lerp the speed as it approaches the target

`DistStayOnTarget` (float)
* Distance at which the effect considers itself to be 'on' the target

`EndEffectTriggerDist` (float)
* The radius of the 'end effects' blast (for the effect to know when to detonate, at end of life)

`EnvironmentCheckRadius` (float)
* Checks for collision within this radius in the environment

`TimeFlight` (float)
* Maximum time to be in flight with no target in range (I think?)

`TimeStayOnTarget` (float)
* Maximum time to stay on the target

`PassiveTimeFlight` (float)
* Max flight time when in 'passive effect' mode (I'm not sure what this really means)

`PassiveTimeStayOnTarget` (float)
* Maximum time to stay on target in 'passive effect' mode

`HornetLookForTargetRange` (float)
* Range which the effect will look for targets in

`HornetPassiveAttackTimer` (float)
* Timeout on the passive attacks?

`HornetPassiveTargetRange` (float)
* Max range of the passive attacks

## SL_ShootBlastsProximity : SL_ShootBlast
Inherits from `SL_ShootBlast`.

Contains no extra fields, this works by having a `SL_ProximityCondition` EffectCondition on the same Effect Transform.

## SL_ShootEnchantmentBlast : SL_ShootBlast
Inherits from `SL_ShootBlast`.

Used for Enchantments which add an AoE on-hit blast. The Damage is based on the weapon used.

`DamageMultiplier` (float)
* The multiplier of the weapon damage, which determines the damage of the blast
* Generally between 0 and 1.

## SL_ShootProjectile : SL_Shooter
SL_ShootProjectile inherits from SL_Shooter. It is similar to ShootBlast, this shoots a projectile with a direction and velocity.

`BaseProjectile` (enum)
* This determines the base visuals for your projectile.
* You can pick any value from [this list](API/Enums/ProjectilePrefabs.md) (there are a lot).

### SL_ProjectileShot
`ProjectileShots` (list of SL_ProjectileShot)

Each SL_ProjectileShot is an actual projectile produced by this effect. You need at least 1 of these, but can have as many as you want. Each shot can have its own offsets and directions applied to it.

For example:
<!-- tabs:start -->
#### ** XML **

```xml
<ProjectileShots>
  <SL_ProjectileShot>
    <LocalDirectionOffset>
      <x>0</x>
      <y>1.5</y>
      <z>0</z>
	  </LocalDirectionOffset>
	  <!-- etc... -->
  </SL_ProjectileShot>
  <MustShoot>false</MustShoot>
  <!-- etc... -->
</ProjectileShots>
```

#### ** C# **

```csharp
ProjectileShots = new SL_ProjectileShot[]
{
  new SL_ProjectileShot
  {
    LocalDirectionOffset = Vector3.one,
    MustShoot = false,
    // etc...
  },
  // etc...
}
```
<!-- tabs:end -->

The fields on SL_ProjectileShot are:

`LocalDirectionOffset` (Vector3)
* Custom direction offset for this projectile

`LockDirection` (Vector3)
* Custom locked direction for this projectile

`MustShoot` (boolean)
* Should this shot be forced to shoot?

`NoBaseDir` (boolean)
* Whether to override the base direction or not

`RandomLocalDirectionAdd` (Vector3)
* Optional random local direction added

### SL_ShootProjectile fields
Back to the other fields on SL_ShootProjectile...

`InstantiatedAmount` (int)
* This is the limit on how many TOTAL projectiles can be active at one time. It does not determine how many shots are created, that is done by the SL_ProjectileShots.
* Think about how many could possibly need to be active at one time (probably in regards to the cooldown of the spell) and set this to that. Should at least be higher than the amount of ProjectileShots you defined.

`Lifespan` (float)
* Maximum lifespan of the Projectile

`LateShootTime` (float)
* Delay on the projectile shot

`Unblockable` (boolean)
* Is the projectile <b>unblockable</b>?

`EffectsOnlyIfHitCharacter` (boolean)
* Only do effects if the projectile actually hit a targetable character?

`EndMode` (enum)
* Determines the end-mode behaviour
* Must be exactly one of: `LifetimeOnly`, `EnvironmentOnly`, `Normal` or `Custom`

`DisableOnHit` (boolean)
* Should the projectile be disabled once it hits something?

`IgnoreShooterCollision` (boolean)
* Should the shooter collision be ignored?

`TargetingMode` (enum)
* Determines the targeting mode for the projectile
* Must be exactly one of: `FindAllies`, `FindEnemies`, `IgnoreTarget`, `Owner`, `OwnerTarget`, `TargetChar` or `NONE`.

`TargetCountPerProjectile` (integer)
* Per projectile shot, how much targets can each projectile have?

`TargetRange` (float)
* Maximum target-acquisition range.

`AutoTarget` (boolean)
* Should the projectile acquire a target automatically? (No lock-on)

`AutoTargetMaxAngle` (float)
* Maximum angle for auto-targeting, if enabled

`AutoTargetRange` (float)
* Maximum range for auto-targeting, if enabled

`ProjectileForce` (float)
* Force applied to the projectile

`AddDirection` (Vector3)
* Optional extra direction added to projectile

`AddRotationForce` (Vector3)
* Optional extra rotation added to projectile

`YMagnitudeAffect` (float)
* Magnitude that the Y axis is affected by

`YMagnitudeForce` (float)
* Force applied to Y magnitude

`DefenseLength` (float)
* Length of defense (blocking) against this projectile?

`DefenseRange` (float)
* Range of blocking against this projectile?

`ImpactSoundMaterial` (enum)
* The sound on impact.
* Must be one of [these values](API/Enums/EquipmentSoundMaterials.md).

`LightIntensityFade` (Vector2)
* Fade applied to the Light on the projectile, if any.
* Only has an `x` and `y` value

`PointOffset` (Vector3)
* Offset of the Point-Light, if any.
* Also has a `z` value.

`TrailEnabled` (boolean)
* Is the trail enabled on the projectile (if any)?

`TrailTime` (float)
* If trail enabled and valid, the time of the trail.

`EffectBehaviour` (enum)
* Determines the behaviour of your ProjectileEffects.
* Must be exactly one of: `Destroy`, `Override` or `NONE`.

`ProjectileEffects` (list of SL_EffectTransform)
* List of actual effects on the projectile.
* See [the Effects article](API/SL_EffectTransform) for more information.

`CameraAddYDirection` (float)
* Adds Y based on Camera direction (?)

## SL_ShootProjectileAmmoVisuals : SL_ShootProjectile
Inherits from `SL_ShootProjectile` and contains no extra fields, used by Bow Skills.

## SL_ShootItem : SL_ShootProjectile

Inherits from `SL_ShootProjectile`, and has no extra fields.

This effect <b>requires</b> a `SL_WeaponLoadoutItem` [SL_ItemExtension](API/SL_ItemExtension?id=sl_weaponloadoutitem) on the Item to support this effect.

Used for Bows.

## SL_ShootProjectilePistol : SL_ShootProjectile
Inherits from ShootProjectile, and is used for pistol skills.

`UseShot` (boolean)
* Should the loaded shot be used, if any?

## SL_StartDuel : SL_Effect
This simple effect will change the affected character's faction to `Bandits`.

This is uniquely used by the Duel Baton item, and contains no fields.

## SL_Stun : SL_Effect
Used by the Sweep Kick skill (if target has Confusion).

`Duration` (float)
* Duration of the stun.

## SL_SpawnSLCharacter : SL_Effect
The `SL_SpawnSLCharacter` class is a custom non-game class which can be used to spawn an [SL_Character](API/SL_Character) from an effect. It can also make the character follow the caster of the effect.

`SLCharacter_UID` (string)
* The SL_Character template UID which you want to spawn

`GenerateRandomUIDForSpawn` (boolean)
* If true, will generate a unique UID for each spawned character, allowing you to re-use the template for dynamic spawns.

`TryFollowCaster` (boolean)
* If true, will try to follow the person who created this effect. This <b>requires</b> you to set the `Wander_Type` on the SL_CharacterAI to `Follow`. You should also set `Wander_FollowPlayer` to <b>false</b> if you are using this.

`SpawnOffset` (Vector3)
* Vector3 Position offset for character spawn position

## SL_Summon : SL_Effect
This effect type is used by skills like Conjure (Summoned Ghost), but also for things like placing a Sigil on the ground, or casting a Runic Trap, etc.

`Prefab` (string)
* The Prefab to load. Depends on the SummonPrefabType that you set.
* If SummonPrefabType set to `Resource`, it will try to load the Prefab as a path in the Resources folder.
* If set to `Item`, it will assume the Prefab is an Item ID.

`SummonPrefabType` (enum)
* Determines whether the summoned object is from the Resources folder, or it's an Item.
* Must be exactly one of `Resource` or `Item`.

`BufferSize` (integer)
* How many can be summoned at once?
* Only valid if SummonMode is `Buffer`.

`LimitOfOne` (boolean)
* Can only one be active at a time?

`SummonMode` (enum)
* Determines some behaviour of the summoning prefab.
* Must be exactly one of: `SameObject`, `CreateNew` or `Buffer`.

`PositionType` (enum)
* Determines the positioning behaviour
* Must be exactly one of: `InFrontOfTarget`, `AroundTarget`, `ProximitySkillItem` or `ProximitySoulSpot`.

`MinDistance` (float)
* Minimum distance of summon from caster (only at time of cast)

`MaxDistance` (float)
* Max distance of summon from caster (only at time of cast)

`SameDirectionAsSummoner` (boolean)
* Summoned prefab has same facing direction as caster?

`SummonLocalForward` (Vector3)
* Optional extra position added to Summoned prefab

`IgnoreOnDestroy` (boolean)
* Does the effect ignore when prefab is destroyed?

## SL_SummonAI : SL_Summon
This effect inherits from SL_Summon. It contains no extra fields, it just tells the game you are summoning an AI Character which will follow the caster.

## SL_SummonBloodSpirit : SL_SummonAI
Like SL_SummonAI, but it tells the game you are summoning a "Blood Spirit" (probably DLC-related).

## SL_Teleport : SL_Effect
Teleports the caster.

`MaxRange` (float)
* Maximum teleport range.

`MaxTargetRange` (float)
* If using target-teleport, maximum distance of target.

`MaxYDiff` (float)
* If using target-teleport, maximum Y (height) difference.

`OffsetRelativeTarget` (Vector3)
* Offset position to target

`UseTarget` (boolean)
* Do we teleport to the current target?

## SL_ThrowItem : SL_ShootProjectile
<b>NOTE:</b> Must be on an `SL_ThrowSkill` (eg. cloned from Throw Lantern skill). Inherits from SL_ShootProjectile.

`CollisionBehaviour` (enum)
* Determines the collision behaviour
* Must be exactly one of: `Destroyed`, `None` or `Stuck`.

`ProjectileBehaviour` (enum)
* Determines how the projectile itself behaves
* Must be exactly one of: `RayCast` or `Physic`

## SL_UnloadWeapon : SL_Effect
Simply unloads the weapon in the given AffectSlot.

`AffectSlot` (enum)
* The equipment slot to unlaod from.
* Must be exactly one of [these values](API/Enums/EquipmentSlotIDs.md).

## SL_UseLoadoutAmunition : SL_Effect
Uses ammunition from a WeaponLoadout.

`MainHand` (boolean)
* Should we affect the main-hand item, or off-hand?

`AutoLoad` (bool)
* Does this automatically load ammo for you?

`DestroyOnEmpty` (bool)
* Is it destroyed when empty?

## SL_VitalityDamage : SL_Effect
Damage dealt directly to the affected character's health, <b>cannot be resisted</b>.

`PercentOfMax` (float)
* Percent of character's health that is lost.

## SL_WeaponDamage : SL_PunctualDamage

This is a type of SL_PunctualDamage that <b>requires an equipped weapon</b>, used by Attack Skills. It bases the damage off the required weapon types for the attack skill it is placed on.

This inherits all the fields on SL_PunctualDamage, and also has:

`OverrideType` (enum)
* Used to override the weapon's damage to a certain element
* You can set this to `COUNT` to bypass any override.
* Otherwise, must be one of: `Physical`, `Ethereal`, `Decay`, `Electric` (<b>not</b> Lightning), `Frost`, or `Fire`.

`ForceOnlyLeftHand` (boolean)
* If true, it will ignore the damage on any required main-hand weapons, if there are any.

`Damage_Multiplier` (float)
* Multiplies the weapon damage by this number

`Damage_Multiplier_Kback` (float)
* If you want special multipliers when the target is knocked back, set this.
* Set to -1 to use Damage_Multiplier

`Damage_Multiplier_Kdown` (float)
* Same as Damage_Multiplier_KBack, but for knockdowns instead.

You also have `Impact_Multiplier`, `Impact_Multiplier_Kback` and `Impact_Multiplier_Kdown` which are the same but they're for impact damage.

## SL_WeaponDamageFlurry : SL_WeaponDamage

Inherits from SL_WeaponDamage, used by Prismatic Flurry.

`HitDelay` (float)
* The time delay between each hit.

## SL_WeaponDamageOwnerHPStam : SL_WeaponDamage

Inherits from SL_WeaponDamage, contains no extra fields. Used by some of the new Weapon Master skills to amplify damage based on our HP and Stam.

## SL_WeaponDamageProjectileWeapon : SL_WeaponDamage

Inherits from SL_WeaponDamage, contains no extra fields. Used by ProjectileWeapon skills.

## SL_WeaponDamageStatusOnKill : SL_WeaponDamage

Inherits from SL_WeaponDamage, grants a Status Effect if this weapon damage deals a killing blow.

`StatusIdentifier` (string)
* The Status Identifier of the effect gained on kills

## SL_WeaponDamageTargetHealth : SL_WeaponDamage
Inherits from SL_WeaponDamage, gains more damage multiplier depending on target's Health % ratio.

`MultiplierHighLowHP` (Vector2)
* Multiplier for target health ratio. `x` is 100% multiplier, `y` is 0% multiplier, value is lerped depending on target health.