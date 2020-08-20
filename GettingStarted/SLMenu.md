# SL Menu

## Overview
The <b>SideLoader Menu</b> is currently used for generating XML Templates from existing Items, Status Effects and Enchantment recipes. It is also capable of exporting icons and textures from the prefab.

## Using The Menu

Press <b>Control + Alt + F6</b> to show or hide the SideLoader menu when in-game.

<i>Note: F6 must be pressed last in the combo.</i>

### Items

To export an Item, simply enter the `Item ID` for the Item you want to export from. You can find these IDs on the [Outward Wiki](https://outward.gamepedia.com), or on the [Outward Tools Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=673914692).

You can also set the `New ID`. This will determine the ID which your template will be <b>applied to</b>. If you want to edit an existing item, enter that ID here, otherwise enter a new unique ID.

When you press the button to generate a template, it will be generated to <b>`Mods/SideLoader/_GENERATED/Items/`</b>. It will automatically create the proper sub-folder structure including the Textures and Icons.

To use this generated template, take the folder (eg. `2000010_IronSword\`) and put it into the `Items\` folder of your SL Pack. It should look like `Items\2000010_IronSword\Iron Sword.xml`, for example.

The Menu also lets you set the `EffectBehaviour` setting on the `SL_Item` template.

* <b>Destroy Effects</b>: `DestroyEffects`
* <b>Override Effects</b>: `OverrideEffects`
* <b>None (leave all)</b>: `NONE`

### Status Effects

The Status Effects tab allows you to dump a status effect, in a similar way to items. The templates will be generated to the folder `Outward\Mods\SideLoader\_GENERATED\StatusEffects\`.

To find the ID of the status you want to dump, use [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1969601658).

### Enchantments

Pick any Enchantment ID off [this list](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/Enchantments.txt).

The template will be generated to `Mods\SideLoader\_GENERATED\Enchantments\`.

### Hot Reload

The <b>Hot Reloading</b> tab is for reloading and re-applying all SL Pack folders at runtime.

You can use this to speed up the development work-flow when creating your SL mods.

Some aspects of SideLoader may not support Hot Reloading. The C# Events (`OnPacksLoaded`, etc) are <b>NOT</b> called on hot reloads.