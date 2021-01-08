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

#### ** XML **

We are going to use the [SL Menu](Basics/SLMenu.md) to generate a template from an existing Item for us.

1. Go into your Keybindings and set a binding for the SL Menu if you have not already, then open the menu.
2. In the Items section, enter `2000010` as the Item ID. 
3. Press the button to generate the template.

Let's find the template SideLoader generated.

1. Press the button at the top of the menu to open the "_GENERATED" folder (`Outward\Mods\SideLoader\_GENERATED\`)
2. Inside here, you should see an `Items\` sub-folder
3. Inside Items, you should see a folder called `2000010_IronSword\`.

Now we need to move this folder into our own SL Pack.

1. <b>Cut</b> this Iron Sword folder (Ctrl+X)
2. Go back to the `SideLoader\` folder. Create a new folder in here called `Test\`. 
3. Create a sub-folder in Test called Items (`Test\Items\`)
4. <b>Paste</b> (Ctrl+V) the Iron Sword folder in here. 

It should now look like this: `Outward\Mods\SideLoader\Test\Items\2000010_IronSword\`.

Note: You can also put the XML file directly in the `Items\` folder if you don't want to change the textures or icons.

Now let's make a basic change to the Item.

1. Open the `Iron Sword.xml` file and have a look. 
2. Replace the `<Description />` value with this: `<Description>Test.</Description>`

Before we start the game and inspect our changes, make sure you have enabled the <b>Outward Debug Menu</b> first:
1. Open the folder `Outward\Outward_Data\`
2. Create a .txt file called "DEBUG", it should look like `DEBUG.txt`. <b>Note:</b> if you have "Show File Extensions" disabled, it will need to look like "DEBUG" instead of "DEBUG.txt".

With Debug Mode enabled, start Outward. Once in-game, press <b>F1</b> to open the Item Spawner, then spawn an Iron Sword and have a look, our description should now be set.

![It worked!](https://i.imgur.com/UxuA8ky.png)

If there were any errors, SideLoader should log them in the file `Outward\BepInEx\LogOutput.log`.

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

