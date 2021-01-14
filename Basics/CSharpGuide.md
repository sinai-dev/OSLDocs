# C# Overview

This is a brief summary of how to use the SideLoader directly from your own C# mod.

## Adding the Reference

1. <b>Install SideLoader</b> to the Outward/BepInEx/plugins folder.
2. In your C# Project, add a reference to <b>SideLoader.dll</b> from this location.
3. Put `using SideLoader;` at the top of classes where you want to use SideLoader.

## Events
One of the most important features of SideLoader in C# is the events.

To subscribe to an event in C#, simply put something like this in your `Awake()` function:
* `SL.OnPacksLoaded += MySetup;` where `MySetup` is a method.

Some of the important events you can subscribe to are:
* `SL.BeforePacksLoaded` is called right before SL Packs are loaded and applied. This is <b>after</b> ResourcesPrefabManager has loaded.
* `SL.OnPacksLoaded` is called after all packs have been loaded and applied. This will also be after ResourcesPrefabManager has loaded.
* `SL.OnSceneLoaded` is called when a scene has truly finished loading. This is <b>not</b> called for the Main Menu or for the "Low Memory Transition Scene".

## SLPack Class

The `SLPack` class is the C# wrapper for SL Pack folders.

Use `SL.GetSLPack(string name)` to get a handle on a pack.
* The Name is the folder name of either: `Mods\SideLoader\{Name}\` or `BepInEx\plugins\{Name}\SideLoader\`

`SLPack.AssetBundles`
* Dictionary of loaded AssetBundles, key is the file name

`SLPack.Texture2D`
* Dictionary of loaded PNGs from the Texture2D folder (not Item or Status PNGs)
* Key is the file name <b>without the .png extension</b>

`SLPack.AudioClips`
* Dictionary of loaded audio clips, key is the file name <b>without .wav</b>

`SLPack.CharacterTemplates`
* Dictionary of loaded `SL_Character` templates, key is the template's `UID`.

Example:
```csharp
// The SLPack "MyPack" is found at either Mods\SideLoader\MyPack or BepInEx\plugins\MyPack\SideLoader\
var pack = SL.GetSLPack("MyPack");

// ..\Texture2D\myTex.png
var myTex = pack.Texture2D["myTex"];

// ..\AssetBundles\mybundle
var bundle = pack.AssetBundles["mybundle"];

// ..\AudioClips\MyAudio.wav
var audio = pack.AudioClips["MyAudio"];

// ..\Characters\mycharacter.xml -> UID: my.character
var myChar = pack.CharacterTemplates["my.character"];
```

## Custom Keybindings

`CustomKeybindings` contains helpers for creating and checking custom in-game keybindings.

```csharp
using SideLoader;

// inside your BaseUnityPlugin.Awake() method:
CustomKeybindings.AddAction("MyKey", KeybindingsCategory.CustomKeybindings, ControlType.Keyboard);

// inside your Update() method:
if (CustomKeybindings.GetKeyDown("MyKey")) 
{
    // the key was pressed this frame
}
if (CustomKeybindings.GetKey("MyKey")) 
{
    // the key is held this frame
}
```

## CustomTags

`CustomTags` is SideLoader's helper for creating or getting a `Tag`. 

`CustomTags.GetTag(string TagName, bool logging = true)`
* Get a Tag easily by providing it's name
* Logging will print to BepInEx's log on failure

`CustomTags.CreateTag(string name)`
* Use this to create a new Tag, by providing the name for it.

## PlayerSaveExtension

The `PlayerSaveExtension` class can be used to extend a player's save easily. You can create a derived class of `PlayerSaveExtension` and implement the abstract methods, and then simply define your save data into the class.

<b>Note:</b> The fields you define must be <b>serializable</b>, which generally means they must be a primitive type (`int`, `bool`, etc) or a `string`. They can also be an `IEnumerable` or `ICollection` which contains primitives/strings, or a struct (or a type with a parameterless constructor) which only contains previously mentioned types. For full details, see [Types Supported by the Data Contract Serializer](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/types-supported-by-the-data-contract-serializer).

Example:
```csharp
public class PlayerSaveExtExample: PlayerSaveExtension
{
    // Declare as many members as you want, but
	// they must be XML-serializable (string, primitive,
	// or a struct or array of strings or primitives).
	public string DateTime;

