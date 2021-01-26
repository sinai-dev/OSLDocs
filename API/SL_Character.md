# SL_Character

See also: [Guides: Custom Characters](Guides/Characters.md)

?> Important note about SL_Characters: They are subject to save data which may interfere with your design process. While developing, you may want to delete the `Outward\Mods\SideLoader\_SAVEDATA\` folder after making a change, so that old data does not overwrite your changes.

<!-- tabs:start -->

#### ** Universal **

`UID` (string)
* The <b>unique identifier</b> for this character template.
* Can be used for the actual Character UID, or you can override it for dynamic spawns.
* Used by the OnSpawn callback to invoke the corresponding template's method.

`SaveType` (enum)
* Determines how the character is saved.
* `Temporary` will not save the character at all.
* `Scene` will save the character with the scene, and reset them on Area Reset.
* `Follower` will make the character change scenes with the players and never be destroyed unless you do it manually with your mod.

`Name` (string)
* The name of the character

`SceneToSpawn` (string)
* Optional, used for an automatic spawn.
* Refers to a <b>Scene build name</b> (these are listed on the wiki) 

`SpawnPosition` (Vector3)
* Optional, only used if SceneToSpawn is set.
* A Vector3 takes an `x`, `y` and `z` value.
* You can find World Position with the SL Menu.

`SpawnRotation` (Vector3)
* Optional, only used if SceneToSpawn is set.
* A Vector3 for the spawn rotation.

`StartingPose` (enum)
* Can use any of [these values](API/Enums/SpellCastType).
* The starting pose animation for the Character, which will be interrupted by movement or combat.

`Scale` (Vector3)
* The character's physical scale, by default `Vector3.one` (1.0, 1.0, 1.0).

`StartSheathed` (boolean)
* Should the character be sheathed on spawn? Default true.

`DestroyOnDeath` (boolean)
* If true, the character will be destroyed after they die

`Faction` (enum)
* The faction ("team") of the character. Not referring to Story factions.
* Common options are: `NONE`, `Players`, `Bandits`
* Other options are: `Mercs`, `Tuanosaurs`, `Deer`, `Hounds`, `Merchants`, `Golden`, `CorruptionSpirit`
* Default is `Players`, if unchanged.

`TargetableFactions` (list of Character.Factions)
* Optional, you can set this to manually define a list of targetable factions.
* Same possible values as Factions, but its a list.

### Loot Drops

`LootableOnDeath` (boolean)
* Should the character be lootable on death? This adds the loot interaction to them when they die.
* If using pouch and/or weapon drops, see the Equipment sub-heading below.

`DropPouchContents` (boolean)
* If LootableOnDeath is true, should the character drop their pouch contents?
* <b>Note:</b> if defining any DropTableUIDs you should set this to true (or SL will do it for you)

`DropWeapons` (boolean)
* If LootableOnDeath is true, should the character drop their weapon and/or shield? (These literally drop on the ground if equipped)

`DropTableUIDs` (list of string)
* A list of string UIDs which are [SL_DropTable](API/SL_DropTable.md) UIDs.
* You should define an SL_DropTable and then reference it here via its UID.
* These drops will be rolled on this SL_Character's death.

### Visual Data
The `CharacterVisualsData` field contains the visual data. You can set it to null or delete it for default visuals.

`Gender` (enum)
* Must be one of `Male` or `Female`

`SkinIndex` (integer)
* The ethnicity of the character
* `0` is Aurai, `1` is Tramontane, `2` is Kazite

`HeadVariationIndex` (integer)
* Face style. May be clamped depending on your chosen Gender and SkinIndex.

?> Note: If you want to hide the face with an Armor by default, you must set this to 0. This is a bug and will be fixed when possible.

`HairStyleIndex` (integer)
* Chosen hair style, corresponds to the character creation options.

`HairColorIndex` (integer)
* Chosen hair color, corresponds to the character creation options.

### Equipment
You can set the Item ID for each equipment slot on the character. These should be fairly self-explanatory.

`Weapon_ID` (integer)

`Shield_ID` (integer)

`Helmet_ID` (integer)

`Chest_ID` (integer)

`Boots_ID` (integer)

`Backpack_ID` (integer)

`Pouch_Items` (List of SL_ItemQty)
* A list of starting items in the character's pouch.
* These could be used for various things, mainly as guaranteed drops.
* You must enable LootableOnDeath and DropPouchContents if you want to drop these items for the player.
* See SL_ItemQty below.

`Backpack_Items` (List of SL_ItemQty)
* If setting these, you must define a Backpack_ID for the character.
* A list of SL_ItemQty objects which are the contents of the backpack.
* If you define a Lantern, Sideloader will automatically light it and hang it from the backpack.
* See SL_ItemQty below.

#### SL_ItemQty
The SL_ItemQty class is a simple holder for defining a quantity of an item.

`ItemID` (int)
* The Item ID.

`Quantity` (int)
* The quantity of the item, default 1.


### Character Stats
The next few fields relate to the character's stats.

`Health` (float)
* Max health of the character.

`HealthRegen` (float)
* Health regenerated per second, when not in combat.
* Can be negative.

`ImpactResist` (float)
* Impact resist stat, 0-100.

`Protection` (float)
* Physical protection stat, 0-100.

`Barrier` (float)
* Barrier stat, 0-100.

`Damage_Resists` (float array)
* Damage resistance stats for the character.
* Like on SL_Item and SL_Effect, this relates to the damage types.
* Order is physical, ethereal, decay, lightning, frost, fire.
* -100 to 100 values.

`Damage_Bonus` (float array)
* Same as Damage_Resists, but for Damage Bonuses.

`Status_Immunity` (list of string)
* This is like the `Tags` field on SL_Item or SL_StatusEffect. It contains a list of strings.
* Each entry is a Tag.
* Use the Outward Tools google sheet (link in Resources on sidebar) to get Tag names.

### SL_CharacterAI

The `AI` field is an SL_CharacterAI object, which is the class used to define your Character's AI behaviour. At the moment there is only one preset class, SL_CharacterAIMelee.

`AI` (SL_CharacterAI)
* Optional, The `AI` can be defined if you want AI for the character.

To define one in XML:

```xml
<AI xsi:type="SL_CharacterAIMelee">
    <!-- ... --->
