# SL_Item

See also: [Custom Items](Guides/Items.md).

?> For Icons and Item Visuals, see [Custom Item Visuals](Guides/ItemVisuals) and/or [SL_ItemVisual](API/SL_ItemVisual).

<!-- tabs:start -->

#### ** Universal **

All SL_Item classes have these fields.

`Target_ItemID` (integer) <b>[REQUIRED]</b>
* The Item ID of the Item you are cloning <b>from</b>, or the item you want to edit. If you leave out fields on the template, it will default to the value from this item.

`New_ItemID` (integer)
* The Item ID you want to apply the template <b>to</b>. It can be a new or existing ID. If set to -1, this will default to the Target ID.
* <b>You can use an Item ID below 0</b>, which is recommended for custom modded items. The game does not use any ID's below 0, so they are all safe for us to take.
* If you want to be compatible with other mods, coordinate with us in the [Outward Modding Discord](https://discord.gg/E9jaeUm) to reserve an ID range.

`Name` (text)
* The item name.
* [Unity Rich Text](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/StyledText.html) is supported. In Xml however, you must use `&lt;` for "<" and `&gt;` for ">", so for example it would look like `&lt;b&gt;` for `<b>`.

`Description` (text)
* Type out the full description as it should appear in game.
* If you want to overwrite to a blank description, set the value to `<Description />` in Xml, or `Description = ""` in C#.
* [Unity Rich Text](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/StyledText.html) is supported. See note about Xml Rich Text above.
* In Xml, don't use "\n" for new lines, put an actual new line.

`LegacyItemID` (integer)
* If you want a Legacy Chest upgrade for this item, enter that item ID here. That will be the result when this item is placed in a chest.
* Set to `-1` for no upgrade.

`IsPickable` (boolean)
* Can the item be picked up? This affects whether it shows in the game's F1 Debug Menu too.

`IsUsable` (boolean)
* Can you "Use" the item?

`QtyRemovedOnUse` (integer)
* If usable, how many get removed on use?

`GroupItemInDisplay` (boolean)
* Should this item stack in displays? This means it can be an item stack.

`HasPhysicsWhenWorld` (boolean)
* Determines if the item should behave as a rigid body when in world space

`RepairedInRest` (boolean)
* Does this item get repaired when resting? (If equipped)

`BehaviorOnNoDurability` (enum)
* What happens when this item reaches 0 durability
* Must be exactly one of: `NotSet`, `DoNothing`, `Destroy`, or `Replace`

`CastType` (enum)
* The animation when this item is "Used", or if it's a skill the animation when it is used
* See [this reference](API/Enums/SpellCastType.md) for the possible values you can set (there are a lot)

`CastModifier` (enum)
* A modifier on the character when you use or activate this item/skill.
* Must be exactly one of: `Immobilized`, `Mobile`, `Attack` or `NONE`

`CastLocomotionEnabled` (boolean)
* Can you move while casting or using this item/skill?

`MobileCastMovementMult` (float)
* If you can move while casting, this is the modifier placed on the character's movement speed.
* It's a "float" value, meaning you can set it to "0.3" or "1.52374", etc.

`CastSheatheRequired` (integer)
* Sets the type of weapon sheathing behavior when you use/cast this item or skill.
* `0` or `-1` = not set, do nothing
* `1` = sheathe required
* `2` = unsheathe required

`OverrideSellModifier` (integer)
* Overrides the Sell Modifier (at ALL merchants) for the item compared to the Buy Price. By default it is 0.3 for most items.
* Set to `-1` for no override
* Generally the only time this is used is for items with 1:1 Buy:Sell, eg Gold Ingot.

### Tags
The `Tags` is a list of string (text) values. Tags are used by the game for lots of various things.
* You can use this for an item to count as a type of ingredient, eg `Water`, `Meat`, `Vegetable`
* Also used for item behaviour, eg `Lexicon`
* See [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1840819680) for a list of tags.
* Do not include the tag number. Eg, put `Weapon`, not `1-Weapon`.

Some tags are added by default. For example, if you create an Axe, it will automatically get "Axe" and "Weapon", you don't have to set those.

### SL_ItemStats (StatsHolder)
The `StatsHolder` contains the item's <b>SL_ItemStats</b> values. Not all items have stats, but most do.

These stats are:

`BaseValue` (integer)
* The base <b>buy price</b> of the item. The sell price is 0.3x this value, by default.

`RawWeight` (float)
* The weight of this item. Can include decimal.

`MaxDurability` (integer)
* The max and starting durability of this item. No decimals.
* Set to -1 for indestructable

Note that Equipment and Weapons have their own custom stats which override the StatsHolder field. More details below in the Equipment and Weapons sections.

### EffectTransforms

`EffectTransforms` is a more complex part of custom items. This value is a <b>list of `SL_EffectTransform` objects.</b>

See the [Effects](API/SL_EffectTransform) page for documentation on all SL_Effect classes, and more details on how to set up EffectTransforms.

This is used for <b>Effects</b> for many purposes such as <b>On-Hit Effects</b> for weapon and skills, <b>Passive</b> effects, <b>Consumable</b> effects, etc.

The EffectTransforms value is actually a <b>list of Transforms</b> (a container for objects in a Unity game), which can each contain any number of custom Effects, as well as further nested child transforms.

`EffectBehaviour` (enum)
* This setting relates to your `EffectTransforms`, and it controls the template behaviour in regards to the Effects on the item or skill.
* The options are: `Destroy`, `Override` and `NONE`.
* <b>`Destroy`</b> will destroy all the existing effects on the item or skill before it applies your new ones. This means everything will be overwritten with your new effects.
* <b>`Override`</b> will only destroy existing transforms if you have defined one of the same name in your EffectTransforms list. Use this to replace specific transforms, but not all of them.
* <b>`NONE`</b> will add your effects on top (if you defined any) and not remove anything.

### ItemExtensions

The `ItemExtensions` and the `ExtensionsEditBehaviour` fields are used for various additional behaviour components added to Items, such as Deployable, Ephemeral, etc.

See [Item Extensions](API/SL_ItemExtension) for more information.

## Visuals and Icons

There are three fields on the SL_Item template which relate to custom visuals.

`SL_Item.ItemVisuals`, `SL_Item.SpecialItemVisuals` and `SL_Item.SpecialFemaleItemVisuals`.

See [Custom Item Visuals](Guides/ItemVisuals) and [SL_ItemVisual](API/SL_ItemVisual) for instructions on setting up custom visuals, and also for details on changing textures or icons with your own .png files.

Even if you are not defining custom visuals at all, you can still use some of these settings to align and rotate the existing visuals.

# SL_Item sub-classes

The sections below are specific to certain types of custom items.

## SL_Skill : SL_Item
See [SL_Skill](API/SL_Skill.md) for more information.

## SL_Equipment : SL_Item

The `SL_Equipment` template is used for base Equipment, and it inherits all the base fields on SL_Item. The classes SL_Bag and SL_Weapon inherit from SL_Equipment.

Along with all the SL_Item fields, you have the following:

`EquipSlot` (enum)
* Sets the Equipment Slot this item equips to
* Must be exactly one of: `Helmet`, `Chest`, `Foot`, `RightHand`, `LeftHand`, `Back`, or `Quiver`

`TwoHandType` (enum)
* Sets the two-handed behaviour for this item, if it equips to the hand-slot
* Must be exactly one of: `None`, `TwoHandedRight` (melee), or `TwoHandedLeft` (bow)

`IKType` (enum)
* For off-hand equipment, the IK (hand/arm animation) when the character holds the item.
* Must be exactly one of: `None`, `Lantern`, `Torch`, or `Lexicon`

### SL_EquipmentStats : SL_ItemStats

The <b>Equipment Stats</b> overrides the base `StatsHolder` field. It will look like:
```xml
<StatsHolder xsi:type="SL_EquipmentStats">
```

`Damage_Resistance` (list of floats)
* This is a list of float number values which sets the Damage Resistance of the equipment.
* Normal values are between -100 and 100.
* Define a `<float></float>` for each entry. There must be at least 6 entries, though technically the game uses 9.
* The order is: <b>`Physical`, `Ethereal`, `Decay`, `Lightning`, `Frost`, `Fire`,</b> and the unused `DarkOLD`, `LightOLD` and `Raw`

`Impact_Resistance` (float)
* Impact resistance stat, usually -100 to 100

`Damage_Protection` (float)
* Physical protection stat, usually -100 to 100

`Damage_Bonus` (list of float)
* Same as Damage_Resistance, but for damage bonuses instead, usually -100 to 100

`Impact_Bonus` (float)
* Bonus impact damage modifier stat, usually -100 to 100.

`Stamina_Use_Penalty` (float)
* Positive values mean it increases stamina costs, negative values decreases stamina costs.

`Mana_Use_Modifier` (float)
* Works the same way as stamina use penalty, but for mana.

`Mana_Regen` (float)
* Base mana regen stat per second

`Movement_Penalty` (float)
* A positive value will reduce movement speed, a negative value increases it.

`Pouch_Bonus` (float)
* Increases capacity of pouch

`Heat_Protection` (float)
* Hot Weather Def stat

`Cold_Protection` (float)
* Cold Weather Def stat

`Corruption_Protection` (float)
* For Soroboreans DLC, Corruption protection stat.

`Health_Regen` (float)
* Health regen per second

`Cooldown_Reduction` (float)
* Cooldown reduction %, between 0 and 100.

`BarrierProtection` (float)
* Barrier protection stat.

`GlobalStatusEffectResistance` (float)
* Global Status Effect Build Up Resistance stat

`StaminaRegenModifier` (float)
* Multiplier which affects Stamina Regen rate

## SL_Armor : SL_Equipment
`SL_Armor` inherits from `SL_Equipment`, and contains a couple extra fields.

`Class` (enum)
* The Armor Class. Not sure if this actually has an effect on anything.
* Must be exactly one of: `Cloth`, `Light`, `Medium` or `Heavy`.

`GearSoundMaterial` (enum)
* Sets the general movement sound of the equipment
* Must be exactly one of [these values](API/Enums/EquipmentSoundMaterials.md).

`ImpactSoundMaterial` (enum)
* The sound when the armor is hit
* Must be exactly one of [these values](API/Enums/EquipmentSoundMaterials.md).

## SL_Weapon : SL_Equipment

As well as all the fields on SL_Item and SL_Equipment, weapons also have a few more values and their own stats.

`WeaponType` (enum)
* Sets the type of weapon.
* Must be one of these values: [Weapon.WeaponType](API/Enums/WeaponType.md)

`Unblockable` (boolean)
* Sets the unblockable behaviour. Is this weapon <b>unblockable</b>? Currently no weapons in the live game have this as true, but it does work.

`SwingSound` (enum)
* The sound when you swing the weapon.
* Must be exactly one of: `Default`, `Weapon_1H`, `WeaponHvy_1H`, `Weapon_2H`, or `WeaponHvy_2H`

`SpecialIsZoom` (boolean)
* If true, the special attack is for zooming in (used for Bows).

`HealthLeechRatio` (float)
* Percent of damage dealt that is restored to player's current health

`HealthBurnLeechRatio` (float)
* Percent of damage dealt that is restored to player's <b>burnt</b> health

`IgnoreHalfResistances` (boolean)
* Does this weapon ignore 50% of enemy resistances?

### SL_WeaponStats : SL_EquipmentStats

The <b>Weapon Stats</b> inherits from SL_EquipmentStats, and contains some extra data for Weapons.

While weapons do have all the same fields as Equipment, they cannot actually some of those stats.

The extra stats you have on WeaponStats are:

`AttackSpeed` (float)
* The attack speed of the weapon. Usually it's 0.8, 0.9, 1.0, 1.1 or 1.2.

`Impact` (float)
* Base impact damage (displayed value only)

`StamCost` (float)
* Base stamina cost of attacks. For bows, this is consumed when you draw.

`AutoGenerateAttackData` (boolean)
* Only relevant for melee weapons.
* Defaults to `false`, and your `Attacks` will be used to set the Attack Data.
* If `true`, this will ignore your `Attacks`, and instead automatically generate scaled damages from your `BaseDamage`, `Impact` and `StamCost` values. There are standard multipliers used for each weapon class which the SideLoader uses to scale your stats.

#### BaseDamage
The `BaseDamage` is a list of custom [SL_Damage](API/SL_Damage) objects, and defines the base damage of the weapon. 

For the "Type", you must set one of: `Physical`, `Ethereal`, `Decay`, `Electric` (<b>not</b> Lightning), `Frost`, or `Fire`.

You can add as many SL_Damage entries as you want, though you should generally have 1 of each type at most.

#### AttackData
The `Attacks` field is a list of `AttackData` objects. This is the data used for the <b>actual attacks</b> for melee weapons, for the damage, impact and stamina cost. Melee weapons have <b>5</b> AttackData entries.

The first two AttackData entries are for the standard attacks, the 3rd is for Special attacks, and the 4th and 5th are for the combo attacks.

If `AutoGenerateAttackData` is `true`, these values are ignored and instead automatically generated.

The fields on AttackData are:

`StamCost`
* The stamina cost per swing

`Knockback`
* The impact damage per hit

`AttackSpeed`
* Not actually used, just ignore it.

The `Damage` value is a list of `float` values.
* This corresponds to the Base Damage you set. Each entry on the list represents a damage type, in the order that you defined the base damage. If you only defined one damage type, you only need one entry, etc.

## SL_MeleeWeapon : SL_Weapon

`SL_MeleeWeapon` inherits from `SL_Weapon`, and contains a few extra fields.

`LinecastCount` (int)
* The amount of line casts (physics check) this weapon performs.

`Radius` (float)
* The radius of the line cast check

## SL_DualMeleeWeapon : SL_MeleeWeapon

`SL_DualMeleeWeapon` inherits from `SL_MeleeWeapon`, and contains no extra fields. It just tells the game to treat this item as a dual melee weapon (like Gauntlets).

## SL_Ammunition : SL_Weapon

`SL_Ammunition` inherits from `SL_Weapon`, and contains one extra field.

`PoolCapacity` (int)
* The maximum amount of arrow projectiles active in the object pool at one time (for this weapon)

`BehaviorOnCollision` (enum)
* Determines behaviour on collision. Must be exactly one of: `None`, `Stuck` or `Destroyed`

## SL_ProjectileWeapon : SL_Weapon

`SL_ProjectileWeapon` inherits from `SL_Weapon`, and contains a few extra fields.

`AutoLoad` (boolean)
* Whether this weapon automatically loads?

`UnloadOnSheathe` (boolean)
* Whether or not to unload the weapon when it is sheathed

`UnloadOnEquip` (boolean)
* Whether to unload the weapon when it is unequipped

`UnloadOnIncompleteShot` (boolean)
* Whether to unload the weapon if the owner performs an incomplete shot

`LocomotionEnabledOnReload` (boolean)
* Whether the owner is able to move while reloading this weapon

`LoadAnim` (enum)
* A Character.SpellCastType value, sets the animation when loading.
* See [this reference](API/Enums/SpellCastType.md) for the possible values you can set (there are a lot)

`FullyBentSound` (enum)
* The sound which plays when the bow is fully drawn.
* Must be one of these values: [GlobalAudioManager.Sounds](API/Enums/Sounds.md).

## SL_Bag : SL_Equipment
Bags inherit from SL_Equipment, and contain a few extra fields.

`Capacity` (float)
* The capacity of the bag

`Restrict_Dodge` (boolean)
* Does this backpack restrict the character's dodge when equipped?

`InventoryProtection` (float)
* Protection to the durability of items in the backpack when hit from behind

## SL_RecipeItem : SL_Item
<b>SL_RecipeItem</b> is a very simple class, just used to make a Recipe Scroll. It inherits from `SL_Item` and contains only one other value:

`RecipeUID` (string)
* The UID of the recipe you want this scroll to teach.
* This corresponds to the `UID` in a [SL_Recipe](API/SL_Recipe) template.

## SL_EnchantmentRecipeItem : SL_Item
<b>Similar to RecipeItem</b>, but used for Enchantment recipes in The Soroboreans. Inherits `SL_Item`.

Note: the `Description` is for the Shop description for EnchantmentRecipeItems, otherwise the game auto-generates it.

`Recipes` (list of int)
* A list of Enchantment Recipe IDs that this scroll is for, usually 3 at most (Helmet/Chest/Boots).
* These can also be custom enchantment IDs.

## SL_Throwable : SL_Item
SL_Throwable inherits from SL_Item, and contains one more field. This is used for Grenades in The Three Brothers DLC.

`DestroyDelay` (float)
* Delay in seconds after throwing for explosion

## SL_ItemContainer : SL_Item
The `SL_ItemContainer` class inherits from `SL_Item`, and contains some extra fields used for item containers.

`BaseContainerCapacity` (float)
* The base container capacity weight

`SpecialType` (enum)
* Used to indicate this is a special type of container.
* Must be one of: `None`, `Pouch`, `SkillKnowledge`, `LegacyChest`, `Stash`, `EnchantmentTable`, `EnchantmentPillar`, `VampiricTransmutationTable`

`OpenSound` (enum)
* The sound played when the container is opened by a player
* Must be one of [these values](API/Enums/Sounds)

`AllowOverCapacity` (bool)
* If true, the container can go over the weight capacity

`CanContainMoney` (bool)
* Is the container allowed to contain money?

`CanReceiveCharacterItems` (bool)
* Is the container allowed to receive character items?

`CanRemoveItems` (bool)
* Is the player allowed to remove items from the container?

`NonSavableContent` (bool)
* Is the content in this container <b>non-</b>savable?

`ShowMoneyIfEmpty` (bool)
* Should the container show the contained money if it is empty?

`VisibleIfEmpty` (bool)
* Is the container visible when empty?

`ItemFilter` (SL_ItemFilter)
* The filter on items which are allowed to go in this container.

### SL_ItemFilter
The SL_ItemFilter class is used by an SL_ItemContainer.

`MatchOnEmpty` (bool)
* If there are no other defined filters, what is the default match result?

`EquipmentTypes` (list of EquipmentSlot.EquipmentSlotIDs)
* A list of the allowed equipment slot types of items
* Must be one of [these values](API/Enums/EquipmentSlotIDs)

`TagTypes` (list of Tag.TagTypes)
* A list of the allowed tag types for items
* Must be one of [these values](API/Enums/TagTypes)

## SL_DeployableTrap : SL_ItemContainer

The SL_DeployableTrap inherits from SL_ItemContainer, and is used for deployed-state Traps.

`OneTimeUse` (bool)
* Limit to one-time use only?

`TrapRecipeEffects` (list of SL_TrapEffectRecipe)
* The list of recipes/effects for the trap.

### SL_TrapEffectRecipe

The class SL_TrapEffectRecipe is used by an SL_DeployableTrap to define the different trap recipes and effects.

`Name` (string)
* The name of this trap recipe result

`Description` (string)
* Description of this trap recipe result

`CompatibleItemTags` (list of string)
* A list of the compatible Tags for this trap recipe

`CompatibleItemIDs` (list of int)
* A list of the compatible Item IDs for this trap recipe

`TrapEffects` (list of SL_Effect)
* A list of SL_Effect (like the Effects on an SL_EffectTransform)
* These are the standard effects

`HiddenEffects` (list of SL_Effect)
* Same as TrapEffects, but used for Pressure Plates when the player has Pressure Plate Expertise.

## SL_Instrument : SL_Item
SL_Instrument inherits from SL_Item, and is used for the Instruments (Ghost Drum and Sky Chimes) in The Three Brothers DLC.

`PeriodicTime` (float)
* How frequently the Periodic Effects (the Hex infliction for the vanilla instruments) happens

`PulseSpeed` (Vector2)
* Affects the pulse speed of the visual effects depending on the number of charges
* `x` is the minimum value and `y` is the maximum value

`StrikeTime` (float)
* How long the delay is after striking the instrument before it can be hit again

## SL_Building : SL_Item
SL_Building inherits from SL_Item, and is used for Buildings in The Three Brothers DLC.

`BuildingType` (enum)
* The type of the building. Must be one of: `Basic`, `Specialized`, `House` or `ResourceProducing`

`ConstructionPhases` (list of `SL_ConstructionPhase`)
* The construction phases, generally 0 = placed but construction not started, 1 = mid-way through construction or done, 2 = done

`UpgradePhases` (list of `SL_ConstructionPhase`)
* The upgrade options. These generally have one phase for construction and not multiple phases like the base construction did.

### SL_ConstructionPhase

The fields on SL_ConstructionPhase are:

`Name` (string)
* The name of the phase

`PhaseType` (enum)
* What type of construction phase this is, must be one of: `WIP`, `Finished`, `Upgrade`

`ConstructionCosts` (BuildingResourceValues)
* The cost of construction
* This value is a `BuildingResourceValues` struct. For the available values, see BuildingResourceValues below.

`ConstructionTime` (float)
* The time it takes to construct this building, in days

`RareMaterialID` (int)
* The Item ID of the Rare Material required for this building (if any)

`BuildingRequirements` (list of `SL_BuildingRequirement`)
* The requirements for the building to be allowed to be constructed
* See `SL_BuildingRequirement` below for the available fields on this value

`HouseCountRequirement` (int)
* The required Housing value for building to take place

`HousingValue` (int)
* The Housing gained from constructing this building

`MultiplyProductionPerHouse` (bool)
* Does this building multiply its production per House constructed?

`CapacityBonus` (BuildingResourceValues)
* The storage capacity bonus gained from constructing this building.
* The value is a `BuildingResourceValues` struct (see below)

`UpkeepCosts` (BuildingResourceValues)
* The costs for maintaining this building once it is constructed, per day.
* The value is a `BuildingResourceValues` struct (see below)

`UpkeepProductions` (BuildingResourceValues)
* The productions this building generates, per day.
* The value is a `BuildingResourceValues` struct (see below)

### BuildingResourceValues
BuildingResourceValues is a struct which contains the fields `Funds`, `Timber`, `Food` and `Stone`. 

Each value is an `int`, representing the value for that material.

### SL_BuildingRequirement
An `SL_BuildingRequirement` value has the following fields:

`ReqBuildingID` (int)
* The Item ID of the Required Building

`ReqUpgradeIndex` (int)
* The required upgrade index on that building, if any

## SL_MultiItem

SL_MultiItem can be used from XML to define multiple item edits from one template. This might be useful if you are making bulk edits, or lots of very small edits.

The SL_MultiItem template is very simple, it's basically just a list of SL_Item templates. You only have one field: `Items`, which is a list of SL_Items.

It should look like this in XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<SL_MultiItem xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	<Items>
	
	    <!-- Just define all your templates here. -->
		
		<SL_Item>
			<Target_ItemID>2000010</Target_ItemID>
			<New_ItemID>2000010</New_ItemID>
			<Name>Iron Sword 2</Name>
			<Description>Test.</Description>
		</SL_Item>
		
		<SL_Item>
			<Target_ItemID>2000150</Target_ItemID>
			<New_ItemID>2000150</New_ItemID>
			<Name>Brand 2</Name>
			<Description>Test.</Description>
		</SL_Item>
		
		<!-- etc... -->
		
	</Items>
</SL_MultiItem>
```

#### ** C# Only **

### Fields

`SLPackName` (string)
* If setting custom textures or icons, the name of the SL Pack you to use

`SubfolderName`
* If setting custom textures or icons, the name of the subfolder in the `Items` folder of the SLPack which contains your `Textures` folder to use
* See [Custom Item Visuals](Guides/ItemVisuals.md) for more information

`SL_Item.CurrentlyAppliedTemplates` (List<SL_Item>)
* Static list of all currently applied SL_Item templates.
* This is reset on Hot Reload.

### Methods

`Apply()`
* Call this to prepare and apply the template. Do this in Awake or BeforePacksLoaded.

`AddOnInstanceStartListener(Action<Item> listener>)`
* The OnInstanceStart event is called when an Item with this template's applied ID is created or loaded during gameplay, in an actual scene.
* `Action<Item> listener` is your callback. The Item argument passed to your method is the instance that was just created.

`static SL_Item.AddOnInstanceStartListener(int itemID, Action<Item> listener)`
* Same as the non-static version (above), but you can use this to add a callback for ANY Item ID, even if you're not making a template for it.

### Events

`Action<Item> OnTemplateApplied`
* Simple event which is called after this template is applied, either on startup or hot reload.
* The item argument passed to your method is the Item prefab that was just created.

## CustomItems

The `CustomItems` class has a few extra helpers you can use to make things easier when modifying custom items in general.

`CustomItems.GetOriginalItemPrefab(int ItemID)`
* Use this to get the true original prefab for an Item ID, in case it has been modified.

`CustomItems.CreateCustomItem(int cloneTargetID, int newID, string name, SL_Item template = null)`
* You can use this to clone an item and get the cloned prefab back.
* Helpful if you just want to do your own custom item mods and not use the SideLoader templates at all, while still being compatible with other mods.

`SetNameAndDescription(Item item, string _name, string _description)`
* Helper to set the Name and/or Description on an Item easily.
* You can use `null` and it will keep the existing value.

`SetItemTags(Item item, string[] tags, bool destroyExisting)`
* In case you want to set the tags on an item directly from a string array, you can do so with this.

<!-- tabs:end -->