# Custom Item Visuals

<b>Custom Item Visuals</b> (including icons, textures or entire prefabs) can be defined either through your SL Pack, or directly from C#.

There are three main features SideLoader offers related to Item Visuals:
* Setting and modifying the textures using <b>.png</b> and <b>.xml</b> files.
* Setting and modifying visual <b>prefabs</b> using <b>AssetBundles</b> and <b>.xml</b> files.
* Using AssetBundles to make bulk texture changes quickly.

SideLoader has some C# API for this with the `CustomTextures` and `CustomItemVisuals` classes as well.

## Setting Item Textures with PNGs

First, you <b>need to create a sub-folder for the custom item</b> inside your `Items` folder if you want to use custom png files.
* It should look like `[SLPack]/Items/[MyItem]/`.

You should then create a sub-folder inside that item folder called "<b>Textures</b>". 
* This Textures sub-folder should look like `[SLPack]/Items/[MyItem]/Textures/`.

If you generated a template with the [SL Menu](Basics/SLMenu.md), it will include the icons and textures in this setup for you.

A sub-folder is also made inside the Textures folder for each <b>material</b> on the item visuals, if any.

<b>The names of the .png files you place in here must use these exact names!</b>

These two files go in the base `Textures\` folder:

`icon.png` (84x128px)
* Replaces the <b>main item icon</b>
* A blank icon template can be found here: [for items](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/Icons/ItemIcon.png), or [for skills](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/Icons/ActiveSkillIcon.png), or [for passives](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/Icons/PassiveSkillIcon.png)

`skillicon.png` (50x50px)
* The small icon used for <b>Skill Tree icons</b>
* Just a small version of the main icon.

You can also replace the <b>Material Textures</b>. A sub-folder is created for each material on the Item Visuals you clone from, and you can edit the layers on these materials as you desire.

Unless you are an advanced user, I would recommend only generating these material textures with Debug Menu, and just changing those files.

`_MainTex.png`
* This is the primary albedo texture.

`_NormTex.png`
* The normal map

`_GenTex.png` 
* Generative texture which uses RGB channels for separate purposes
* Specular (R), Gloss (G), Occlusion (B)

`_SpecColorTex.png`
* Used to add color to the specular map in some cases (RGB)

`_EmissionTex.png`
* Emissive map (glow map)

### SL_Material

SideLoader allows you to adjust the Material and Shader properties through xml files. This is done with a file called <b>properties.xml</b>, which must be placed in the Material sub-folder for the textures.

When you generate a template with the SL Menu, it will include these xml files in the correct structure for you.

?> <b>Tip:</b> you can copy the properties.xml (or sections of it) from one material to another.

See [SL_Material](API/SL_Material.md) for documentation on this class.

## Setting Visual Prefabs

There are three fields in the [SL_Item](API/SL_Item.md) Template which relate to the custom visual prefabs. Each of these fields represent a SL_ItemVisual object, where you can define the data for each type of visuals.

`SL_Item.ItemVisuals`
* The base visuals, also used for the equipped visuals if no SpecialItemVisuals are set.

`SL_Item.SpecialItemVisuals`
* The special (alternate) visuals, commonly used for the <b>equipped</b> visuals.

`SL_Item.SpecialFemaleItemVisuals`
* The special <b>female</b> visuals, when there is a female alternate version.

For each of those three fields, you can define either a SL_ItemVisual or SL_ArmorVisuals object. Generally though you should match the type of SL_ItemVisual you are replacing. You can only replace prefabs if the target item also used the same type of visuals at the moment.

See [SL_ItemVisual](API/SL_ItemVisual.md) for details about these classes.

## Custom Visual Prefabs

SideLoader supports setting custom visual prefabs for items, provided that you set them up correctly in the Unity Editor.

?> <b>Note:</b> You must use the same version of the Unity Editor that Outward uses to build your asset bundles. Currently, this is <b>[Unity 2018.4.8](https://download.unity3d.com/download_unity/9bc9d983d803/Windows64EditorInstaller/UnitySetup64-2018.4.8f1.exe)</b>.

### Creating the Prefab

* Import your mesh into the Unity Editor and drag it into the Scene hierarchy to create a GameObject out of it.
* There must be a `MeshRenderer` or `SkinnedMeshRenderer` on the base GameObject.
* Add a `BoxCollider` to this GameObject if you are setting the normal Item Visuals.
* If setting Special Visuals (used for the equipped version), you must also make a version WITHOUT the BoxCollider.
* Drag this GameObject to the Assets window to create a Prefab out of it.

!> <b>Important!</b> Make sure that the <b>scale</b> of your base Transform is <b>(1.0, 1.0, 1.0)</b>. If you need to scale the model, use the "Import Settings" on the .fbx. 

### Creating the AssetBundle

First, make sure you are familiar with the [AssetBundle Workflow](https://docs.unity3d.com/Manual/AssetBundles-Workflow.html) in the Unity Editor as well as working with [SL Pack AssetBundles](SL-Packs#assetbundles).

* In the project, create a folder called "Editor", and create a new C# script inside this folder.
* Copy and paste the script provided in the "Building the Asset Bundles" section of the link above.
* You should now see the option to "Build AssetBundles" in the Assets drop-down menu at the top of the Editor.
* Once you set that up, build your prefab to an asset bundle and put it in your `AssetBundles/` folder of your SLPack.

### Set the SL_ItemVisual

Once you've created the AssetBundle, you'll need to set the `Prefab_SLPack`, `Prefab_AssetBundle` and `Prefab_Name` for the SL_ItemVisuals in the SL_Item template. See [SL_ItemVisual](API/SL_ItemVisual.md) to read more about these fields.

You should now see your model when you spawn your item.

## Bulk Item Textures from AssetBundles
SideLoader supports setting textures from an AssetBundle. This is mainly used for bulk texture changes, since doing so with .png files would take an extremely long time.

These special AssetBundles must be placed in a folder called `TextureBundles` (case sensitive) in the `Items` folder of your SL Pack. For example, `MyPack\Items\TextureBundles\mybundle`.

#### Step 1: {docsify-ignore}
Firstly, create a new Unity Project. Use Unity version 2018.4.

In the project, create a folder called "Editor", and create a new C# script inside this folder.

Copy and paste the script provided here (in the "Building the Asset Bundles" section): [Unity: AssetBundle workflow](https://docs.unity3d.com/Manual/AssetBundles-Workflow.html).

You should now see the option to "Build AssetBundles" in the Assets drop-down menu at the top of the Editor.

#### Step 2: {docsify-ignore}
Now you need to create a folder corresponding to each Item you want to set. These folders should be placed directly into the root "Assets" folder of your project. Your "Assets" folder can be thought of as the same as the "Items" folder in your SL Pack.

!> <b>Note:</b> These folder names <b>must start with</b> the Item ID of the item the textures correspond to. When generating from the SL Menu, it will be in this structure by default. You can put whatever you want after the ID, but it must start with the ID.

If you generated the items using the SL Menu, you can simply copy the folder (eg. `2000010_IronSword`) and drag it into your Unity Project into the Assets window.

The "Textures" sub-folder that is created by SideLoader is optional, it will work whether or not you use it. So `2000010_IronSword\Textures\...` would work, as would `2000010_IronSword\...`.

You can place the `icon.png` and the `skillicon.png` in the base Item folder (or the Textures sub-folder).

Each material folder should have its own sub-folder, as per the generated format.

It should look something like this:

![The Unity Project structure](https://i.imgur.com/OG93Kfe.png)

#### Step 3: {docsify-ignore}
Now, go back to the base Assets folder and select all of the Item folders. 

In the bottom right of the Editor next to AssetBundle, click "None" and then click "New..." and pick any name for the bundle.

![Building the Asset Bundles](https://i.imgur.com/UWjZ06i.png)

Now select from the navigation bar: `Assets > Build Assetbundles`.

Right click your Assets window and select "Show in Explorer" (or navigate to your Unity project Assets folder manually). Open the Assets folder, then open the AssetBundles folder, and select your bundle file.

<b>Note:</b> do not select all the other files (such as the "AssetBundles" file, or the .manifest or .meta files, etc). Only select the AssetBundle file name which you created.

Take this bundle and place it in the `TextureBundles` folder (`MyPack\Items\TextureBundles\`).

![The SideLoader pack structure](https://i.imgur.com/ScW0Pk9.png)

Launch the game, and SideLoader should log that it loaded and applied your textures in the `BepInEx\LogOutput.log` file.