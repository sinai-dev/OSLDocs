# SL_ItemVisual

<!-- tabs:start -->
#### ** Universal **

These are the fields you can set on an SL_ItemVisual class.

?> <b>Note:</b> the `Prefab_` fields are not required. They're only used if you are using your own custom Visual Prefabs.

See also: [Custom Item Visuals](Guides/ItemVisuals.md).

`ResourcesPrefabPath` (string)
* If the custom `Prefab_` Visuals are <b>not</b> set, you can use this to transmog the visual prefab to a different Item's visuals.
* You can copy this from a different item for easy "transmogs".

`Prefab_SLPack` (string)
* The name of the SLPack you are using for the visuals.
* This means you can use Asset Bundles found in other SLPacks, if you want to.

`Prefab_AssetBundle` (string)
* The <b>file name</b> of the asset bundle in your SL Pack that contains your item visual prefab
* For example, if the bundle is `MyPack/AssetBundles/mybundle`, then put `mybundle` as your setting.

`Prefab_Name` (string)
* Set this to the name of the <b>GameObject</b> in the AssetBundle for your visual prefab.
* Make sure you actually made a GameObject out of your 3D model and you're not trying to load a Mesh object directly.

#### Vector3 Settings {docsify-ignore}

The next 4 settings are Vector3 settings. In xml, a Vector3 looks like this:
```xml
<!-- Example is Position. 
 Same goes for Rotation, PositionOffset, and RotationOffset -->
<Position> 
  <x>0</x>
  <y>0</y>
  <z>0</z>
</Position>
```

`Position` (Vector3)
* Allows you to directly set the position. This will override any setting from the original item or from Unity Editor.
* If you don't set this (ie. delete or comment it from the template) then SideLoader will copy the values from the existing visuals.

`Rotation` (Vector3)
* Allows you to directly set the rotation. This will override any setting from the original item or from Unity Editor.
* If you don't set this (ie. delete or comment it from the template) then SideLoader will copy the values from the existing visuals.

`PositionOffset` (Vector3)
* An offset applied to the position. You can use this to copy the position of the original visuals, and only apply an offset from that.

`RotationOffset` (Vector3)
* An offset applied to the rotation. You can use this to copy the rotation of the original visuals, and only apply an offset from that.

### SL_ArmorVisuals
SL_ArmorVisuals inherits from SL_ItemVisual, and contains two extra fields.

`HideFace`
* For head or chest armor only. Does equipping these visuals hide the player's Face?

`HideHair`
* For head or chest armor only. Does equipping these visuals hide the player's hair?

#### ** XML Example **
Here is an Xml Example of an SL_ItemVisual object in XML.

```xml
<ItemVisuals>
    <Prefab_SLPack>TsarRevolver</Prefab_SLPack>
    <Prefab_AssetBundle>tsarrevolver_bundle</Prefab_AssetBundle>
    <Prefab_Name>TsarRevolver_prefab</Prefab_Name>
    <!-- 
    I don't define Position or Rotation, so they are copied from the original.
	I simply use the PositionOffset and RotationOffset to adjust my visuals slightly.
	This was figured out with some trial and error.
    -->
    <PositionOffset>
        <x>-0.02</x>
        <y>0.075</y>
        <z>0.08</z>
    </PositionOffset>
    <RotationOffset>
        <x>-18</x>
        <y>90</y>
        <z>6</z>
    </RotationOffset>
</ItemVisuals>

<SpecialItemVisuals>
<!-- etc... -->
```

#### ** C# Only **

The `CustomItemVisuals` class contains a few helpers you might want to use from C#.

`SetSpriteLink(Item item, Sprite sprite, bool skill = false)`
* Set the sprite override for an Item, for either its ItemIcon or SkillTreeIcon
* If skill is set to true, will set the SkillTreeIcon.

`GetOrigItemVisuals(Item item, VisualPrefabType type)`
* Get the original unmodified item visuals for an Item

<!-- tabs:end -->

## CustomItemVisuals

If you want to do anything custom with Item Visuals from C#, the `CustomItemVisuals` class may be of help to you, see the public methods in your IDE.