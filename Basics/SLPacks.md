# SL Packs

An <b>SL Pack</b> (<b>SideLoader Pack</b>) is used to load and configure custom assets, and it is simply a <b>folder</b> which uses a specific structure and names.

You can create and manage your SL Packs via the [SL Menu](Basics/SLMenu). Deleting a pack requires you to delete the folder manually yourself. When working from the SL Menu, a lot of the rest of the process will be handled for you and you may not find the need to manually touch the folders at all, however it is still recommended to have an understanding of how it all works.

## SLPack Name

The folder structure of an SL Pack is like so:
* `Outward\BepInEx\plugins\{Name}\SideLoader\`

By default, the `{Name}` in the path is the unique name of your Mod or Asset Pack if you don't override it with a `manifest.txt` file. It can be anything, but try to be original to avoid conflicts.

For example, `Outward\BepInEx\plugins\MyCoolPack\SideLoader\` is an SL Pack folder called MyCoolPack.

?> In C#, this {Name} is what you'll use for `SL.GetSLPack(string name)`

SL also supports legacy packs in the structure `Mods\SideLoader\{NAME}\`, although this is no longer recommended for new packs.

## Manifest file
SideLoader also supports a file in the root of an SL Pack called `manifest.txt`, this file can be used to manually set the name of the SLPack instead of relying on the folder name.

The reason for this is because Outward is now on the [Thunderstore](https://thunderstore.io) and we will not have total control over our folder names when using r2modman.

Currently, SideLoader will simply read the `manifest.txt` file and whatever you write in there will be your SL Pack name. For example, CombatHUD's `manifest.txt` file just contains `sinai-dev CombatHUD`, so this is what is used as the SL Pack name.

If this design changes in the future then we will use a different name than `manifest.txt` for the new design and provide backwards support for this one, but we just needed a simple solution for now.

## SL Pack Structure
Other than the Name, the only special thing about your SL Pack is the name of the sub-folders you create inside of it.

The sub-folders you can create are:
* `AudioClip\`
* `AssetBundles\`
* `Characters\`
* `DropTables\`
* `Enchantments\`
* `Items\`
* `ItemSources\`
* `Recipes\`
* `StatusEffects\`
* `StatusFamilies`
* `Tags\`
* `Texture2D\`

### AssetBundles

The `AssetBundles\` folder is simply used for loading Unity AssetBundles.

You <b>only need to place the actual AssetBundle</b> in here, you do <b>not</b> need to include the ".meta" or ".manifest" files, nor do you need to include the generic "AssetBundles" file.

AssetBundles which you place in this folder can be referenced by the <b>file name</b>.

See also: [Unity AssetBundles Guide](https://docs.unity3d.com/Manual/AssetBundles-Workflow.html)

?> <b>Note:</b> You must use the same version of the Unity Editor that Outward uses to build your asset bundles. Currently, this is <b>[Unity 2018.4.8](https://download.unity3d.com/download_unity/9bc9d983d803/Windows64EditorInstaller/UnitySetup64-2018.4.8f1.exe)</b>.

### AudioClip

The `AudioClip\` folder is used for <b>.wav</b> files, either for replacing existing game sounds/music or for custom audio for your own mods.

Your .wav files simply need to be placed inside the AudioClip folder. To replace Audio, use the same name that the game uses. 

See also: [Replacing Audio](Guides/ReplacingAudio.md) for more details.

### Characters

The `Characters\` folder is used for defining custom Characters.

You can also define a Skill Tree with XML here through an `SL_CharacterTrainer` template.

See [Custom Characters](Guides/Characters.md).

### DropTables

The `DropTables\` folder is used to define an SL_DropTable which is used by other parts of your pack.

See [SL_DropTable](API/SL_DropTable) for more details.

### Enchantments

The `Enchantments\` folder is used for defining custom Enchantments. Simply place the XML files in here, do not place them in a sub-folder.

See [SL_EnchantmentRecipes](API/SL_EnchantmentRecipe.md).

### Items

The `Items\` folder is used for defining custom Items and Skills.

You can place custom item xmls directly in this folder if you want (eg. `Items\MySimpleItem.xml`).

You can also create a <b>sub-folder for each item</b>. This is used if you are defining any <b>custom icons or textures</b> for the item (explained in more detail on the Custom Items page).

See also: [Custom Items](Guides/Items.md).

### ItemSources

The `ItemSources\` folder can be used to define custom Item Sources, to put items into the world.

`SL_ItemSource` XMLs should be placed directly in this folder.

See [SL_ItemSource](API/SL_ItemSource.md) for more details.

### Recipes

The `Recipes\` folder is used to define custom recipes. Simply place the XML files in this folder, do not place them in a sub-folder.

See also: [SL_Recipes](API/SL_Recipe.md).

### StatusEffects

The `StatusEffects\` folder is for custom Status Effects and Imbues. Similar to custom items, you can put the XMLs directly in this folder or you can create a folder for each status.

See also: [Custom Statuses](Guides/StatusEffects.md)

### StatusFamilies

The `StatusFamilies\` folder is used to define custom Status Effect Families.

See [SL_StatusEffectFamily](API/SL_StatusEffectFamily.md) for more details.

### Tags

The `Tags\` folder is used to define Custom Tags.

You can define `SL_TagManifest` XMLs in here (or using the SL Menu), and these tags will be created for you or others to use in the game.

See [SL_TagManifest](API/SL_TagManifest.md) for more details.

### Texture2D

The `Texture2D\` folder is used for loading or replacing textures. SideLoader will find currently loaded textures which have the same name and replace them.

You can also make a sub-folder inside the Texture2D folder called `Local\`. Textures which are placed in here are <b>not</b> used for global replacements, but they can be accessed through your SL Pack for other things. This should be used if you don't want to automatically replace textures based on the texture's name.

See also: [Replacing Textures](Guides/ReplacingTextures.md)
