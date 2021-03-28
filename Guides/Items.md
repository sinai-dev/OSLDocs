# Custom Items

<b>Custom Items</b> can be used to make completely new items or modify existing ones. This includes generic items, consumables, equipment, weapons, and skills. You can do this either from an SL Pack, or directly from your own C# mod.

Make sure you have first had a quick read of the [SL Packs](Basics/SLPacks) article to understand how to work with SideLoader from folders. No knowledge of C# is necessary for this.

Your custom item XML can be placed in the `Items\` sub-folder of your [SL Pack](Basics/SLPacks.md). The XML can be named anything.
* Eg, `[SLPack Folder]\Items\MyItem.xml`

If you want to use custom Item Visuals (including changing the icons or the textures), you need to create a <b>sub-folder for each item</b> - this is true for C# as well, unless you do it manually.
* Eg, `[SLPack Folder]\Items\MyItem\MyItem.xml`

Even if planning on using C#, I recommend first being familiar with the SLPack approach as you will be using the same folder structure if you want to load any custom textures or icons for your items.

For full documentation about the SL_Item class, see [SL_Item](API/SL_Item.md).

## Examples

<!-- tabs:start -->

#### ** XML/SLMenu **

Make sure you have enabled the <b>Outward Debug Menu</b> for this guide first:
1. Open the folder `Outward\Outward_Data\`
2. Create a .txt file called "DEBUG", it should look like `DEBUG.txt`. <b>Note:</b> if you have "Show File Extensions" disabled, it will need to look like "DEBUG" instead of "DEBUG.txt".

We are going to use the [SL Menu](Basics/SLMenu.md) to generate a template from an existing Item for us.

1. Go into your Keybindings and set a binding for the SL Menu if you have not already, then open the menu.
2. Create an SL Pack or select an existing one
3. Select the "Items" category of the SL Pack (it is selected by default)
4. In the Target ID field, enter `Iron Sword` and click on the Iron Sword option of the auto-completer. Alternatively, just enter `2000010`.
5. Press the button to generate the template.

Now let's make a basic change to the Item.

1. Change the `SL_Item.Description` by typing into the input field on that row and pressing "Apply".
2. Press "Save XML" to save the changes to disk.
3. Press the Hot Reload button up the top-left to reload all SL Packs.
4. Load a character and press <b>F1</b> to open the Item Spawner, then spawn an Iron Sword and have a look, our description should now be set.
5. <b>Note:</b> Existing spawned objects will not be affected by a Hot Reload unless you go back to the menu or reload the scene first.

![It worked!](https://i.imgur.com/UxuA8ky.png)

If there were any errors, SideLoader should log them in the <b>Debug Console</b> of the menu, and also to the file `Outward\BepInEx\LogOutput.log`.

#### ** C# **

```csharp
using SideLoader;

// namespace ... class ...

// In your plugin's Awake method:
internal void Awake() 
{
    SetupMyTemplate();

    SL.OnPacksLoaded += OnPacksLoaded;
}

private void SetupMyTemplate()
{
    /* You can define any SL_Item class, we'll use SL_Weapon for this example. */
    var template = new SL_Weapon()
    {
        Target_ItemID = 2000010, // iron sword
        New_ItemID = 2000010,

        Name = "MyItem",
        StatsHolder = new SL_WeaponStats()
        {
            // ...
        },
        // etc...
    };

    /* If setting any custom item visuals or icons, you need to set these next two values. */

    /* I recommended using the SL Menu to generate a template from an existing Item to see the correct structure. */
    /* For more information, see the Custom Item Visuals article. */

    /* The SLPack folder name which you want to use */
    template.SLPackName = "MyPackName"; 

    /* The subfolder name in the Items/ folder of the SLPack for this custom item.
     * If you're setting an icon or custom textures from .png files, you need to set this.
     * Your .png files need to be in a "Textures/" subfolder inside this subfolder, and use the names as described on the Custom Item Visuals page.
    */
    template.SubfolderName = "MyItemSubfolderName"; 

    /* when you're done, call this to prepare and register the template. */
    template.Apply();
}

private void OnPacksLoaded()
{
    /* SideLoader has finished setting up all custom content, now is a safe time to do other changes that depend on that. */
    var myItem = ResourcesPrefabManager.Instance.GetItemPrefab(myTemplateID);

    // etc ...
}
```

<!-- tabs:end -->

## See also
* [SL_Item](API/SL_Item.md)

