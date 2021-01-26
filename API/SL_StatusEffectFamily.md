# SL_StatusEffectFamily

The `SL_StatusEffectFamily` class is used to define a custom Status Effect family. These are used by certain [SL_StatusEffects](API/SL_StatusEffect.md), either as a unique individual family or as part of a multi-effect family like Poisons or Bleeding, etc.

Use the SL Menu/XML to create a template, or define it from C# directly.

?> Note: Currently this is only really intended for creating new <b>Reference</b> families, I have not experimented with editing existing ones.

?> Note 2: This is not for Bind families. A Bind family cannot be referenced as a Status Family elsewhere (ie, you can't use a Bind Family for a HasStatusEffectEffectCondition). Bind families should be defined directly on the StatusEffect itself.

## Fields

`UID` (string)
* Unique Identifier for this family.
* Existing families use a generated UID for this
* You can use anything, eg `me.mymod.mystatusfamily`.

`Name` (string)
* Name of the family. Not sure if even used.

`StackBehaviour` (enum)
* The stacking behaviour, must set from one of:
* `Ignored` - new status always ignored
* `Override` - new status always overrides existing
* `Highest` - picks highest StatusEffect.Priority
* `StackAll` - all effects are combined
* `IndependantUnique` - limit of one unique status
* `Independant` - limit of one status

`MaxStackCount` (int)
* The maximum amount of statuses stacked at once
* `-1` for no limit

`LengthType` (enum)
* The length type, must be one of:
* `Short` - lasts in seconds based on Lifespan, lost on sleep/death
* `Long` - no lifespan, used by Diseases and permanent effects.

## Examples

For XML, if you are defining an SL_StatusEffect and using the Bind family mode, the family should be defined directly in the SL_StatusEffect template.

If using a Reference family, you can define a new one from the `StatusFamilies` folder of your SLPack, or via C#.

<!-- tabs:start -->

#### ** XML **

```xml
<?xml version="1.0" encoding="utf-8"?>
<SL_StatusEffectFamily xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <UID>com.me.mystatusfamily</UID>
    <Name>MyFamilyName</Name>
    <StackBehaviour>Highest</StackBehaviour>
    <MaxStackCount>1</MaxStackCount>
    <LengthType>Short</LengthType>
</SL_StatusEffectFamily>
```

#### ** C# **

```csharp
// in BeforePacksLoaded or Awake():
var family = new SL_StatusEffectFamily 
{
    UID = "me.mymod.myfamily",
    Name = "MyStatusFamily",
    StackBehaviour = StatusEffectFamily.StackBehaviors.Highest,
    MaxStackCount = 1,
    LengthType = StatusEffectFamily.LengthTypes.Short
};

family.Apply();
```

<!-- tabs:end -->