# SL Overview

## How are SideLoader mods made?
There are two ways you can use SideLoader:
1. Using <b>SL Packs</b> (folders containing <b>XML</b> files and other assets)
2. Using the <b>C#</b> API from your own plugin

This documentation primarily focuses on SL Packs and will use XML examples over C# examples, however all of the same rules apply to C# as well. See the [C# API](https://sinai-dev.github.io/_docfx/api/SideLoader.html) for C#-specific documentation, but note that it does not usually go into as much detail as these articles will.

To get the most out of SideLoader, it is recommended to use both the SL Pack folders to load your assets, and the C# API for extended features.

## What can SideLoader do?

SideLoader's features can mostly be broken down into two categories: <b>Simple Replacements</b> of textures and audio, and more complex <b>Custom Assets</b> such as Items, Status Effects, Characters and more.

As well as this, SideLoader offers some additional C# API, and various other helpers for asset loading and replacement, or custom content.

### Simple Replacements

SideLoader allows you to replace certain assets (currently `.png` textures and `.wav` audio) with your own files directly. This is designed to be as simple as possible, all you have to do is use the same file name that the game uses to replace the asset, and place it in the corresponding [SLPack folder](GettingStarted/SLPacks). 

### Custom Assets
SideLoader also offers an expansive API for all sorts of custom game assets such as Items, Status Effects, Characters and more. You can use this API to <b>edit existing content</b>, or <b>create completely new content</b> as well.

You can do this with no knowledge at all of C# or programming, all you need to do is use the [SL Menu](GettingStarted/SLMenu) to generate templates for you from existing content and edit it as you like. If you do know C# you can also make use of SideLoader's C# API for some additional features.

In a nutshell, SideLoader mirrors the game's classes and strips away everything unnecessary (or non-serializable) so that we can just use simple XML documents to create and edit them. You can also make use of custom textures and even custom AssetBundles for item visuals.

## Editing Etiquette
When using SideLoader to <b>edit existing assets</b>, you should be courteous of other modders and only include things in your template if you are actually changing them. This allows different types of SideLoader mods to be compatible with each other.

For example, one mod might make changes to lots of item descriptions, and another might make changes to their textures. These two mods would be compatible with each other assuming both authors only included the things they are changing.

The SL Menu can generate templates for you from existing assets, and it will include everything you might want to change when it generates the template. However you don't need to include all of the output when you are making your mod, and in fact you should delete everything you are not changing, other than the few required fields.

When browsing the documentation for the related asset you are making, a few fields will be marked as <b>[REQUIRED]</b> near the top, and these are the only required parts.

## F.A.Q ##
### What should I do to get started? {docsify-ignore}
See the articles in the <b>Getting Started</b> sub-heading, and then follow along with [Guides](GettingStarted/Guides.md).

### Can I use this to edit existing Items/Skills/etc? {docsify-ignore}

Yes, SideLoader allows you to control whether you are creating a completely new asset or just editing an existing one. When editing an existing item or status effect, you should remove (delete or comment out) all other things on the template that you don't want to change, to avoid conflicts with other SideLoader edits to the same item.

### How can I know the possible values for X setting on a template? {docsify-ignore}

Check the wiki page for that kind of template, or the Enums / google doc resources.

### Does this work in multiplayer? {docsify-ignore}

Yes, but it <b>doesn't</b> "sync" your changes or custom items, you need to make sure all players have the same SideLoader mods enabled if you want it to be synced. 

### Can I load custom Levels using this? {docsify-ignore}

There is some limited support for it from C# with the `CustomScenes` class, but it's not well tested yet.
