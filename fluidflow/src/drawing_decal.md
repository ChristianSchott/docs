# FFDecal

A `FFDecal` allows drawing on multiple texture channels of a `FFCanvas` at once.
Each decal channel is identified by the shader property name of the texture channel it affects.
Additionally, setting the decal channel type to `FLUID` allows setting the fluid amount.
Setting the type to `NORMAL` sets a flag internally, so normal maps are projected properly.

The `FFDecal` also has an optional mask field.
When the mask texture is left empty, no mask is applied.
If a mask texture is set, all decal channels are affected by it.
The R, G, B, and A buttons allow specifying which channels of the mask texture will be used for calculating the mask value.
The mask value for each pixel is calculated by adding the values from the selected color channels, and dividing by the count of selected channels.

For example, when selecting the green and alpha channel, the mask value `v = (Mask.g + Mask.a)/2`.