	// Serialize your class from the character / world here.
	public override void Save(Character character, bool isWorldHost)
	{
		DateTime = System.DateTime.Now.ToShortDateString();
		SL.Log("Saving DateTime as " + DateTime);
	}

	// Your class is loaded from an XML save, apply it to the character / world.
	public override void ApplyLoadedSave(Character character, bool isWorldHost)
	{
		SL.Log("Loaded saved DateTime: " + this.DateTime);
	}
}

// Then in your BaseUnityPlugin Awake() method,
// prepare a singleton of your class to be used
// for serialization. Just call "Prepare()".
var saveExt = new PlayerSaveExtExample();
saveExt.Prepare();
```

## CustomTextures
Helper class for working with `Texture2D`s.

`Texture2D LoadTexture(string filePath, bool mipmap, bool linear)`
* Helper to load a Texture2D from a png at the provided file path (can be relative to `Outward\` folder)
* `mipmap` = whether to generate mipmap for this texture
* `linear` = set true only if this is a Normal Map.

`Sprite CreateSprite(Texture2D tex, SpriteBorderTypes borderType)`
* Helper to create a `Sprite` from a provided `Texture2D`
* `SpriteBorderTypes borderType` = must be one of `NONE` (no border), `ItemIcon` (gives item border offset), or `SkillTreeIcon` (gives small skill tree menu icons offset)

`SaveIconAsPNG(Sprite icon, string dir, string name = "icon")`
* Helper to save a Sprite as a PNG file at the provided directory path and name (don't include ".png")

`SaveTextureAsPNG(Texture2D tex, string dir, string name, bool isDTXnmNormal = false)`
* Helper to save a Texture2D as a PNG file at the provided directory path and name (don't include ".png")
* `isDTXnmNormal` = set true only if saving a DTXnm Normal Map (probably true if saving a normal map used by the game)

# Other C# Helpers

## Mouse Control

SideLoader also provides a helper for taking control of the mouse, in a way that integrates nicely with Outward.

Use this if you have a custom menu or something and want to unlock the mouse and prevent character input.

```csharp
// when you want to unlock the mouse:
SideLoader.Helpers.ForceUnlockCursor.AddUnlockSource();

// when you're done:
SideLoader.Helpers.ForceUnlockCursor.RemoveUnlockSource();
```

## At (AccessTools)
SideLoader's class `At` is a Reflection helper, designed for simplicity and relatively good performance. It can be found in the `SideLoader.Helpers` namespace.

The Methods should all be fairly self-explanatory and easy to follow if you have a basic grasp on C# Reflection, but feel free to ask me in the Outward Discord or anywhere if you need help with this.

For example, to set the private field `m_name` on an `Item`, simply do:

```csharp
SideLoader.Helpers.At.SetField(myItem, "m_name", "MyNewName");
```

The methods you'll likely want to use are:
* `SetField<T>` and `GetField<T>`
* `SetProperty<T>` and `GetProperty<T>`
* `Invoke<T>`

There is also a static class version for each of those methods, for example `GetFieldStatic`, `SetPropertyStatic` or `InvokeStatic`. These methods are not generic because you cannot use static types as generic arguments.

The rest of `At`'s helper methods are generic, so be careful with the instances you pass to them, make sure the generic type actually contains the member you want to access. For example, if you have an instance of a Weapon but it is currently only cast to an Item, you would need to do something like this: `At.SetField(myItem as Weapon, "m_someWeaponField", someValue);`

## UnityHelpers
The `UnityHelpers` class contains a few helpers for working with Unity objects in general.

`DestroyChildren(Transform t)`
* Destroys all children gameobjects on the Transform `t`

`Component FixComponentType(Type desiredType, Component existingComponent)`
* Used to change the type of a component to a different one, while maintaining all possible values.
* First, checks if `existingComponent` is the same type as `desiredType`, or inherits from it.
* If NOT, it will replace the component with a `desiredType` component and copy all possible shared values.
* Returns the resulting Component to you.

`T GetCopyOf<T>(T component, Transform transform) where T : Component`
* Copy the provided component onto the provided transform as a clone.