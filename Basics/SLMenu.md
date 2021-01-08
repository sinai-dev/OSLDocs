# SL Menu

The <b>SideLoader Menu</b> can be used for generating Templates from existing Items, Status Effects and Enchantment recipes, and has a few other various helpers.

## Using The Menu

Open your in-game keybindings and set a binding for the SideLoader Menu. 

Press the button and the menu should open. If it does not, you may have installed SideLoader incorrectly, or you may have conflicting mods installed. Contact Sinai if you can't figure it out.

### Items

To export an Item, simply enter the `Item ID` for the Item you want to export from. You can find these IDs on the [Outward Wiki](https://outward.gamepedia.com), or on the [Outward Tools Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=673914692).

When you press the button to generate a template, it will be generated to <b>`Mods/SideLoader/_GENERATED/Items/`</b>. It will automatically create the proper sub-folder structure including the Textures and Icons.

To use this generated template, take the folder (eg. `2000010_IronSword\`) and put it into the `Items\` folder of your SL Pack. It should look like `Items\2000010_IronSword\Iron Sword.xml`, for example.

You can also just take the XML file itself and put it in the `Items\` folder directly.

### Status Effects

The Status Effects tab allows you to dump a status effect, in a similar way to items. The templates will be generated to the folder `Outward\Mods\SideLoader\_GENERATED\StatusEffects\`.

To find the ID of the status you want to dump, use [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1969601658).

### Enchantments

Use the [Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1969601658) to find Enchant IDs, and dump them with the menu.

The template will be generated to `Mods\SideLoader\_GENERATED\Enchantments\`.

### Hot Reload

In the <b>SL Packs</b> tab you can press the <b>Hot Reload</b> button, which will reload and re-apply all SL Pack folders at runtime.

You can use this to speed up the development work-flow when creating your SL mods.

Some aspects of SideLoader may not support Hot Reloading. The C# Events (`OnPacksLoaded`, etc) are <b>NOT</b> called on hot reloads currently.