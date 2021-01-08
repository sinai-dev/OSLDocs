# Custom Characters

See also: [Guides: Custom Characters](Guides/Characters.md)

## SL_Character

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

`AddCombatAI` (boolean)
* Defaults to false
* If true, will add a basic combat AI to the character.
* This AI is based on the Summoned Ghost. You can modify it if you want.

`CanDodge` (boolean)
* Can the combat AI dodge?

`CanBlock` (boolean)
* Can the combat AI block?

`Faction` (enum)
* The faction ("team") of the character. Not referring to Story factions.
* Common options are: `NONE`, `Players`, `Bandits`
* Other options are: `Mercs`, `Tuanosaurs`, `Deer`, `Hounds`, `Merchants`, `Golden`, `CorruptionSpirit`
* Default is `Players`, if unchanged.

### Visual Data
The `CharacterVisualsData` field contains the visual data. You can set it to null or delete it for default visuals.

`Gender` (enum)
* Must be one of `Male` or `Female`

`SkinIndex` (integer)
* The ethnicity of the character
* `0` is Aurai, `1` is Tramontane, `2` is Kazite

`HeadVariationIndex` (integer)
* Face style. May be clamped depending on your chosen Gender and SkinIndex.

`HairStyleIndex` (integer)
* Chosen hair style, corresponds to the character creation options.

`HairColorIndex` (integer)
* Chosen hair color, corresponds to the character creation options.

### Equipment IDs
You can set the Item ID for each equipment slot on the character. These should be fairly self-explanatory.

`Weapon_ID` (integer)

`Shield_ID` (integer)

`Helmet_ID` (integer)

`Chest_ID` (integer)

`Boots_ID` (integer)

`Backpack_ID` (integer)

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

## SL_CharacterTrainer

The SL_CharacterTrainer has two additional fields, and can be used to set up a basic trainer interaction for your character which opens a skill tree menu.

`InitialDialogue` (string)
* The initial dialogue message when you talk to the character. Currently this is all that is realistic, more advanced support may come in the future.

`SkillTree` (SL_SkillTree)
* Your custom skill tree.

For more information on SL_SkillTree, see [Skill Trees](Guides/SkillTrees.md).

For an XML template of CharacterTrainer, see [Custom Characters](Guides/Characters.md).