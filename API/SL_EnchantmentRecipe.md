# SL_EnchantmentRecipe

`SL_EnchantmentRecipe`s can be defined from the SL Menu/XML or C#. Enchantment XMLs should be placed in the `Enchantments\` sub-folder of your SL Pack.

You can make a recipe scroll with a `SL_EnchantmentRecipeItem` template. See the [SL_Item](API/SL_Item.md) page for more details.

Use the [SL Menu](Basics/SLMenu.md) to create an enchantment template, or define it from C# directly.

<!-- tabs:start -->
#### ** Universal **

`EnchantmentID` (int)
* The ID of this Enchantment Recipe and result.
* Can be an existing ID, or a new one.

`Name` (string)
* The name of this Enchantment.

`Description` (string)
* Description (displayed when inspecting the <b>applied</b> enchantment). Not all Enchantments use this.

`IsEnchantingGuildRecipe` (boolean)
* Is this for the New Sirocco Enchanting Guild? True if so, false otherwise.

`IncenseItemID` (int)
* The Item ID for the Incense required by this recipe.

The `PillarDatas` field is list of PillarData objects.

Each PillarData has:
* `Direction` (enum): one of `North`, `South`, `East` or `West`
* `IsFar` (boolean): Whether this is a "Far" pillar or not (otherwise "Close").

The `CompatibleEquipment` contains information about the compatible equipment.

* `RequiredTag` (string): The <b>REQUIRED</b> Tag for Equipment with this Enchantment. Eg, "Item", "Weapon", "Armor", "Helmet", "Boots". Can pick any Tag off [this list of Tags](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1840819680).
* `Equipments` (list of IngredientData): The optional list of compatible Equipments, either by Tag or Specific Item ID. If not set, any item with RequiredTag will work.

Each IngredientData has:
* `SelectorType` (enum): One of: `SpecificItem` (Item ID) or `Tag` (Item Tag). Determines how the game uses the SelectorValue.
* `SelectorValue` (string): Depends on the SelectorType. If SpecificItem, this should be an <b>Item ID</b>. If Tag, this should be a Tag name.

`TimeOfDay` (list of Vector2)
* A list of Vector2 (`x` and `y`) which represents valid times during the day for this recipe.
* The values of the Vector2 correspond to the hours of the day (0 to 24).
* For example, a value of  `x: 5, y: 15` would mean that the any time between 5am and 3pm would succeed.

`Areas` (list of enum)
* A list of AreaEnum objects, corresponding to areas where this recipe is valid.
* You can pick any value off [this list](API/Enums/AreaEnum.md).

The `WeatherConditions` field is a list of WeatherCondition objects. Each of these has:
* `WeatherType` (enum): One of: `Clear`, `Rain`, `Snow` or `SeasonEffect`.
* `Invert` (boolean): Like the Invert field on SL_EffectCondition, this flips the result of the evaluation.

The `Temperature` is a list of TemperatureStep objects, corresponding to valid Environment temperatures for this recipe.
* Each TemperatureStep can be either: `Coldest`, `VeryCold`, `Cold`, `Fresh`, `Neutral`, `Warm`, `Hot`, `VeryHot`, `Hottest`.

`WindAltarActivated` (boolean)
* Whether or not the Wind Altar needs to be activated in the current region for this recipe.

`EnchantTime` (float)
* The time (in seconds) that this recipe takes to actually enchant.

`Effects` (list of SL_EffectTransform)
* Like on Items and Status Effects, this is a container for the Effects.
* Enchantments generally only use this for On-Hit effects, but it could probably be used for anything.
* See [the Effects article](API/SL_EffectTransform.md) for more information on how to set this up.

The `AddedDamages` is a list of AdditionalDamage objects. Each AdditionalDamage has:
* `AddedDamageType` (enum): The type of damage that will be added. One of `Physical`, `Ethereal`, `Decay`, `Electric`, `Frost`, `Fire` or `Raw`.
* `SourceDamageType` (enum): The existing type of damage to add from. Same values as AddedDamageType.
* `ConversionRatio` (float): The amount of SourceDamageType which is added as AddedDamageType. For example, 0.25 would add 25% of the Source type as Added type, 2.0 would be 200%, etc.

The `StatModifications` is a list of StatModification objects. Each StatModification has:
* `Stat` (enum): Pick any value off [this list](API/Enums/EnchantmentStat.md).
* `Type` (enum): How to add this stat. Must be one of `Bonus` or `Modifier`
* `Value` (float): The value applied to this stat.

`FlatDamageAdded` (list of SL_Damage)
* This is flat damage, simply added directly to the weapon's damage.

`DamageModifierBonus` (list of SL_Damage)
* This is defined the same way as FlatDamageAdded, however this adds Damage Modifier Bonus to the equipment.
* The "Damage" field is actually the value added to the corresponding Damage Bonus stats for that element, it's not literally flat damage.
* Looks the same as FlatDamageAdded, other than the field name.

`DamageResistanceBonus` (list of SL_Damage)
* Again, this is the same as the previous two fields, however this adds Damage Resistance stat.
* The "Damage" field is added to your Damage Resistance for that corresponding damage type.

`HealthAbsorbRatio` (float)
* Used by Vampiric weapons, the amount of Health leeched based on damage dealt.

`ManaAbsorbRatio` (float)
* Like "The Good Moon" enchantment, this will leech Mana based on damage dealt.

`StaminaAbsorbRatio` (float)
* Not actually something that is used by any existing enchantment, but it does work. Leeches Stamina.

`Indestructible` (boolean)
* Makes the equipment indestructible if true.

`TrackDamageRatio` (boolean)
* Used by Vampiric weapons (for the Vampiric Transmutation, the required damage dealt).

`GlobalStatusResistance` (float)
* Grants global Status Resistance stat (The Three Brothers DLC)

`QuestEventUID` (string)
* Required Quest Event UID. Only use this if you understand how Quest Event UIDs work and how to find them.

#### ** C# Only **

To apply the enchantment in C#, just call `template.ApplyTemplate();`. You should do this during the Awake method of your mod, and SideLoader will apply it during its normal setup process (will be applied by `OnPacksLoaded`).

<!-- tabs:end -->