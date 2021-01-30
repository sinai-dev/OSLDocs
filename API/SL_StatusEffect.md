# SL_StatusEffect

See also: [Custom Status Effects](Guides/StatusEffects.md).

<!-- tabs:start -->
#### ** Universal **

`TargetStatusIdentifier` (string) <b>[REQUIRED]</b>
* The Identifier Name of the Status Effect you want to clone from or modify

`StatusIdentifier` (string)
* If you want to create a new Status, set a new Identifier for it here.

`NewStatusID` (integer)
* Sets the Preset ID of the Status Effect. This isn't really used by many things, but you should still set it if you want to be safe.
* If you set to 0 or below, SideLoader will ignore this value completely.

`Name` (string)
* The name of your Status Effect. Unity rich text supported.

`Description` (string)
* Your status description.
* Status Effects can make use of special formatting to display the values of the effects.
* For example, typing `[E1V1]` in your effect description will be substituted to the "first value" from the "first effect" on the status. This may not always work depending on the actual effects you use.

`Lifespan` (float)
* The length of the Status Effect in seconds.

`Purgeable` (bool)
* If false, the status cannot be purged in any way other than the natural lifespan.

`ActionOnHit` (enum)
* The behaviour when the character with this status gets hit by an attack
* `None` - do nothing
* `ReduceStack` - reduce a level of the status (for LevelStatusEffect)
* `RemoveStatus` - removes the status entirely 

`Priority` (int)
* The priority of this effect if it is part of a stacking family.
* Higher number = higher stacking priority.
* The `FamilyMode` must be set to `Reference`.

`RefreshRate` (float)
* How frequently the status effects are applied, in seconds.
* Depending on the effect, this may not matter. Most important for regeneration and DoT effects.

`BuildupRecoverySpeed` (float)
* How quickly the status buildup falls down to 0.
* Value is how much buildup falls per second.

`AmplifiedStatusIdentifier` (string)
* Determines the Status Effect which will be received if the player has Shamanic Resonance
* Set this to the Identifier Name of the Status Effect you want to set.

`IgnoreBuildupIfApplied` (boolean)
* If false, buildup cannot be added when already 100 or if status is active.

`IgnoreBarrier` (bool)
* Whether or not this status ignores the Barrier protection stat

`DelayedDestroyTime` (int)
* Delayed time in seconds when the status stops before it is actually destroyed (for on-stop special FX).

`DisplayedInHUD` (boolean)
* Is the status visible in HUD?

`IsHidden` (boolean)
* Is the status completely hidden?

`IsMalusEffect` (boolean)
* Determines the category this effect is shown under in the HUD.
* `true` will show this effect in the Negative category, `false` will show it in the Positive category.

`ComplicationStatusIdentifier` (string)
* The Status Identifier for the "Complication Status" (I think it becomes a more severe status if untreated or ignored?) Or maybe used for Ambraine Withdrawal.

`RequiredStatusIdentifier` (string)
* The Status identifier for the Required Status. For example, Plague, Blaze and Holy Blaze, these statuses require Poisoned or Burning on the target to work.

`RemoveRequiredStatus` (bool)
* Relating to the previous value, should the required status be removed when this is applied?

`NormalizeDamageDisplay` (bool)
* Should the damage value (if automatically parsed) be normalized (rounded) for the display?

