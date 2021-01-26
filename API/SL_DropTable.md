# SL_DropTable

SL_DropTable is a wrapper used to define a Drop Table, which would be used by other parts of your SL Pack, or for whatever you need.

SL_DropTables can be defined and edited via the SL Menu/XML, or directly from C#.

The SL Pack subfolder for SL_DropTables is `DropTables\`.

<!-- tabs:start -->

#### ** Universal **

`UID` (string)
* Unique ID for your droptable, eg. `com.myname.mydroptable`
* Used by other parts of your SL Pack to reference this drop table.

`GuaranteedDrops` (list of SL_ItemDrop)
* A list of basic guaranteed drops on your table.
* See `SL_ItemDrop` below.

`RandomTables` (list of SL_RandomDropGenerator)
* A list of random sub-tables which will each generate their own drops as well.
* See `SL_RandomDropGenerator` and `SL_ItemDropChance` below.

### SL_ItemDrop
The class `SL_ItemDrop` is a basic wrapper for an item drop.

`DroppedItemID` (integer)
* The Item ID of the dropped item.

`MinQty` (integer)
* The minimum drop quantity, default 1.

`MaxQty` (integer)
* The maximum drop quantity, default 1. <b>Should be at least the same as the Min quantity! (SideLoader will force this to be the case if not)</b>

### SL_ItemDropChance : SL_ItemDrop

The class `SL_ItemDropChance` inherits from `SL_ItemDrop` and contains one extra field.

`DiceValue` (integer)
* The weight of this drop on the random table, in terms of the dice roll.
* Should be <b>at least 1</b>, and as high as you want.
* This determines the drop chance based on the weights compared to the rest of your table and the NoDrop weight. See `SL_RandomDropGenerator` for more details.

### SL_RandomDropGenerator

The `SL_RandomDropGenerator` is a wrapper for the random sub-tables on your drop table.

Each RandomDropGenerator will roll when your SL_DropTable is rolled.

The chance for each drop is not set directly as a % value, but as a Dice Value weight. 

`MinNumberOfDrops` (integer)
* The minimum number of drops rolled from this sub-table. Default is 1.

`MaxNumberOfDrops` (integer)
* The maximum number of drops rolled.

`NoDrop_DiceValue` (integer)
* The Dice Value weight of receiving no drop at all. This is like an extra drop on your table and is weighted against the other drops as though it was a real drop, but the player receives nothing.

`Drops` (list of SL_ItemDropChance)
* The random drops on your table. Their Dice Value weights (along with the NoDrop) will determine their relative drop chance.

#### Drop-rate examples
The drop rates are determined through an imaginary "dice roll", where each drop has an associated unique range of values from 0 to the maximum dice value. Each drop may occupy 1 or more values, but no two ranges will overlap - they will be laid out consecutively after each other.

For one example, if your `NoDrop_DiceValue` was set to 5 and you have <b>one</b> `SL_ItemDropChance` of Iron Sword with a `DiceValue` of 10, this would mean you have:
* `0 to 4` = no drop (~33.3%, range of 5)
* `5 to 14` = Iron Sword (~66.6%, range of 10)
* Total dice range of 15 (values 0 to 14).

For another example, let's say you have a NoDrop of 10 and <b>two</b> drops with dice values of 5 for Iron Sword and 15 for Salt respectively, you will now have:
* `0 to 9` = no drop (~33.3%, range of 10)
* `10 to 14` = Iron Sword (~16.6%, range of 5)
* `15 to 29` = Salt (50%, range of 15)
* Total dice range of 30 (values 0 to 29).

#### ** C# only **

From C#, just define your SL_DropTable like normal, then all you have to do is call `Prepare()` like so:

```csharp
// In your Awake() method:
var table = new SL_DropTable()
{
	/* ... */
};
table.Prepare();
```

<!-- tabs:end -->