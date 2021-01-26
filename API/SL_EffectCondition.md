# SL_EffectCondition

An <b>SL_EffectCondition</b> object can be defined on an [Effect Transform](Effect-Transforms). 

These conditions will determine whether [SL_Effects](API/SL_Effect) on the same transform will execute or not.

## SL_EffectCondition Fields

The only base field which is shared by all EffectConditions is:

`Invert` (boolean)
* If true, it will flip the result of the condition after evaluating.
* Used if your condition produces the opposite behaviour than intended.
* Also used by "XOR" (exclusive-or, or mutually-exclusive) conditions, with one inverted and one not inverted.

# SL_EffectCondition sub-classes

These are all the subclasses of SL_EffectCondition. They all contain the Invert field above.

## SL_AngleFwdToTargetFwdCondition : SL_EffectCondition
Used by the Backstab skill. Compares your forward-angle to the target's forward-angle.

`AnglesToCompare` (List of Vector2)
* A list of valid angles.
* For backstab, it uses one Vector2: `<x>100</x> <y>190</y>`.

## SL_AttackTypeCondition : SL_EffectCondition
Checks the last Attack ID used by the casting character.

`AffectOnAttackIDs` (list of integer)
* A list of integer (`int`) values, corresponding to accepted Attack IDs.
* Attack IDs are between 0 and 5.

## SL_BooleanCondition : SL_EffectCondition
This can be used to set a "hard-coded" condition check for the whole transform. It will always return whatever value you set.

This could be used to invert an entire transform without manually changing each condition, I guess.

`Valid` (boolean)
* True to do nothing, false to flip the whole effect transform.

## SL_ContainedWaterCondition : SL_EffectCondition
A condition used to check the type of water in a waterskin, or any waterskin maybe?

`ValidWaterType` (enum)
* The water type to check.
* Must be exactly one of: `Clean`, `Fresh`, `Salt`, `Rancid` or `Magic`.

## SL_CorruptionLevelCondition : SL_EffectCondition

Requires a certain level of Corruption on the character.

`Value` (float)
* Between 0 and 1000 (1000 = 100% Corruption).

`CompareType` (enum)
* Determines the way the value is checked
* Must be one of: `Equal`, `GEqual`, `Greater`, `LEqual`, `Less`, `NotEqual`

## SL_CounterSuccessCondition : SL_EffectCondition
<b>Note: Must be applied to an `SL_CounterSkill`</b>.

Checks if the last counter on this CounterSkill was a success, if so it evaluates to true.

## SL_DealtDamageCondition : SL_EffectCondition
Checks if the damage dealt contains a certain type.

`RequiredDamages` (list of SL_Damage)
* The valid damage types to check for
* Determines the damage types and minimum values
* This value is a list of `SL_Damage` objects. Each SL_Damage contains `Type` and a `Damage` value. See documentation on PunctualDamage or Weapons for more details.

## SL_DelayEffectCondition : SL_EffectCondition
Not a real condition check, just adds delay to the effects.

`Delay` (float)
* The delay. Value-unit depends on the DelayType you set.

`DelayType` (enum)
* The type of delay.
* Must be exactly one of: `GameTimeHour` or `RealTimeSecond`.

## SL_EquipDurabilityCondition : SL_EffectCondition
Checks an equipment slot, and checks if it has enough durability.

`EquipmentSlot` (enum)
* The equipment slot to check.
* Must be exactly one of [these values](API/Enums/EquipmentSlotIDs.md).

`MinimumDurability` (float)
* The minimum durability value (flat).

## SL_HasHealthCondition : SL_EffectCondition
To check the current health of the character, by default it checks that they are above this value, use Invert to check if they are below it.

`Value` (float)
* The value to check for, either a direct value or a percentage if CheckPercent is true

`CheckPercent` (boolean)
* If true, your Value will be used as a percentage value. Eg, 50 would check that they have 50% or more health.

`IgnoreBurntForPercent` (boolean)
* If true, your percentage Value will be based on the character's active max health, ignoring burnt health.

