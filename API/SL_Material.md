# SL_Material

SideLoader allows you to adjust the Material and Shader properties through xml files. This is done with a file called <b>properties.xml</b>, which must be placed in the Material sub-folder for the textures.

When you generate a template with the SL Menu, it will include these xml files in the correct structure for you.

?> <b>Tip:</b> you can copy the properties.xml (or sections of it) from one material to another.

See also: [Custom Item Visuals](Guides/ItemVisuals.md).

`ShaderName` (string)
* Sets the <b>Shader</b> that will be used on the Material.
* Unless you know how to find the Shader names, just copy this from other materials if you need to.
* You can also set this to Unity's default shaders, such as PBR.

`Keywords` (list of string)
* A list of `<string>` values which determines the <b>enabled keywords</b> on the Shader.
* Keywords are used to enable certain special features of the Shader.

`Properties` (list of ShaderProperty)
* A list of `ShaderProperty` objects, used to set properties on the shader.
* See the ShaderProperty section below.

`TextureConfigs` (list of TextureConfig)
* A list of `TextureConfig` objects, used to set some config values for the Textures.
* See the TextureConfig section below.

#### ShaderProperty

A `ShaderProperty` only has one value, called `Value`. However, the type of this value will vary depending on the type of ShaderProperty.

* A <b>FloatProperty</b> will expect a <b>float</b> value (number).
* A <b>ColorProperty</b> will expect a <b>Color</b> value (`r`, `g`, `b`, `a`)
* A <b>VectorProperty</b> will expect a <b>Vector4</b> value (`x`, `y`, `z`, `w`)

#### TextureConfig

A `TextureConfig` has a few simple values which determine how SideLoader loads the associated Texture.

`TextureName` (string)
* Must match the name of one of the textures being set on this material.

`UseMipMap` (true/false)
* Default `true`
* If true, will enable <b>MipMap</b> for this texture (adjusts resolution dynamically per distance)

`MipMapBias` (integer)
* If `UseMipMap = true`, this will set the MipMap Bias level. Only set if you know what you're doing.