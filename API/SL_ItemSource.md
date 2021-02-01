# SL_ItemSource

`SL_ItemSource` can be used to define a source for an item, whether the item is custom or existing.

You can define an `SL_ItemSource` through the [SL Menu](Basics/SLMenu.md) and the `ItemSources` SLPack folder, or through C#.

<!-- tabs:start -->

#### ** Universal **

The base class SL_ItemSource is abstract, it cannot be used directly. You can define an Item Spawn (SL_ItemSpawn) or add onto a Drop Table (SL_DropTableAddition). In either case, you will have the following field:

`IdentifierName` (string)
* Your <b>unique</b> Identifier Name for this item source, eg "com.myname.myitemsource1".
* This is required to be unique for `SL_ItemSpawn`, but for `SL_DropTableAddition` its only used for logging purposes.

### SL_ItemSpawn

An SL_ItemSpawn can be used to define a world spawn for an Item. You can also use this to place static non-pickable objects as scenery.

`ItemID` (integer)
* The Item ID of the item that will spawn.

`Quantity` (integer)
* The quantity of the item that will spawn (only works it is stackable), default is 1.

`SceneToSpawnIn` (string)
* The build name of the Scene that this spawn is for.
* You can get build names off the Wiki, and the SL Menu shows your current scene.

`SpawnPosition` (Vector3)
* The world position of the SL_ItemSpawn

`SpawnRotation` (Vector3)
* The starting rotation of the spawned item

`ForceNonPickable` (bool)
* If true, the item spawn will <b>not be pickable</b>, meaning players cant pick it up.

`TryLightFueledContainer` (bool)
* If the item is a Fueled Container (ie. Lantern), should SL try to light it on spawn / reload?

### SL_DropTableAddition

An SL_DropTableAddition can be used to "add on" to an existing drop table or Merchant in the game any time it is rolled on.

You can declare 1 or more SL_DropTable UIDs which will be rolled alongside the original target.

<b>Notes:</b>
* Your drops <b>will not</b> affect the existing drop rates if you use SL_DropTableAddition
* Your items will always appear as the first items in the window

`SelectorTargets` (list of string)
* The targets to add on to, which can either be <b>the name of a DropTable</b> or <b>the UID of a Merchant</b>.
* All DropTable names start with "DropTable_". You can find DropTable names [on this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1530496898). If you use "DT_" instead this will also work, SL will fix the name for you.
* You can get Merchant UIDs using [this sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=2082872385).

`DropTableUIDsToAdd` (list of string)
* A list of `SL_DropTable.UID`s which will be rolled alongside the original selector targets.
* You <b>cannot</b> declare existing game drop tables here, only `SL_DropTable` UIDs.

#### ** C# Only **

From C#, all you have to do is call `mySource.ApplyTemplate()` once you have defined it.

<!-- tabs:end -->