## SL_HasQuantityItemsCondition : SL_EffectCondition
Checks if the affected character owns a total number of items (any item).

`TotalItemsRequired` (integer)
* The total items required.

## SL_HasStatusEffectEffectCondition : SL_EffectCondition
Checks if the affected character has a Status Effect or not.

`DiseaseAge` (float)
* If checking for a disease, the minimum age of the disease.

`CheckOwner` (boolean)
* Check the owner?

`StatusSelectorType` (enum)
* Determines how your `SelectorValue` is used.
* Must be exactly one of: `StatusSpecific` (Status Identifier), `StatusType` (Status Tag) or `StatusFamily` (Status Family UID).

`SelectorValue` (string)
* Depends on your StatusSelectorType
* If `StatusSpecific`, checks for a Status Identifier of this name, meaning it checks the `SL_StatusEffect.StatusIdentifier` for an exact match.
* If `StatusType`, checks for a status with this Tag. This means it checks the `SL_StatusEffect.Tags` for one or more matches.
* If `StatusFamily`, checks for a status family with this UID, which can be either the Status Effect's Bind or Reference family (depending on family mode).

## SL_HasStatusLevelCondition : SL_EffectCondition
Similar to HasStatusEffectEffectCondition, but checks for a Level-Status Effect (like Alertness).

`StatusIdentifier` (string)
* The Status Identifier name to check for

`CompareLevel` (integer)
* The level required

`CheckOwner` (boolean)
* Whether to check on the owner or not

`ComparisonType` (enum)
* The behaviour for the comparison evaluation
* Must be exactly one of: `Equal`, `Greater`, `Less`, `NotEqual`, `GEqual` or `LEqual`.

