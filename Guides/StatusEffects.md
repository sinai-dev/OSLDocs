# Custom Status Effects

<b>Custom Status Effects</b> can be used to make completely new Status Effects and Imbues or modify existing ones. You can do this either from an SL Pack, or directly from your own C# mod.

The SL Pack sub-folder for custom Status Effects is `StatusEffects\`.

## Overview

Even if you're using C#, I recommend doing this to see how Statuses work.

Use the <b>[SideLoader Menu](Basics/SLMenu.md)</b> to generate templates for you from existing Status Effects or Imbues. It will also dump the icon and set it up in an SLPack structure ready to go for you.

Note - to get the ID of a Status Effect to clone from, use [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1969601658) - find your effect on the list. For Status Effects, use the "Identifier Name", for Imbue Effects use the number to the left of it.

Pick a Status Effect similar to what you want to make (or pick the Status Effect you want to edit), and generate a template for it.

Your custom status XML can be placed in the `StatusEffects\` sub-folder of your [SL Pack](Basics/SLPacks.md). The XML can be named anything.
* Eg, `[SLPack Folder]\StatusEffects\MyStatus.xml`

If you want to use a custom icon, you need to create a <b>sub-folder for each status</b>.
* Eg, `[SLPack Folder]\StatusEffects\MyStatus\MyStatus.xml`
* The icon needs to be called <b>icon.png</b> and placed in this same folder as the xml.

See [SL_StatusEffect](API/SL_StatusEffect.md) and [SL_ImbueEffect](API/SL_ImbueEffect.md) for more details.

## C# Example
```csharp
// in your Awake() or BeforePacksLoaded method:
var template = new SL_StatusEffect 
{
    TargetStatusIdentifier = "Burning",
    StatusIdentifier = "MyNewEffect",
    NewStatusID = 5012,
    // etc...
};

template.Apply();
```
