# SL_Recipe

<b>Custom Recipes</b> can be defined from XML or C#. You can set recipes for any station type.

Recipes XMLs should be placed in the `Recipes\` sub-folder of your SL Pack.

?> <b>Note:</b> Custom recipes are applied after all Custom Items have been set up, so you can safely reference those here.

You can make a recipe scroll with a `SL_RecipeItem` template. See the [SL_Item](API/SL_Item.md) page for more details.

<!-- tabs:start -->
#### ** Universal **

`UID` (string)
* You can set anything, but best to use something like a `myname.myrecipe` format.
* Used by Recipe Scroll items. Those items require a UID to learn the appropriate recipe.

`StationType` (enum)
* Can be exactly one of: `Survival`, `Alchemy`, `Cooking`, or `Forge` (unused)

`Ingredients` (list of Ingredient)
* Add between 1 and 4 ingredients
* See SL_Ingredient below for more details.

`Results` (list of ItemQty)
* You can add any number of results that you want.
* Each must have an `ItemID`, and `Quantity` for the result.

### Ingredient
`Ingredient` is the class used to contain your ingredient info.

`Type` (enum)
* Can be either a `AddSpecificIngredient` for an Item ID, or `AddGenericIngredient` for a Tag.

`SelectorValue` (string)
* If setting to a specific item, use that item's Item ID.
* If setting to a generic tag, use the tag name. See [this google sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1840819680) for a list of tags.

### ItemQty
`ItemQty` is the class used to contain the result item(s) info.

`ItemID` (int)
* The result item's Item ID

`Quantity` (int)
* How many to receive, usually 1. Setting to 0 or below will do nothing.


#### ** XML **

```xml
<?xml version="1.0" encoding="utf-8"?>
<SL_Recipe xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <!-- Choose a unique UID for this recipe -->
  <UID>myname.myrecipe</UID>
  <!-- One of: "Survival", "Alchemy", "Cooking", "Forge" (unused) -->
  <StationType>Survival</StationType> 
  <Ingredients>

    <Ingredient>
      <!-- Type: "AddSpecificIngredient" for an Item ID, or "AddGenericIngredient" for a Tag Name -->
      <Type>AddSpecificIngredient</Type>
      <SelectorValue>2010070</SelectorValue>
    </Ingredient>

    <Ingredient>
      <Type>AddGenericIngredient</Type>
      <SelectorValue>Water</SelectorValue>
    </Ingredient>

    <Ingredient>
      <Type>AddSpecificIngredient</Type>
      <SelectorValue>2010070</SelectorValue>
    </Ingredient>

    <Ingredient>
      <Type>AddSpecificIngredient</Type>
      <SelectorValue>2010070</SelectorValue>
    </Ingredient>

  </Ingredients>

  <Results>
    <ItemQty>
      <!-- You can set any ID for the result item ID, including custom items. -->
      <ItemID>2010072</ItemID> 
      <Quantity>1</Quantity>
    </ItemQty>
    <!-- You can have multiple results
    <ItemQty>		
      <ItemID>2000010</ItemID> 
      <Quantity>1</Quantity>
    </ItemQty>
    -->
  </Results>
</SL_Recipe>
```

#### ** C# **

You should apply this recipe at Awake or BeforePacksLoaded.

```csharp
var myrecipe = new SL_Recipe() 
{
    // define fields here
};

myrecipe.ApplyRecipe();
```

<!-- tabs:end -->