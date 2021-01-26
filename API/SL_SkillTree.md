# SL_SkillTree

<b>Skill Trees</b> can be defined through the SideLoader with C# or XML. 

For C#, I suggest subscribing to `SL.OnPacksLoaded` and defining the skill tree after this point. The Skill Tree template assumes that the skills IDs are already set up when you apply it.

For SL Menu/XML, you must use an [SL_CharacterTrainer](API/SL_Character) and define your skill tree directly there.

<!-- tabs:start -->
#### ** Universal **

The `SL_SkillTree` class is the base class for containing the tree, as well as applying it once it is set up.

`Name` (string)
* The Name of your skill tree. This also sets the SkillSchool.UID and name.

`SigilIconName` (string)
* You can use this to set the Sigil sprite for the skill tree from a PNG file.
* The PNG should be placed in the `Texture2D\Local\` folder of your SLPack, and you reference it by its file name (without ".png").
* For example, set `MyTreeIcon` as the name and use a png file `{SLPack}\Texture2D\Local\MyTreeIcon.png`.
* For an example skill tree sigil, [see here](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/tex_men_treeLogoHermit.png)

`SkillRows` (List<SL_SkillRow>)
* The rows of your skill tree. `SL_SkillRow` is explained below.

### SL_SkillRow
A skill tree can contain up to 5 rows. `SL_SkillRow` is the holder for these rows. Each row can contain 3 `SL_BaseSkillSlot` templates.

`RowIndex` (integer)
* The row index. Between `1` and `5`.
* `3` is the breakthrough row.
* `1` is the bottom, and `5` is the top.

`Slots` (List<SL_BaseSkillSlot>)
* The list of slots on this row.
* Each slot should actually be either a `SL_SkillSlot` or a `SL_SkillSlotFork`, don't use the base slot directly.

### SL_BaseSkillSlot
The base class used by both `SL_SkillSlotFork` and `SL_SkillSlot`.

`ColumnIndex` (integer)
* The column index of this slot on the row. 
* `1` is left, `2` is middle, `3` is right.

`RequiredSkillSlot` (Vector2)
* This one may seem a bit confusing at first, but it's pretty simple.
* A Vector2 is just a `x` and `y` value.
* The `x` is the <b>row</b>, and the `y` is the <b>column</b> of your required skill slot.
* You should make sure that `row x column y` exists on your tree.

### SL_SkillSlotFork : SL_BaseSkillSlot
`SL_SkillSlotFork` is used to force a choice between two skills. Normally if you use a fork, it is in <b>Column 2</b> and it is the only slot on that row.

`Choice1` (SL_SkillSlot)
* The first choice.

`Choice2` (SL_SkillSlot)
* The second choice.

### SL_SkillSlot : SL_BaseSkillSlot
`SL_SkillSlot` is the actual class used for defining a skill slot.

`SkillID` (integer)
* The reference Skill `ItemID` for this slot. You should make sure it's already set up.

`SilverCost` (integer)
* The cost in silver for this slot

`Breakthrough` (boolean)
* Is this a breakthrough? Defaults to false

#### ** C# Only **

`SkillSchool CreateBaseSchool()`
* Call this first to create the base `GameObject`. 
* It returns the `SkillSchool` to you (a component on the base `GameObject`)

`Sprite Sigil`
* Only use this if you did not use the SL Pack approach to setting the tree sigil.
* The `Sigil` is the background icon when using the skill trainer for this tree.
* A Sigil you can use as an example can be found [here](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/tex_men_treeLogoHermit.png).
* Use the `CustomTextures` class to load your PNG and create your Sprite to set.

<!-- tabs:end -->