</AI>
```

Or in C#:

```csharp
sl_character.AI = new SL_CharacterAIMelee()
{
  // ...
}
```

`CanDodge` (boolean)
* Can the character perform dodges?

`CanBlock` (boolean)
* Can the character perform blocks?

`CanWanderFar` (boolean)
* Can the character wander as far as they want? (for "Wander" type only)

`ForceNonCombat` (boolean)
* Set true to force the character to always be passive

`AIContagionRange` (float)
* Probably won't do anything yet. I think it's used for AI Squads.

`Wander_Speed` (float)
* Speed modifier for Wander state

`Wander_FollowPlayer` (boolean)
* <b>Note:</b> This is used by Summoned Ghost, but I don't think it's working yet for SL_Characters properly. Probably best to leave it on false.

`Wander_Type` (enum)
* Must be one of: `Wander` or `Patrol`
* `Wander` is free-roam
* `Patrol` requires you to set the Waypoints (explained below)

`Wander_PatrolWaypoints` (list of SL_Waypoint)
* If using `Patrol` type, set the world-position waypoints here.
* See SL_Waypoint below for more details.

`Suspicious_Speed` (float)
* Movement speed modifier for suspicious state

`Suspicious_Duration` (float)
* How long to stay suspicious for after losing detection

`Suspicious_Range` (float)
* Distance of suspicious detection range

`Suspicious_TurnModif` (float)
* Modifier for turn speed in suspicious state

`Combat_ChargeTime` (Vector2)
* Random min/max for charge time for attacks

`Combat_TargetVulnerableChargeModifier` (float)
* Modifier for charge time when target is vulnerable

`Combat_ChargeAttackRangeMulti` (float)
* Modifier for attack range expectation by AI

`Combat_ChargeTimeToAttack` (float)
* Time for an attack

`Combat_ChargeStartRangeMult` (Vector2)
* Random min/max for range to start an attack charge

`Combat_SpeedModifiers` (list of float)
* A list of modifiers for the combat speed based on range, needs more investigation

`Combat_ChanceToAttack` (float)
* Between 0 and 100, average chance to attack per AI update (not sure on frequency)

`Combat_KnowsUnblockable` (boolean)
* Does the AI know about unblockable attacks?

`Combat_DodgeCooldown` (float)
* Cooldown on dodge frequency, if can dodge

`Combat_AttackPatterns` (list of AttackPattern)
* The actual attacks the character can perform.
* A list of AttackPattern objects, see below for details on AttackPattern.

#### SL_Waypoint

An SL_Waypoint has:

`WorldPosition` (Vector3)
* The world position of the waypoint

`RandomRadius` (float)
* How much randomness there is to the world position

`WaitTime` (Vector2)
* A random min/max for how long the character waits at the patrol point.

#### AttackPattern

An AttackPattern has:

`ID` (integer)
* The ID for this attack pattern, indexed from 0 to the number of attack patterns you've defined.

`Chance` (float)
* Weighted chance to use this attack

`Range` (Vector2)
* The allowed range to target for this attack

`HPPercent` (Vector2)
* The allowed HP percent min/max for the AI character to perform the attack

`Attacks` (list of AtkTypes)
* A list of `AtkTypes` objects, which can be either `Normal` or `Special`

## SL_CharacterTrainer

The SL_CharacterTrainer has two additional fields, and can be used to set up a basic trainer interaction for your character which opens a skill tree menu.

`InitialDialogue` (string)
* The initial dialogue message when you talk to the character. Currently this is all that is realistic, more advanced support may come in the future.

`SkillTree` (SL_SkillTree)
* Your custom skill tree.

For more information on SL_SkillTree, see [Skill Trees](Guides/SkillTrees.md).

For an XML template of CharacterTrainer, see [Custom Characters](Guides/Characters.md).

#### ** C# Only **

### Fields

`ShouldSpawn` (bool Function)
* The <b>optional</b> ShouldSpawn delegate accepts a `Func<bool>` method, which will determine if the SL_Character should spawn for a Scene-type spawn or not.
* Does not affect manual dynamic spawns (just check it yourself in that case)

For example:
```csharp
myCharacter.ShouldSpawn = () => { true; };

