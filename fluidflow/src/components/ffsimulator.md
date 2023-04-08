# FFSimulator

The `FFSimulator` provides a basic texture-based flow simulation, for a referenced `TextureChannel`.

**Note** that the `TextureChannelFormat` of the targeted `TextureChannel` should have 4 channels (RGBA) and support values >1 (optimally floating point/HDR).

The `FFSimulator` is initialized automatically as soon as the targeted `FFGravityMap` is fully initialized.

## Flow Speed

Each simulation step, fluid flow into/out of the current texel to the direct neighbors.
Thus, the flow speed is directly dependent on the update frequency.

As texel-density on the mesh can vary, the fluid can appear to flow faster/slower on parts of the mesh.

## Performance

A timeout can be used to halt simulation after a specified amount of time after the last edit of the targeted `TextureChannel` in order to save computation cost.

In general, the performance impact of the simulation is directly proportional to the `TextureChannel` resolution, and simulation update frequency.

### Fields

Check the tooltip hints for additional information of individual fields.