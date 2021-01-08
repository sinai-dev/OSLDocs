# SL_Damage

SL_Damage is a wrapper used for Damages, used by many classes.

## SL_Damage Fields

`Damage` (float)
* The actual damage value

`Type` ([DamageType.Types](API/Enums/DamageTypes.md))
* The type of the damage.
* Must be one of: `Physical`, `Ethereal`, `Decay`, `Electric`, `Frost`, `Fire` or `Raw`. There is also `DarkOLD` and `LightOLD` which are unused.