# Custom Skills

The `SL_Skill` template inherits from [SL_Item](API/SL_Item.md), and includes a few extra fields for controlling skills.

For custom skills, most of the skill is actually defined from the <b>EffectTransforms</b> field. See the [Effects](API/SL_EffectTransform) articles for more details on this.

For Passives, none of these fields really do anything. You'll only need to worry about the EffectTransforms.

## SL_Skill : SL_Item
All SL_Skills inherit from this class. If you use a generated SL_Item template, these values will be at the bottom.

`Cooldown` (float)
* Cooldown of the skill.

`StaminaCost` (float)
* Stamina cost of the skill

`ManaCost` (float)
* Mana cost of the skill

`HealthCost` (float)
* Health cost of using the skill

`DurabilityCost` (float)
* Durability cost of activation for current weapon (flat amount)

`DurabilityCostPercent` (float)
* Same as durability cost, but as a % of total weapon durability value

`VFXOnStart` (bool)
* Whether the `StartVFX` will play immediately when this skill is activated, or after a short delay with the rest of the effects

`StopStartVFXOnEnd` (bool)
* Whether the `StartVFX` will end if the skill ends and the VFX is still playing

`StartVFX` (enum)
* Determines the VFX prefab that will play on start.
* Can pick any of [these values](API/Enums/VFXPrefabs.md), or `NONE` to set no VFX.

### Required Items

The `RequiredItems` value is a list of SkillItemReq objects.

The XML should look like this:
```xml
<RequiredItems>
  <SkillItemReq>
    <ItemID>6500010</ItemID> <!-- Fire Stone (6500010) -->
    <Quantity>1</Quantity> <!-- Requires 1 -->
    <Consume>true</Consume> <!-- Consumed: true -->
  </SkillItemReq>
  <!-- etc.. can add more -->
</RequiredItems>
```

The fields should be self-explanatory. Add as many SkillItemReq objects as you want.

## SL_AttackSkill : SL_Skill

If a skill is an "Attack Skill" you can use these fields. Attack Skills are any skills that do some kind of damage or counter. Currently the only active skills which aren't attack skills are placing Sigils, Reveal Soul, Mana Ward, Conjure, Flamethrower, and cosmetic skills.

`RequireImbue` (boolean)
* Used by skills that require any kind of imbue, eg. Gong Strike or Elemental Discharge.

`RequiredWeaponTags` (list of string)
* This works the same way as the `Tags` field on the SL_Item class. It's a list of <string> values.
* The game uses this for the Lexicon tag, which can be fulfilled by either off-hand or main-hand weapons.

`AmmunitionAmount` (int)
* The ammunition amount required and used by the skill, for Bow skills usually

```xml
<RequiredWeaponTags>
  <string>Lexicon</string>
</RequiredWeaponTags>
```

`RequiredWeaponTypes` (list of WeaponType)
* A list of required main-hand weapon types, used for skills that require a certain type of weapon. 
* Characters can have ANY entry on the list to use the skill.
* Values must be exactly one of [these values](API/Enums/WeaponType.md) (Weapon Type enum)

It should look like this in XML:
```xml
<RequiredWeaponTypes> <!-- either 1H Sword or 1H Axe for this skill. -->
  <WeaponType>Sword_1H</WeaponType> 
  <WeaponType>Axe_1H</WeaponType>
</RequiredWeaponTypes>
```

`RequiredOffHandTypes` (list of WeaponType)
* Same as RequiredWeaponTypes, but for off-hand.
* If you set both RequiredWeaponTypes <b>and</b> RequiredOffHandTypes, the character needs one from BOTH lists.

`AmmunitionTypes` (list of WeaponType)
* Same as previous two values, but for required Ammunition.

## SL_RangeAttackSkill : SL_AttackSkill
Inherits from SL_AttackSkill, used for bow skills.

`AutoLoad` (boolean)
* Should this load the bow for you?

`FakeShoot` (boolean)
* Not sure what this does, sorry.

