# SL Overview

## How are SideLoader mods made?
There are two ways you can use SideLoader:
1. Using <b>SL Packs</b> (folders containing <b>XML</b> files and other assets)
2. Using the <b>C#</b> API from your own plugin

To get the most out of SideLoader, it is recommended to use both the SL Pack folders to load your assets, and the C# API for extended features.

### Simple Replacements

SideLoader allows you to replace certain assets (currently `.png` textures and `.wav` audio) with your own files directly. This is designed to be as simple as possible, all you have to do is use the same file name that the game uses to replace the asset, and place it in the corresponding [SLPack folder](Basics/SLPacks). 

See also:
* [Replacing Audio](Guides/ReplacingAudio.md)
* [Replacing Textures](Guides/ReplacingTextures.md)

### Custom Content
SideLoader also offers an expansive API for all sorts of <b>changes or additions</b> to game content such as Items, Status Effects, Characters and more. 

See also: 
* [Custom Characters](Guides/Characters.md)
* [Custom Items & Skills](Guides/Items.md)
* [Custom Item Visuals](Guides/ItemVisuals.md)
* [Custom Statuses](Guides/StatusEffects.md)
* [Custom Skill Trees](Guides/SkillTrees.md)
* [Recipes](API/SL_Recipe.md)
* [Enchantment Recipes](API/SL_EnchantmentRecipe.md)

### Editing Etiquette
When using SideLoader to <b>edit existing content</b>, you should be courteous of other modders and only include things in your template if you are actually changing them. This allows different types of SideLoader mods to be compatible with each other.

For example, one mod might make changes to lots of item descriptions, and another might make changes to their textures. These two mods would be compatible with each other assuming both authors only included the things they are changing.

The SL Menu can generate templates for you from existing assets, and it will include <b>everything</b> you might want to change when it generates the template. However you don't need to include all of the output when you are making your mod, and in fact you should delete everything you are not changing, other than the few required fields.

## F.A.Q ##
### What should I do to get started? {docsify-ignore}
See the other articles in the <b>Basics</b> category, and then follow along with the <b>Guides</b>.

### Can I use this to edit existing Items/Skills/etc? {docsify-ignore}

Yes, SideLoader allows you to control whether you are creating a completely new asset or just editing an existing one. When editing an existing item or status effect, you should remove (delete or comment out) all other things on the template that you don't want to change, to avoid conflicts with other SideLoader edits to the same item.

### How can I know the possible values for X setting on a template? {docsify-ignore}

Check the docs page for that kind of template, or [Resources](Basics/Resources) page.

### Does this work in multiplayer? {docsify-ignore}

Yes, but make sure all players have the same SideLoader mods installed for it to work correctly.

### Can I load custom Levels using this? {docsify-ignore}

There is some limited support for it from C# with the `CustomScenes` class, but it's not well tested yet.
