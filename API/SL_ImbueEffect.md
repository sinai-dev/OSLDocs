## SL_ImbueEffect

Imbues are similar to [Status Effects](API/SL_StatusEffect.md), but much simpler overall. See also: [Custom Status Effects](Guides/StatusEffects.md).

`TargetStatusID` (integer)
* The ID of the Imbue you want to clone from

`NewStatusID` (integer)
* The ID of the Imbue you want to apply to. Can be a new or existing ID.

`Name` (string)
* The name of your Imbue. Unity rich text supported.

`Description` (string)
* Your imbue description.

`EffectBehaviour` (enum)
* Relates to the <b>Effects</b> on your imbue.
* Explained on the Effects page.

`Effects` (list of `SL_EffectTransform`)
* See the [SL_EffectTransform](API/SL_EffectTransform.md) page for documentation on all SL_Effect classes, and more details on how to set up EffectTransforms.