## SL_HeightCondition : SL_EffectCondition
Checks the height of the character (relative to nearest ground? or just in general? I don't know).

`AllowEqual` (bool)
* Allow == and not just >= ?

`CompareType` (enum)
* The behaviour of the equality check
* Must be exactly one of: `Higher` or `Lower`

`HeightThreshold` (float)
* The height requirement. Don't know if relative or just based on "sea level".

## SL_InTownOrCityCondition : SL_EffectCondition
Checks if the character is in a town or city, contains no extra fields.

## SL_ImbueEffectCondition : SL_EffectCondition
Checks if the affected character has an imbue.

`ImbuePresetID` (integer)
* The Preset ID of the Imbue required.

`AnyImbue` (boolean)
* Will any imbue work, or only the one with the PresetID set?

`WeaponToCheck` (enum)
* Which slot to check?
* Must be exactly one of: `MainHand` or `OffHand`

## SL_ImbueEffectORCondition : SL_EffectCondition
Same as ImbueEffectCondition, but allows you to check from a list of valid Imbues, and not just one.

It is exactly the same, but instead of `ImbuePresetID` you have `ImbuePresetIDs` (a list of integer).

## SL_InBoundsCondition : SL_EffectCondition
Checks if the character is within a certain `Bounds` value, in absolute world space.

`Bounds` (Bounds)
* A Bounds struct has the fields: `center` (Vector3) and `size` (Vector3), use these to set the bounds, ignore the other values.

## SL_InstrumentClose : SL_EffectCondition
Requires an Instrument item to be within a certain distance.

`MainInstrumentID` (int)
* The item ID of the primary instrument required

`OtherInstrumentIDs` (list of int)
* A list of ints for other accepted Instrument Item IDs

`Range` (float)
* Max distance from affected character for the instrument to be

## SL_InZoneCondition : SL_EffectCondition
I'll be honest, I don't really know how this one works. It might not actually be a real condition that is used.

`Radius` (float)
* Radius of zone to check? Based on what?

## SL_IsEquipSlotFilledCondition : SL_EffectCondition
Checks if the given EquipmentSlot has something equipped or not.

`EquipmentSlot` (enum)
* The slot to check.
* Must be one of [these values](API/Enums/EquipmentSlotIDs.md).

## SL_IsWorldHostCondition : SL_EffectCondition
Self-explanatory condition. Is the affected character the world host or not? This condition has no extra fields.

## SL_MostRecentCondition : SL_EffectCondition
Used by Runic spells to check which of two status effects is most recent.

`StatusIdentifierToCheck` (string)
* The identifier of the status which should be more recent

`StatusIdentifierToCompareTo` (string)
* The identifier of the status you are comparing to (which should be older)

## SL_NoSameShooterBlastInProximity : SL_EffectCondition
Prevents multiple of the same shooter from creating this effect in a certain range.

`Proximity` (float)
* The range for only 1 of the shooter to be active inside.

## SL_OwnsItemCondition : SL_EffectCondition
Checks if the affected character owns an amount of a specific Item ID.

`ReqItemID` (integer)
* The Item ID required

`ReqAmount` (integer)
* Required amount of this item

## SL_PassiveSkillCondition : SL_EffectCondition
Checks if the affected character has a passive skill of this Item ID.

`ReqSkillID` (integer)
* The required passive skill ID.

## SL_PrePackDeployableCondition : SL_EffectCondition
Requires a packable Instrument to be within a certain distance.

`ProximityDist` (float)
* The distance for the instrument to be

`ProximityAngle` (float)
* Max angle relative to player's facing direction that we check inside

`PackableItemIDs` (list of int)
* A list of ints for the accepted Instrument Item IDs

## SL_ProbabilityCondition : SL_EffectCondition
A random condition. Has a chance to evaluate true or false.

`ChancePercent` (integer)
* Chance from 0 to 100 to evaluate to true.
* Eg, if 100, it will always be true, and if it is 0 and will always be false. 50 would be 50/50 true/false.

## SL_ProximityCondition : SL_EffectCondition
Checks for proximity to required Item IDs.

`RequiredItems` (list of SL_Skill.SkillItemReq objects)
* Each `<SkillItemReq>` has: `ItemID` (integer, Item ID), `Consume` (true/false, item is consumed?) and `Quantity` (integer, quantity required).

`ProximityAngle` (float)
* The required angle relative to character facing direction for the proximity item

`OrMode` (bool)
* Changes the RequiredItems behaviour to accept ANY of the items for the condition to evaluate as true, instead of all of them.

## SL_ProximitySoulSpotCondition : SL_EffectCondition
Inherits from SL_ProximityCondition, and contains one extra field. This is for spells which require a Soul Spot nearby.

`Distance` (float)
* Maximum distance from soulspot.

## SL_QuestEventAreaCondition : SL_EffectCondition
Checks the current scene for quest events.

`EventUIDs` (list of string)
* A list of `<string>` Event UIDs. These are the UIDs to check for.

## SL_ShooterPosStatusEffect : SL_EffectCondition
I'm not really sure. It's used by Rune spells.

`StatusIdentifier` (string)
* The identifier of the status effect to check for.
* If true, overrides the Shooter cast position to the position of the status effect...?

## SL_StatusEffectCondition : SL_EffectCondition
Checks if the affected character has a given status effect. Use `SL_HasStatusEffectEffectCondition` for other checks.

`StatusIdentifier` (string)
* Identifier name of the status to check for.

## SL_TimeDelayCondition : SL_EffectCondition
Not sure how this is a condition, but it delays the effect like a DelayCondition.

`DelayRange` (Vector2)
* x/y range for the delay.

`TimeFormat` (enum)
* The format of the delay time.
* Must be exactly one of: `GameHours` or `Seconds`

`IgnoreFirstCheck` (boolean)
* Whether to ignore the first check or not

## SL_WeaponIsLoadedCondition : SL_EffectCondition
Checks if the weapon in the given slot is loaded or not.

`SlotToCheck` (enum)
* The Weapon Slot to check.
* Must be exactly one of: `MainHand` or `OffHand`.

## SL_WindAltarActivatedCondition : SL_EffectCondition

Just checks if the Wind Altar is activated in the current region.

Has no extra fields, just Invert.