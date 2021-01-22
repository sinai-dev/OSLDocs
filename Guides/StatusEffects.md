# Custom Status Effects

?> SideLoader has received a major update to its menu in v3.2.0, these docs are not yet updated for the changes.

<b>Custom Status Effects</b> can be used to make completely new Status Effects and Imbues or modify existing ones. You can do this either from an SL Pack, or directly from your own C# mod.

The SL Pack sub-folder for custom Status Effects is `StatusEffects\`.

## Overview

Use the <b>[SideLoader Menu](Basics/SLMenu.md)</b> to generate templates for you from existing Status Effects or Imbues. It will also dump the icon and set it up in an SLPack structure ready to go for you.

Your custom status XML can be placed in the `StatusEffects\` sub-folder of your [SL Pack](Basics/SLPacks.md). The XML can be named anything.
* Eg, `[SLPack Folder]\StatusEffects\MyStatus.xml`

If you want to use a custom icon, you need to create a <b>sub-folder for each status</b>.
* Eg, `[SLPack Folder]\StatusEffects\MyStatus\MyStatus.xml`
* The icon needs to be called <b>icon.png</b> and placed in this same folder as the xml.
* For LevelStatusEffects, you can also have an icon for each additional level of the status, which must be named "icon2.png", "icon3.png", etc.

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

/* If setting a custom icon, set these next two values to do so. */

/* The SLPack folder name which you want to use */
template.SLPackName = "MyPackName"; 

/* The subfolder name in the StatusEffects/ folder of the SLPack for this custom status.
 * Your icon should be called "icon.png" and placed directly in this folder.
*/
template.SubfolderName = "MySubfolderName"; 

/* when you're done, call this to prepare and register the template. */
template.Apply();
```
