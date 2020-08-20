# Custom Item Visuals

<b>Custom Item Visuals</b> (including icons, textures or entire prefabs) can be defined either through your SL Pack, or directly from C#.

There are three main features SideLoader offers related to Item Visuals:
* Setting and modifying the textures using <b>.png</b> and <b>.xml</b> files.
* Setting and modifying visual prefabs using <b>AssetBundles</b> and <b>.xml</b> files.
* Using AssetBundles to make bulk texture changes quickly.

## Replace Textures with PNGs

First, you <b>need to create a sub-folder for the custom item</b> inside your `Items` folder if you want to use custom png files.
* It should look like `[SLPack]/Items/[MyItem]/`.

You should then create a sub-folder inside that item folder called "<b>Textures</b>". 
* This Textures sub-folder should look like `[SLPack]/Items/[MyItem]/Textures/`.

If you generated a template with the [SL Menu](GettingStarted/SLMenu), it will include the icons and textures in this setup for you.

A sub-folder is also made inside the Textures folder for each <b>material</b> on the item visuals, if any.

<b>The names of the .png files you place in here must use these exact names!</b>

These two files go in the base `Textures\` folder:

`icon.png` (84x128px)
* Replaces the <b>main item icon</b>
* A blank icon template can be found here: [for items](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/ItemIcon.png), or [for skills](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/ActiveSkillIcon.png), or [for passives](https://github.com/sinai-dev/Outward-SideLoader/blob/master/Resources/PassiveSkillIcon.png)

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

`ShaderName` (string)
* Sets the <b>Shader</b> that will be used on the Material.
* Unless you know how to find the Shader names, just copy this from other materials if you need to.
* You can also set this to Unity's default shaders, such as PBR.

`Keywords` (list of string)
* A list of `<string>` values which determines the <b>enabled keywords</b> on the Shader.
* Keywords are used to enable certain special features of the Shader.

`Properties` (list of ShaderProperty)
* A list of `ShaderProperty` objects, used to set properties on the shader.
* See the ShaderProperty section below.

`TextureConfigs` (list of TextureConfig)
* A list of `TextureConfig` objects, used to set some config values for the Textures.
* See the TextureConfig section below.

#### ShaderProperty

A `ShaderProperty` only has one value, called `Value`. However, the type of this value will vary depending on the type of ShaderProperty.

* A <b>FloatProperty</b> will expect a <b>float</b> value (number).
* A <b>ColorProperty</b> will expect a <b>Color</b> value (`r`, `g`, `b`, `a`)
* A <b>VectorProperty</b> will expect a <b>Vector4</b> value (`x`, `y`, `z`, `w`)

#### TextureConfig

A `TextureConfig` has a few simple values which determine how SideLoader loads the associated Texture.

`TextureName` (string)
* Must match the name of one of the textures being set on this material.

`UseMipMap` (true/false)
* Default `true`
* If true, will enable <b>MipMap</b> for this texture (adjusts resolution dynamically per distance)

`MipMapBias` (integer)
* If `UseMipMap = true`, this will set the MipMap Bias level. Only set if you know what you're doing.

## Changing Visual Prefabs

There are three fields in the SL_Item Template which relate to the custom visual prefabs. Each of these fields represent a SL_ItemVisual object, where you can define the data for each type of visuals.

`SL_Item.ItemVisuals`
* The base visuals, also used for the equipped visuals if no SpecialItemVisuals are set.

`SL_Item.SpecialItemVisuals`
* The special (alternate) visuals, commonly used for the <b>equipped</b> visuals.

`SL_Item.SpecialFemaleItemVisuals`
* The special <b>female</b> visuals, when there is a female alternate version.

For each of those three fields, you can define either a SL_ItemVisual or SL_ArmorVisuals object. Generally though you should match the type of SL_ItemVisual you are replacing. You can only replace prefabs if the target item also used the same type of visuals at the moment.

### SL_ItemVisual

These are the fields you can set on a SL_ItemVisual object.

?> <b>Note:</b> the `Prefab_` fields are not required. They're only used if you are using your own custom Visual Prefabs.

`ResourcesPrefabPath` (string)
* If the custom `Prefab_` Visuals are <b>not</b> set, you can use this to transmog the visual prefab to a different Item's visuals.
* You can copy this from a different item for easy "transmogs".

`Prefab_SLPack` (string)
* The name of the SLPack you are using for the visuals.
* This means you can use Asset Bundles found in other SLPacks, if you want to.

`Prefab_AssetBundle` (string)
* The <b>file name</b> of the asset bundle in your SL Pack that contains your item visual prefab
* For example, if the bundle is `MyPack/AssetBundles/mybundle`, then put `mybundle` as your setting.

`Prefab_Name` (string)
* Set this to the name of the <b>GameObject</b> in the AssetBundle for your visual prefab.
* Make sure you actually made a GameObject out of your 3D model and you're not trying to load a Mesh object directly.

#### Vector3 Settings {docsify-ignore}

The next 4 settings are Vector3 settings. In xml, a Vector3 looks like this:
```xml
<!-- Example is Position. 
 Same goes for Rotation, PositionOffset, and RotationOffset -->
