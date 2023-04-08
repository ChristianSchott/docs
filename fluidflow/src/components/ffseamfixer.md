# FFSeamFixer

During rasterization, only pixels are drawn, which center points lie withing a triangle.
This means, that pixels of the texture, which lie barely outside of an UV island, are not drawn during paining/decal projection, but can be visible during rendering of the model.
This will be visible as artifacts at UV seams of the mesh.

The `FFSeamFixer` solves this problem, by expanding the color at the border of the UV islands outwards a few pixels.

***Note!*** when using fluid simulation, these seam artifacts are fixed for the fluid `TextureChannel` by the `FFSimulator` automatically, or `FFEffects` when using a `FFParticleSimulator`

## Performance

The `FFSeamFixer` only fixes seams on targeted `Texture Channels` which have been marked as updated by the `FFCanvas`.
You can manually mark a `Texture Channel` as modified by calling the `MarkModified(textureChannel);` method.

When using the `CUSTOM` update mode, call the `FixModifiedChannels()` method, to trigger an update manually.

## Cache

For finding the UV islands' borders, the `Surfaces` have to be rendered to an internal `RenderTexture` first.
The `Use Cache` field allows to cache this internal texture for future use, instead of regenerating it each update.
This requires some memory on the GPU, but reduces drawcalls/overhead of fixing seam. 