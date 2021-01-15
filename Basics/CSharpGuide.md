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