`Tags` (list of string)
* The `Tags` is a list of string (text) values. For statuses, these are not used that much.
* See [this Google Sheet](https://docs.google.com/spreadsheets/d/1btxPTmgeRqjhqC5dwpPXWd49-_tX_OVLN1Uvwv525K4/edit#gid=1840819680) for a list of tags.
* Do not include the tag number.

`FamilyMode` (enum)
* The status family mode
* Must be one of: `Bind` (unique individual family) or `Reference` (part of a joined family)
* If you use `Bind`, you must define the `BindFamily`.
* If you use `Reference`, you must set the `ReferenceFamilyUID`, and also define an `SL_StatusEffectFamily` for this UID if it is a new one.
* See also: [SL_StatusEffectFamily](API/SL_StatusEffectFamily)

`BindFamily` (SL_StatusEffectFamily)
* If using `Bind` FamilyMode, define the family here.
* <b>NOTE:</b> A bind family cannot be referenced as a Status Family elsewhere (ie, you can't use a Bind Family for a HasStatusEffectEffectCondition). If you want this family to be "public", you need to use a Reference family.
* See [SL_StatusEffectFamily](API/SL_StatusEffectFamily) for details on these fields

`ReferenceFamilyUID` (string)
* If using `Reference` FamilyMode, set this to the reference family's UID.
* A family with this UID must exist, see [SL_StatusEffectFamily](API/SL_StatusEffectFamily) for details on how to create a new family

`PlayFXOnActivation` (boolean)
* Play the FX effects on activation?

`VFXInstantiationType` (enum)
* The type of VFX behaviour to use.
* If setting VFXPrefab, this should be set to `VFX`.
* Otherwise, must be one of: `None`, `Normal`, `ExternalOrLoad`

`VFXPrefab` (enum)
* If VFXInstantionType set to VFX, this is the VFX Prefab that will be used.
* Must be one of [these values](API/Enums/VFXPrefabs).

`PlaySpecialFXOnStop` (bool)
* Should Special FX be played on stop?

`SpecialSFX` (enum)
* The sound FX to use on activation
* Must be one of [these values](API/Enums/Sounds)

`FXOffset` (Vector3)
* The offset position for the VFX

`EffectBehaviour` (enum)
* Relates to the <b>Effects</b> on your status.
* Explained on the Effects page.

`Effects` (list of `SL_EffectTransform`)
* See the [Effects](API/SL_EffectTransform) page for documentation on all SL_Effect classes, and more details on how to set up EffectTransforms.

### SL_LevelStatusEffect : SL_StatusEffect

The class SL_LevelStatusEffect inherits from SL_Effect, and is used to increase the effects of the status by levels. Each level of the status will add on the total effects on top, so the potency of all effects is multiplied by the level directly.

`MaxLevel` (int)
* The maximum level of the effect, indexed from 1.
* For example, Alert has 4 levels, so the MaxLevel is 4.

For the additional level icons of the LevelStatusEffect, they must be placed alongside your `icon.png` in your status's folder in your SLPack, and must be named `icon2.png`, `icon3.png`, etc for each additional level of the status.

### SL_LeechStatus : SL_StatusEffect

The class SL_LeechStatus inherits from SL_StatusEffect, and is used to heal the person who applied this effect.

`LeechRatio` (float)
* The ratio of the damage dealt healed to the person who applied the effect.
* 1.0 is 100%, etc

`LeechFXRatio` (float)
* The ratio of damage dealt used to amplify the visual FX of the status
* 1.0 is 100%, etc

### SL_Disease : SL_StatusEffect

The class SL_Disease inherits from SL_StatusEffect, and contains a few extra fields. This is used for Diseases such as Cold, Indigestion, etc.

I haven't tested this class much so let me know if it's buggy.

`AutoHealTime` (int)
* If greater than 0, the amount of time until automatically healed, in game hours

`CanDegenerate` (boolean)
* If true, this status can degenerate into a worse form. Probably set through effect family, not sure.

`DegenerateTime` (float)
* If can degenerate, the time in game hours after it will degenerate to a worse status

`DiseaseType` (enum)
* Sets the disease type for this effect
* Must be one of: `Cold`, `Taenia`, `Malnutrition`, `Indigestion`, `Malaria`, `Rheumatism`, `Paranoia`, `Otitis`, `Flu`, `Glaucoma`, `Infection`, `Exalted_LifeDrain`, `AmbraineWithdrawal`

`SleepHealTime` (int)
* If greater than 0, the amount of time of straight sleeping which heals this status, in game hours

#### ** C# Only **

For C#, just call `ApplyTemplate()` to apply your template, at Awake or BeforePacksLoaded.

### Events

`Action<StatusEffect> OnTemplateApplied`
* Called when this template is applied during startup or hot reload.
* The `StatusEffect` argument passed to your method is the StatusEffect prefab that was just created.

## CustomStatusEffects

The `CustomStatusEffects` class is similar to `CustomItems`, it contains the API used by SideLoader to set up custom status effects and imbues, you may find it useful.

<!-- tabs:end -->

