# SL_EffectTransforms

SideLoader has support for <b>Custom Effects</b> with [SL_Item](API/SL_Item), [SL_Skill](API/SL_Skill) and [SL_StatusEffect](API/SL_StatusEffect), and more.

An <b>SL_EffectTransform</b> is a container for these effects, as well as Effect Conditions. Each Transform represents a group of effects that are applied at the same time and with the same conditions.

In most cases, looking at how existing items and skills work will give you the best idea on how to use these correctly.

## Edit Behaviour
`EffectBehaviour` (an `EditBehaviour` enum) is a setting found on various SideLoader templates. It determines <b>how your template effects are applied</b>. This is referring to when SideLoader actually applies this template, not when the effects are applied in game.
* `NONE` will <b>leave the existing effects untouched</b>. Use NONE if you don't want to modify the existing effects at all, or you want to add on-top of them.
* `Destroy` will <b>wipe all the existing effects</b> before it applies yours. This is recommended in most cases if you are editing the effects.
* `Override` will <b>only destroy transforms if you have defined one with the same TransformName</b>. You can use this to have greater control and only replace specific transforms. In some cases, it is possible that there are duplicate-name children on one transform, and SideLoader has no way to solve this. You would need to use Destroy and replace everything in this case.

?> <b>Note:</b> Even if you don't define any SL_EffectTransforms, your EffectBehaviour is <b>still applied</b> to the base prefab. All SL classes have `Override` or `NONE` as the default value if not set.

## SL_EffectTransform
An `SL_EffectTransform` is simply a container for effects and their conditions. When defining your effects on items, skills or status effects, you are generally defining a list of SL_EffectTransforms.

The fields on an SL_EffectTransform are:

`TransformName` (string)
* <b>Important</b>, this is used by the game to determine the <b>effects category</b>.
* See [TransformName](#TransformName) below.

`Position` (Vector3)
* A vector3 has an `x`, `y` and `z` value.
* Sets the local position of the transform.
* By default, this is ignored. You can use this to set a non-zero position for the transform.
* Certain effects may make use of this (eg. Backstab), but most don't.

`Scale` (Vector3)
* This vector3 value sets the local scale (size) of the transform.
* By default this is (1.0, 1.0, 1.0).
* This would not have any effect in most cases, but for certain things it will affect the size of the effect.

`Rotation` (Vector3)
* Another vector3 value, sets the local rotation of the transform.
* This one is even more rarely used, if at all. I included it for completeness.

`Effects` (list of SL_Effect)
* The actual list of effects for this transform.
* See [SL_Effect](API/SL_Effect)

`EffectConditions` (list of SL_EffectCondition)
* An optional list of conditions, which will determine whether or not the effects are activated.
* See [SL_EffectCondition](API/SL_EffectCondition)

`ChildEffects` (list of SL_EffectTransform)
* Can contain child SL_EffectTransforms, allowing the same hierarchy structure as Unity Transforms.
* This is not really needed, but the devs sometimes use this to sort their effects more neatly.

### TransformName

The <b>TransformName</b> is very important, the game uses this to know when to apply the Effects on this transform. 

The game checks if your TransformName <b>contains</b> one of the following <b>keywords</b>, in this order:

`Normal`
* For standard activation effects, ie. using the item or activating the skill
* Unlike `Activation`, these effects happen during the animation at a certain point depending on the animation
* For status effects, these are applied on each refresh.
* The affected character is the person activating/creating this effect.

`Hit`
* Used for on-hit effects.
* The affected character is the person <b>hit by the hit-effects</b>.
* <b>Note:</b> your conditions will also be evaluated against this character, unless the class allows you to override the check to the owner character.

`Passive`
* For passive effects, these apply only once when the passive is learned or loaded from a save.
* The affected character is the person activating/creating this effect.

`Activation`
* Used immediately on activation, before the animation begins.
* The affected character is the person activating/creating this effect.

`Block`
* Used for skills that have on-block effects
* The affected character is the character who blocked the attack.

There is one unique exception: if the TransformName is <b>exactly</b> `Effects`, `Effect`, `ExtraEffects` or `HiddenEffects`, it works the same as the `Normal` keyword. This is likely just an internal thing for the devs before they made the keyword system.

You can also override the TransformName by setting the `OverrideCategory` of your [SL_Effect](API/SL_Effect).

See [SL_Effect](API/SL_Effect) and [SL_EffectCondition](API/SL_EffectCondition) for more details.

## Example

<!-- tabs:start -->

#### ** XML **

```xml
<SL_EffectTransform>
  <!-- The TransformName determines the category of effects. -->
  <!-- HitEffects is a common category, used by weapons, skills and projectiles -->
  <TransformName>HitEffects</TransformName> 
  <Effects> 
    <!-- Any number of SL_Effect objects can be defined here -->
    <SL_Effect xsi:type="Effect Type Here">
      <!-- Fields for this effect go here -->
    </SL_Effect>
  </Effects>
  <EffectConditions>
    <!-- SL_EffectCondition objects can be defined here -->
    <SL_EffectCondition xsi:type="Condition Type Here">
      <!-- ... -->
    </SL_EffectCondition>
  </EffectConditions>
  <ChildEffects>
    <!-- generally you don't need Child Effects for most things, but the game sometimes uses them -->
  </ChildEffects>
  <!-- You don't need to set Position, Rotation or Scale, but it is used in some cases. -->
  <!-- These values are all a Vector3. For example, position: -->
  <Position>
	<x>1.0</x>
	<y>2.0</y>
	<z>-5.0</z>
  </Position>
</SL_EffectTransform>
```

#### ** C# **

```csharp
var transform = new SL_EffectTransform 
{
  TransformName = "HitEffects",
  Effects = new SL_Effect[]
  {
    new SL_AddStatusEffect
    {
      // ...
    },
    // ...
  },
  EffectConditions = new SL_EffectCondition[]
  {
    new SL_HasStatusEffectEffectCondition
    {
      // ...
    },
    // ...
  },
  // ...
}
```

<!-- tabs:end -->