<Position> 
  <x>0</x>
  <y>0</y>
  <z>0</z>
</Position>
```

`Position` (Vector3)
* Allows you to directly set the position. This will override any setting from the original item or from Unity Editor.
* If you don't set this (ie. delete or comment it from the template) then SideLoader will copy the values from the existing visuals.

`Rotation` (Vector3)
* Allows you to directly set the rotation. This will override any setting from the original item or from Unity Editor.
* If you don't set this (ie. delete or comment it from the template) then SideLoader will copy the values from the existing visuals.

`PositionOffset` (Vector3)
* An offset applied to the position. You can use this to copy the position of the original visuals, and only apply an offset from that.

`RotationOffset` (Vector3)
* An offset applied to the rotation. You can use this to copy the rotation of the original visuals, and only apply an offset from that.

### SL_ArmorVisuals
SL_ArmorVisuals inherits from SL_ItemVisual, and contains two extra fields.

`HideFace`
* For head or chest armor only. Does equipping these visuals hide the player's Face?

`HideHair`
* For head or chest armor only. Does equipping these visuals hide the player's hair?

### Xml Example
Here is an Xml Example of an SL_ItemVisual object.

```xml
<ItemVisuals>
    <Prefab_SLPack>TsarRevolver</Prefab_SLPack>
    <Prefab_AssetBundle>tsarrevolver_bundle</Prefab_AssetBundle>
    <Prefab_Name>TsarRevolver_prefab</Prefab_Name>
    <!-- 
    I don't define Position or Rotation, so they are copied from the original.
	I simply use the PositionOffset and RotationOffset to adjust my visuals slightly.
	This was figured out with some trial and error.
    -->
    <PositionOffset>
        <x>-0.02</x>
        <y>0.075</y>
        <z>0.08</z>
    </PositionOffset>
    <RotationOffset>
        <x>-18</x>
        <y>90</y>
        <z>6</z>
    </RotationOffset>
</ItemVisuals>

<SpecialItemVisuals>
<!-- etc... -->
```

## Custom Visual Prefabs

There are two parts to setting up your custom Visual Prefabs.

### Creating the AssetBundle

!> <b>Note:</b> You must use the same version of the Unity Editor that Outward uses to build your asset bundles. Currently, this is <b>[Unity 2018.4.8](https://download.unity3d.com/download_unity/9bc9d983d803/Windows64EditorInstaller/UnitySetup64-2018.4.8f1.exe)</b>.

First, make sure you are familiar with the [AssetBundle Workflow](https://docs.unity3d.com/Manual/AssetBundles-Workflow.html) in the Unity Editor as well as working with [SL Pack AssetBundles](SL-Packs#assetbundles).

Secondly, note that custom visual prefabs for <b>Armor</b> or <b>Bows</b> is not supported yet. If you figure it out, please let me know! You need to work out how to use the game's SkinnedMeshRenderer and Human_v.fbx rig to set up your own armor .fbx correctly with Blender.

For standard Item Visual prefabs, all you have to do is:
* Create a <b>GameObject</b> out of your model.
* Add a <b>BoxCollider</b> to this gameobject
* The base GameObject <b>must have a MeshRenderer on it</b>. It can just be a blank MeshRenderer, but the component must exist.

!> <b>Important!</b> Make sure that the <b>scale</b> of your base Transform is <b>(1.0, 1.0, 1.0)</b>. If you need to scale the model, use the "Import Settings" on the .fbx in the Unity Editor. 

Once you set that up, build it to an asset bundle and put it in your `[MyPack]/AssetBundles/` folder.

### Set the SL_ItemVisual

Once you've created the AssetBundle, you'll need to set the `Prefab_SLPack`, `Prefab_AssetBundle` and `Prefab_Name` in the SL_ItemVisual template. See above for more details.

## AssetBundle Textures
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

Launch the game, and SideLoader should log that it loaded and applied your textures in the `output_log.txt` file.