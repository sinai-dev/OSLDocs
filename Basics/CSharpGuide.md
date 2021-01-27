# C# Overview

This is a brief summary of how to use the SideLoader directly from your own C# mod.

## Adding the Reference

1. <b>Install SideLoader</b> to the Outward/BepInEx/plugins folder.
2. In your C# Project, add a reference to <b>SideLoader.dll</b> from this location.
3. Put `using SideLoader;` at the top of classes where you want to use SideLoader.
4. Add the BepInDependency attribute to your plugin class, as shown below:

```csharp
[BepInDependency(SLPlugin.GUID, BepInDependency.DependencyFlags.HardDependency)]
[BepInPlugin("com.me.myname", "My Mod", "1.0")]
public class MyPlugin : BaseUnityPlugin
// ...
```

## Events
One of the most important features of SideLoader in C# is the events.

To subscribe to an event in C#, simply put something like this in your `Awake()` function:
* `SL.OnPacksLoaded += MySetup;` where `MySetup` is a method.

The major events you can subscribe to are:

`SL.BeforePacksLoaded` 
* called right before SL Packs are loaded and applied. This is <b>after</b> ResourcesPrefabManager has loaded.

`SL.OnPacksLoaded` 
* called after all packs have been loaded and applied. This will also be after ResourcesPrefabManager has loaded.

`SL.OnSceneLoaded` 
* called when a scene has truly finished loading. This is <b>not</b> called for the Main Menu or for the "Low Memory Transition Scene".

`SL.OnGameplayResumedAfterLoading`
* called when the gameplay actually resumes, after a scene has loaded.

The order of the initial SideLoader setup in regards to the events is:
```
ResourcesPrefabManager.Load
V
SL.BeforePacksLoaded
V
// SideLoader sets up and applies all SL Packs and prepared templates
V
SL.OnPacksLoaded
```

## C# API

For the main C# API features, see [C# API](API/CSharpAPI.md). For more specific features, there will be a C# tab on the relevant API pages.