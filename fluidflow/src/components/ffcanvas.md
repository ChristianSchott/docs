# FFCanvas

The `FFCanvas` is the main component of FluidFlow.
It manages the RenderTextures used for drawing, and sets up the renderers' materials for displaying the textures.

In order for the drawing to work properly each mesh, that is being drawn on, requires a bijective UV map, meaning that there is a 1-to-1 mapping between the object's surface and its texture coordinates.
Consequently, the UV islands must not overlap, and there should be at least a 2 pixel-wide gap between UV islands for the fluid simulation to work properly.

If a models primary UV map does not fulfill these requirements, the secondary/lightmap UVs can be used instead.
Unity can automatically generate such a secondary lightmap UVs, by enabling the `Generate Lightmap UVs` option, in the models import settings.
However, it has to be noted that the 'quality' of automatically generated lightmap UVs is lower than a set of handcrafted UVs, as UV seams might be in very prominent places of the model.
FluidFlow can deal with UV seams (see `FFEffects`/`FFSeamFixer`), but it is impossible to completely hide all artifacts from UV seams.
Especially for fluid simulations, UV seams can be noticeable, as fluid has to be teleported between UV islands, and texel density will most likely change at the seam.

### Fields

- **Auto Initialize**: initialize the `FFCanvas` automatically `OnStart()`.
- **Initialize Async**: don't block the main thread if pre-processing of the meshes is requied. Check out [FFModelCache](../scriptable_object/ffmodelcache.md) for faster initialization. 

## Surfaces

The internal textures of a `FFCanvas` can be shared by multiple renderers and submeshes of these renderers.
For each renderer in the `Surfaces` list, you can define which UV-set should be used for drawing.
Additionally, you can define which subset of submeshes of the renderer is included in the `FFCanvas`, and define the section of the texture atlas the surface should occupy using the `UV Scale` and `UV Offset` properties.
Optionally, you can assign different section for different sets of submeshes.
In general, no UV-island should overlap any other UV-island, even of other submeshes/renderers.

Click the `Open In Editor` button, for a visual editor for simpler layouting of the uv maps.

## Texture Channels

Each **Texture Channel** is internally represented by a [RenderTexure](https://docs.unity3d.com/ScriptReference/RenderTexture.html), and is directly (reference) or indirectly (name) identified using a [`TextureChannel`](../scriptable_objects/texture_channel.md) object.
The `TextureChannel` also defines to which texture property of the **Surfaces'** materials the `RenderTexture` is applied to.

The format of the internal `RenderTexture` is determined using a [`TextureChannelFormat`](../scriptable_objects/texture_channel_format.md) object.

It is either initialized with a solid color, or by copying the contents of the current textures assigned to the renderers' materials.

When the texture channels are initialized, they are assigned corresponding texture properties of the renderers using [`MaterialPropertyBlocks`](https://docs.unity3d.com/ScriptReference/MaterialPropertyBlock.html).
So do not be confused, if the textures do not show up in the material's inspector fields, as they are directly assigned to the renderers.

## Material Property Overrides

The **Material Property Overrides** define to which material properties the `UV Scale`, `UV Offset`, and selected UV set information is passed.

- **Atlas Transform Propety**: refers to a `float4` property encoded as (`UV_Scale`.xy, `UV_Offset`.xy). This is the same format as used for Unity's default texture tiling and offset property, accessed via the `_ST` postfix in the shader (e.g. `_MainTex_ST`).

- **Secondary UV Name**: is either set as a keyword, if the secondary UV set is used, or a `float` property with the given name is set to `0`/`1`, depending on the UV set used.