`OverrideAimOffset` (boolean)
* Override the aim offset of the caster with the AimOffset value?

`AimOffset` (Vector2)
* An optional `x` and `y` offset for aiming.

## SL_MeleeSkill : SL_AttackSkill
A SL_MeleeSkill inherits from SL_AttackSkill, and contains a few extra fields.

`Blockable` (boolean)
* Can the attack be blocked?

`Damage` (float)
* The physical damage of the melee hit.

`Impact` (float)
* The impact damage of the melee hit.

`LinecastCount` (integer)
* The amount of Linecasts to scan (like a Raycast, but for a physical line-object).

`Radius` (float)
* The radius of the collision

`Unblockable` (bool)
* Is the attack <b>unblockable</b>? Overrides `Blockable`.

`NoWeaponAtkTag` (bool)
* Overrides this weapon skill being identified as one by the `WeaponSkillAttack` tag (Damascene/Masterpiece weapons)
* True to not be identified as one

## SL_PistolSkill : SL_AttackSkill
Inherits from SL_AttackSkill, contains two extra fields for Pistols.

This is currently only set up to help modify the Fire & Reload skill, there is no API for creating new pistol-based skills from this yet.

`PrimaryActivationRequirement` (enum)
* Determines the requirement for the normal activation (ie. reloading)
* Must be one of: `Ignore`, `Empty`, `OwnsAmmunition`, `Loaded`, `OwnsAmmunitionOrLoaded`

`AlternativeActivationRequirement` (enum)
* Determines the requirement for the alternate activations (ie. shooting), determined by the EffectConditions for the various shots
* Must be one of: `Ignore`, `Empty`, `OwnsAmmunition`, `Loaded`, `OwnsAmmunitionOrLoaded`

## SL_ThrowSkill : SL_AttackSkill
Inherits from SL_AttackSkill, and contains a few extra fields. This is used with a `SL_ThrowItem` effect.

`ThrowableItemIDs` (list of integer)
* A list of integers (`<int>`) which represent Item IDs you can throw with this skill.
* Eg, `<ThrowableItemIDs><int>2000010</int></ThrowableItemIDs>` would allow you to throw an Iron Sword.

## SL_TrinketSkill : SL_Skill
Inherits from SL_Skill, used for simple trinket skills like Dagger Slash.

`CompatibleItemIDs` (list of integer)
* A list of `<int>` Item IDs which work with this skill.

## SL_CounterSkill : SL_MeleeSkill
Inherits from SL_MeleeSkill, and contains some extra fields.

`BlockDamageTypes` (list of DamageType)
* A list of DamageType.Types objects which this skill can <b>block</b>.
* See [this page](API/Enums/DamageTypes.md) for a list of Damage Types.

`CounterDamageTypes` (list of DamageType)
* A list of DamageType.Types objects which this skill can <b>counter</b>.
* See [this page](API/Enums/DamageTypes.md) for a list of Damage Types.

`MaxRange` (float)
* Maximum range of the counter/block.

`TurnTowardsDealer` (boolean)
* On counter, will turn caster towards the attacking character.

`KnockbackMult` (float)
* Multiplier for impact, not sure exactly what for.

`BlockMult` (float)
* Multiplier for blocking, not sure exactly what for.

`DamageMult` (float)
* Multiplier for damage, not sure exactly what for.

## SL_CounterSelfSkill : SL_CounterSkill
Inherits from SL_CounterSkill. Contains no extra fields, is used by Brace. CounterSelf fires the HitEffects on the owner character on a successful counter.

## SL_CounterAbsorbSkill : SL_CounterSkill
Inherits from SL_CounterSkill. Contains one extra field.

`Absorbs` (list of AbsorbType)
* This is a list of the Absorb Types this counter can absorb.

### AbsorbType
An AbsorbType has the following fields:

`Condition` (SL_BooleanCondition)
* An SL_BooleanCondition to check.

`DamageTypes` (list of DamageType.Types)
* Like the list of damage types in CounterSkill.

