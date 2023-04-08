# FFEffects

`FFEffects` provides some basic additional effects for the fluid simulation, to the targeted `TextureChannel` on the `FFCanvas` referenced by the `FFGravityMap`.

## Effects

- **Stitch Seams**: the `FFSimulatorParticle` does not automatically deal with UV seam artifacts like the `FFSimulator`. 
This option uses the `FFGravityMap's` flow texture for stitching the seams.
This should be used instead of the `FFSeamFixer`, when a `FFGravityMap` is available.

- **Evaporation**: reduce total fluid amount over time.
    - `LINEAR`: the same amount of fluid is removed each update
    - `EXPONENTIAL`: a fraction of the remaining fluid is removed each update

- **Blur**: when a texel contains a lot of fluid, some will spread to surrounding texels containing less fluid.
This can for example be useful for making fluid appear to soak into clothing.