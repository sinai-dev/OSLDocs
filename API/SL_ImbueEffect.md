## SL_ImbueEffect

Imbues are similar to [Status Effects](API/SL_StatusEffect.md), but much simpler overall. See also: [Custom Status Effects](Guides/StatusEffects.md).

<!-- tabs:start -->
#### ** Universal **

`TargetStatusID` (integer)
* The ID of the Imbue you want to clone from or modify

`NewStatusID` (integer)
* If you want to create a new Imbue, set this to your desired ID here.

`Name` (string)
* The name of your Imbue. Unity rich text supported.

`Description` (string)
* Your imbue description.

`EffectBehaviour` (enum)
* Relates to the <b>Effects</b> on your imbue.
* Explained on the Effects page.

`Effects` (list of `SL_EffectTransform`)
* See the [SL_EffectTransform](API/SL_EffectTransform.md) page for documentation on all SL_Effect classes, and more details on how to set up EffectTransforms.

#### ** C# Only **

For C#, just call `Apply()` to apply your template, at Awake or BeforePacksLoaded.

### Events

`Action<ImbueEffectPreset> OnTemplateApplied`
* Called when this template is applied during startup or hot reload.
* The `ImbueEffectPreset` argument passed to your method is the ImbueEffectPreset prefab that was just created.

## CustomStatusEffects

The `CustomStatusEffects` class is similar to `CustomItems`, it contains the API used by SideLoader to set up custom status effects and imbues, you may find it useful.

<!-- tabs:end -->