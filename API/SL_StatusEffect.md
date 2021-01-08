# SL_StatusEffect

See also: [Custom Status Effects](Guides/StatusEffects.md).

## Fields

`TargetStatusIdentifier` (string) <b>[REQUIRED]</b>
* The Identifier Name of the Status Effect you want to clone from

`StatusIdentifier` (string) <b>[REQUIRED]</b>
* Determines the Status Effect your template will be applied to, or the new Identifier Name of your custom status.
* Set to an existing Identifier to apply to an existing Status, or create a new one to create a new Status.

`NewStatusID` (integer) <b>[REQUIRED]</b>
* Sets the Preset ID of the Status Effect. This isn't really used by many things, but you should still set it.
* If you set to 0 or below, SideLoader will ignore this value completely.

`Name` (string)
* The name of your Status Effect. Unity rich text supported.

`Description` (string)
* Your status description.
* Status Effects can make use of special formatting to display the values of the effects.
* For example, typing `[E1V1]` in your effect description will be substituted to the "first value" from the "first effect" on the status. This may not always work depending on the actual effects you use.

`Lifespan` (float)
* The length of the Status Effect in seconds.

`LengthType` (enum)
* Must be one of: `Short` or `Long`
* Short effects count down from in-game seconds, and are lost on sleep or death
* Long effects are diseases or incurable effects

`RefreshRate` (float)
* How frequently the status effects are applied, in seconds.
* Depending on the effect, this may not matter. Most important for regeneration and DoT effects.

`BuildupRecoverySpeed` (float)
* How quickly the status buildup falls down to 0.
* Value is how much buildup falls per second.

`AmplifiedStatusIdentifier` (string)
* Determines the Status Effect which will be received if the player has Shamanic Resonance
* Set this to the Identifier Name of the Status Effect you want to set.
* Note: if you're setting this to a custom Status, you should set that one up first. SideLoader applies templates in alphabetical order (of the file name), so use that to apply the other one first.

`IgnoreBuildupIfApplied` (boolean)
* If false, buildup cannot be added when already 100 or if status is active.

`DisplayedInHUD` (boolean)
* Is the status visible in HUD?

`IsHidden` (boolean)
* Is the status completely hidden?

`IsMalusEffect` (boolean)
* Determines the category this effect is shown under in the HUD.
* `true` will show this effect in the Negative category, `false` will show it in the Positive category.

`PlaySpecialFXOnStop` (bool)
* Should Special FX be played on stop?

`ComplicationStatusIdentifier` (string)
* The Status Identifier for the "Complication Status" (I think it becomes a more severe status if untreated or ignored?) Or maybe used for Ambraine Withdrawal.

`RequiredStatusIdentifier` (string)
* The Status identifier for the Required Status. For example, Plague, Blaze and Holy Blaze, these statuses require Poisoned or Burning on the target to work.

`RemoveRequiredStatus` (bool)
* Relating to the previous value, should the required status be removed when this is applied?

`NormalizeDamageDisplay` (bool)
* Should the damage value (if automatically parsed) be normalized (rounded) for the display?

`IgnoreBarrier` (bool)
* Whether or not this status ignores the Barrier protection stat

`Tags` (list of string)
* The `Tags` is a list of string (text) values. For statuses, these are not used that much.
* See [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1840819680) for a list of tags.
* Do not include the tag number.

Your tags XML should look like this, for example:
```xml
<Tags>
  <string>Boon</string>
  <string>Status Type</string>
</Tags>
```

`EffectBehaviour` (enum)
* Relates to the <b>Effects</b> on your status.
* Explained on the Effects page.

`Effects` (list of `SL_EffectTransform`)
* See the [Effects](API/SL_EffectTransform) page for documentation on all SL_Effect classes, and more details on how to set up EffectTransforms.