// or 

myCharacter.ShouldSpawn = ShouldSpawnMyChar;

private bool ShouldSpawnMyChar()
{
	// ...
}
```

### Methods

`Prepare()`
* Call this in OnPacksLoaded to register your character template.

`Spawn(Vector3 position, Vector3 rotation, string characterUID, string extraRpcData)`
* Manually spawn a character using this template, with an optional manual UID and RPC data.
* `characterUID` is optional, if not provided it will use the default from your template. If you are spawning multiple dynamic characters from this template, use UID.Generate().
* `extraRpcData` is any custom data you want to have passed over network, it will be sent to your `OnSpawn` callback.

`SL_Character.TryEquipItem(Character character, int id)`
* Helper to force-equip an Item onto the provided Character.
* Will check if they own the item first, otherwise will spawn and give it to them.

### Events
`Action<Character, string> OnSpawn`
* Invoked when any character using this template is spawned
* The `Character` argument is the Character instance that was just spawned
* The `string` argument is the `extraRpcData` you may have provided to your Spawn method.

`Action<Character, string, string> OnSaveApplied`
* Invoked when any character using this template is loaded from a save. 
* Same arguments as OnSpawn, the extraRpcData is from what you gave to the Spawn method.
* The second string argument is the extra save data you may have supplied from `OnCharacterBeingSaved`.

`Func<Character, string> OnCharacterBeingSaved`
* Invoked as the character is being saved.
* The Character argument is the character being saved
* The string argument is the data you must provide as the return value, which is sent to the OnSaveApplied method.
* For example, `public string MySaveMethod(Character character) { /* ... */ }`

## CustomCharacters

The `CustomCharacters` class has a few methods to give you more control of your characters from C#.

`CustomCharacters.DestroyCharacterRPC(Character character)` (or `string UID`)
* Destroys a character via a networked RPC call, so it will clean up for all connected players.

`CustomCharacters.CloneCharacter(Character _targetCharacter)` (or `string _gameObjectPathAndName`)
* [BETA] Clones the provided character and returns the clone to you.
* Experimental feature, intended to be used on existing scene enemies (not custom SL_Characters).

<!-- tabs:end -->