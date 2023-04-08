# FFBrush

A `FFBrush` can currently only be used for drawing a solid color (triplanar texture mapping might be added in the future).
By setting the brush type to FLUID, the fluid amount added can be specified.

The drawing of a `FFBrush` is internally implemented as a distance function from an arbitrary 3D shape.
When drawing a brush, a maximum distance is specified, and all pixels within this distance from the shape are colored.
The fade value multiplied by this maximum distance defines the distance from the edge of the shape, where the intensity of the brush starts decreasing.
And the intensity of the brush reaches zero, when the distance from the shape is equal to the maximum distance.
The interpolation function is currently fixed to a smooth-step function, which has zero for its 1st- and 2nd-order derivatives at `x = 0` and `x = 1`.

## Custom Brush Shape

It is possible to add custom brush shapes, by creating a new method in the `FluidFlow/Scripts/Draw/BrushExtensions.cs`.
This requires setting the required variables as global shader values, and writing a custom shader with the distance function of the shape.

The `FluidFlow/Resources/InternalShader/Draw/CustomBrushTemplate.shader` provides a template for such a custom brush shader, which only requires you to fill in the distance function.
To call this custom brush shader from C#, you can add a new `MaterialCache` to the other brush shaders in the `FluidFlow/Scripts/Draw/BrushExtensions.cs`.