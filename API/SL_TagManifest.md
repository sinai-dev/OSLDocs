# SL_TagManifest

`SL_TagManifest` is used to define a list of custom Tags, which can be used by yourself or others for many different things.

A `Tag` by itself does nothing, but it can be used by various game systems such as crafting, skills, etc.

<!-- tabs:start -->

#### ** Universal **

There is only one field on an SL_TagManifest, which is a list of the Tags you want to define.

`Tags` (list of SL_TagDefinition)
* A list of `SL_TagDefinition`, which are the tags you want to define.

### SL_TagDefinition

The `SL_TagDefinition` is a simple wrapper for a custom tag.

`TagName` (string)
* Your unique Tag Name to define.

`OptionalTagUID` (string)
* Optional, a UID such as `myname.tags.mytagname`, this is not used for anything important. Leave blank or null for auto-generate.

#### ** C# Only **

For C#, you can also just use the `CustomTags` class to easily define a custom tag.

```csharp
var myTag = CustomTags.CreateTag("MyUniqueTag");
```

<!-- tabs:end -->