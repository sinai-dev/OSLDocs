# SL_ItemExtension

An <b>SL_ItemExtension</b> can be defined in the `ItemExtensions` field on an [SL_Item](API/SL_Item.md) template.

## Edit behaviour

The field `SL_Item.ExtensionsEditBehaviour` is an `EditBehaviour` value (enum), it will determine how your Extensions are applied.

* `NONE` or `Override` will not remove any of the existing Extensions, and it will add or modify Extensions based on the ones you have defined.
* `Destroy` will remove all Extensions which are not defined in your ItemExtensions list.

## Fields

All SL_ItemExtension classes have this field.

`Savable` (boolean)
* Does this ItemExtension save its values to the player's save file? This depends on the type of ItemExtension you are using, it may not be applicable to all types.

# Sub-classes

Below are all the subclasses of `SL_ItemExtension`. These contain the field above, as well as extra fields.

## SL_BasicDeployable : SL_ItemExtension
The base class for `SL_Deployable` and `SL_BuildingUpgrade`, it cannot be used directly as the class.

`CantDeployInNoBedZones` (boolean)
* Whether or not this item can deploy in "no bed zones"

`DeploymentDirection` (Vector3)
* A Vector3 (has `x`, `y` and `z` values) representing the direction that the deployed will be in.

`DeploymentOffset` (Vector3)
* The offset position of the deployed item prefab

## SL_BuildingUpgrade : SL_BasicDeployable
Inherits from `SL_Deployable`. 

`BuildingToUpgradeID` (int)
* The Item ID of the Building this is an upgrade for

`SnappingRadius` (float)
* The snapping radius when deploying, default 5

`RequiredQuestEventUIDs` (list of string)
* A list of Quest Event UIDs required to build this upgrade.

`UpgradeFromIndex` (int)
* The UpgradeIndex which this continues from

`UpgradeIndex` (int)
* The Upgrade Index for this upgrade

## SL_Deployable : SL_BasicDeployable
Inherits from `SL_BasicDeployable`. Used to deploy or pack a deployable item (tents, crafting stations, etc). This behaviour is pretty specific to the existing Deployable items, you will need to use C# to modify the interactions in a significant way.

Deployables all have a "Deployed" state version and a "Packed" state version. The player only ever has the Packed version in their inventory. Generally the Deployed-state Item ID is the Packed-state Item ID + 1.

`State` (enum)
* Must be one of: `Deployed` or `Packed`

`DeployedStateItemID` (int)
* The Item ID of the Deployed-state version of the prefab. This is only set if you are editing a Packed-state item.

`PackedItemPrefabID` (int)
* The Item ID of the Packed-state item prefab. Only valid if you are editing a Deployed-state item. 
* This will allow the Deployed-state item to be disassembled, and the player receives this PackedItemPrefabID item.
* You have to actually set this on the Deployed-state item, setting this to the Packed-state item will do nothing.

`AutoTake` (boolean)
* Whether or not there is an animation when picked up (true = no animation)

`CastAnim` (enum)
* The animation when deploying or packing
* Can be any value off [this list](API/Enums/SpellCastType.md).

`DeploymentSound` (enum)
* The sound played when deployed
* Can be any value off [this list](API/Enums/SpellCastType.md)

`DisassembleOffset` (Vector3)
* The offset position when disassembling the item

`DisassembleSound` (enum)
* The sound played when disassembled
* Can be any value off [this list](API/Enums/SpellCastType.md)

## SL_ItemAddOn : SL_Deployable
SL_ItemAddOn inherits from `SL_Deployable`, and contains a few extra fields.

This class is for deployable which snap-on to another deployable, such as Alchemy Kit or Cooking Pot.

`AddOnCompatibleItemID` (int)
* The Item ID which this item can be deployed onto.

`AddOnStatePrefabItemID` (int)
* The Item ID of the deployed-state version of this item.

`SnappingRadius` (float)
* The radius which this item will snap-on to the compatible item when being deployed.

## SL_DestroyOnOwnerDeath : SL_ItemExtension

This class has no extra fields, it just destroys the Item if the Character dies.

## SL_Ephemeral : SL_ItemExtension

Ephemeral components give a lifespan to an Item, after which they are destroyed.

`Lifespan` (float)
* The time, in seconds, after which this item is destroyed.

## SL_MultipleUsage : SL_ItemExtension

This is like a more advanced version of the IsUsable and QtyRemovedOnUse fields on SL_Item. It allows a finer degree of control.

