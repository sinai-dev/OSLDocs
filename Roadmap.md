# Roadmap
This is a rough plan of the development of SideLoader. The order is not set in stone by any means.

<span style="color:red">*</span> = out of my control at the moment.

## v1
- [x] Texture Replacement
- [x] Audio Replacement
- [x] Custom Items
- [x] Item Visuals

## v2
- [x] Using BepInEx / Harmony
- [x] Re-write Custom Serializer
- [x] SL Template Generator
- [x] Item Extensions
- [x] Item Recipes
- [x] Skill Trees
- [x] Status Effects
- [x] Effect Serialization
  - [x] Effect Conditions
- [x] Characters
  - [ ] Savable Characters
  - [ ] SL_Summon Support
- [x] Custom Enchantments
- [x] Custom Tags (C#)
- [x] Hot Reloading
- [x] All supported SL_Item, SL_Effect and SL_EffectConditions

Maybe:
- [ ] Custom or Modified Blast/Projectile Visuals

## v3 Ideas
SL 3.0 is probably a long way off, but my rough idea is:
- [ ] Improved SL Menu
  - [ ] Browser for loaded SL Packs
  - [ ] Editing SL templates
- [ ] Non-Human Characters <span style="color:red">*</span>
- [ ] Custom Props
- [ ] Custom Scenes
- [ ] Generic `UnityEngine.Transform` Serialization (to replace EffectTransforms and expand it)
- [ ] More robust Serializer (allow for UnityEngine classes, etc)
- [ ] Custom Status Families

Maybe:
- [ ] SkinnedMeshRenderer support <span style="color:red">*</span>
- [ ] XML Skill Trees / Skill Trainer setup
- [ ] XML Custom Tags
- [ ] Item Sources (adding items to merchants, droptables, etc)
- [ ] Custom Quests, or at least SL_Quest (SL_Item)
- [ ] Custom Dialogue / NodeCanvas Support