`AppliedOnPrice` (boolean)
* Whether each item of the stack applies to the total stack price

`AppliedOnWeight` (boolean)
* Whether each item of the stack applies to the total stack weight

`AutoStack` (boolean)
* Whether multiple items of the same type automatically stack together

`MaxStackAmount` (integer)
* The maximum stack count before making a new stack

## SL_Perishable : SL_ItemExtension

Perishable components make the item lose durability over time. Used by Food and Torches/Lanterns.

`BaseDepletionRate` (float)
* The base value used to reduce durability by

`DisableInInventory` (boolean)
* Whether this item perishes while in the inventory or not

`DontPerishInWorld` (boolean)
* Whether this item perishes when placed on the ground in the world

`DontPerishSkipTime` (boolean)
* Whether this item perishes when time is skipped (traveling, sleeping, etc)

`OverrideUpdateRate` (boolean)
* If set to a value above 0, this overrides the rate at which the BaseDepletionRate is applied. Otherwise it's every 5 seconds (multiplied against Environment Time delta).

## SL_Preserver : SL_ItemExtension

Preserver is used for Bags, and it slows the rate of Perishable items inside them.

`NullifyPerish` (boolean)
* Whether to completely prevent items perishing inside this bag

`PreservedElements` (list of SL_PreservedElement)
* See below

### SL_PreservedElement
The `PreservedElements` field on SL_Preserver is a list of SL_PreservedElement objects. Each of these sets the preservation amount for a certain Item Tag.

`Preservation` (float)
* The amount of preservation %, from 0 to 100.

`PreservedItemTag` (string)
* The Item Tag for this preservation value. Generally just "Food", but you could also use "Equipment", or something else.

## SL_Sleepable : SL_ItemExtension

Used for tents and bedrolls, allows the player to sleep in it. Must be on a Deployed-state SL_Deployable item.

`RestStatusIdentifier` (string)
* The Identifier of the Status Effect received from sleeping in this Sleepable

`AffectFoodDrink` (boolean)
* Whether sleeping in this tent affects the players food and drink values

`AmbushReduction` (int)
* The reduction to the ambush chance from sleeping in this item

`Capacity` (int)
* The amount of players that can sleep in this item. Not sure if going above 1 is supported.

`CharAnimOffset` (Vector3)
* Offset position of the player when interacting with the item.

`ColdProtection` (int)
* Protection against Cold weather when sleeping

`CorruptionDamage` (int)
* Corruption received after sleeping (not per hour), from 0 to 1000.

`HealthRecuperationModifier` (int)
* Percent modifier placed on the rate at which the player restores health (not burned health)

`HeatProtection` (int)
* Protection against Hot weather when sleeping

`IsInnsBed` (boolean)
* Is this a bed inside an Inn?

`ManaPreservationModifier` (int)
* Percent modifier placed on the rate at which the player restores mana while sleeping

`Rejuvenate` (boolean)
* Does the player restore all food and drink in this bed?

`StaminaRecuperationModifier` (int)
* Percent modifier placed on the rate at which the player restores stamina while sleeping

## SL_WeaponCharger : SL_ItemExtension

Used for Bows to draw-back the weapon.

`ChargingStaminaCost` (float)
* Stamina cost per tick while drawing the bow (tick every 0.5 or 1 seconds?)

## SL_WeaponLoadout : SL_ItemExtension

Used by pistols and bows, it handles the ammunition for the weapon.

`AmmunitionType` (enum)
* Must be one of: `Item` or `WeaponType`
* Determines whether the `CompatibleItemID` (Item) or the `CompatibleEquipmentType` (WeaponType) is used to find compatible ammunition.

`CompatibleItemID` (int)
* If AmmunitionType is Item, this is the compatible Item Id.

`CompatibleEquipmentType` (enum)
* If AmmunitionType is WeaponType, this is the compatible weapon type.
* Must be one of [these values](API/Enums/WeaponType.md)

`MaxProjectileLoaded` (int)
* Maximum projectile shots which can be loaded, before needing to reload.

`SaveRemainingShots` (boolean)
* Whether to save any remaining shots in the weapon when we reload.

`ShowLoadedAmmunition` (boolean)
* Whether to visibly show the loaded ammunition for the weapon

## SL_WeaponLoadoutItem : SL_WeaponLoadout

SL_WeaponLoadoutItem inherits from `SL_WeaponLoadout`, and contains one extra field. It is used by Bows.

`ReduceAmmunitionOnLoad` (boolean)
* Whether to reduce the ammunition when the weapon is loaded or not.