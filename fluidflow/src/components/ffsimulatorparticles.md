# FFSimulatorParticle

The `FFSimulatorParticle` provides a basic particle-based flow simulation.
The Particle data is saved in two internal `RenderTextures` containing the particles' UV position, fluid amount, speed, and color.

The `FFSimulatorParticle` is initialized automatically as soon as the targeted `FFGravityMap` is fully initialized.

## Flow Simulation

Each simulation step, the particles sample the `FFGravityMap`'s flow texture at their position and move in their corresponding 'down' direction.
A small circle is then drawn for each active particle to the targeted `TextureChannel` of the `FFCanvas`.
Over multiple update steps, this results in a trail as the particle is moving down the surface.

## Adding Particles to the Simulation

Compared the the `FFSimulator` particles have to be added to the `FFSimulatorParticle` directly instead of to a `FFCanvas` texture channel.

The particle-data textures behave similar to a First-In-First-Out (FIFO) buffer, meaning that older active particles may be overwritten by newer ones, if the internal particle-data texture size is too small for the amount of particles you are adding.

The next best section, new particles are added to, is determined in the `FFParticleUtil.Data.Request(Vector2Int size)` method.

There are two approaches for introducing new particles to the simulation:
- `ProjectParticles()`: project particles onto the renderer using a `FFProjector`. This renders the surfaces to a small section of the particles data `RenderTextures` using the projector's view-projection matrix. The UV coordinates of the rendered fragments within the projector's frustum are used as the new particles' starting position.
- `AddParticles()`: define the position, fluid amount, speed, and color of the new particles, and upload it to the data-buffer texture. However, the particle's position is the UV position within the `FFCanvas` texture atlas, and thus non trivial to compute.
`FFCanvasExtensions` contains two helper functions for computing these texture atlas UV positions (`AtlasTransformUV()`, `TryGetCanvasUV()`).

Checkout `FluidFlow/Examples/Scripts/SimpleDrawParticle.cs` for basic examples of these two approaches.


## Flow Speed

Each simulation step, particles must only move one pixel, to ensure that they do not step into a different UV island.
Thus, the flow speed is directly dependent on the update frequency.

As texel-density on the mesh can vary, the particles can appear to move faster/slower on parts of the mesh.

## Performance

A timeout can be used to halt simulation after a specified amount of time after the last edit to the particles in order to save computation cost. You can reset the timeout manually using the `ResetTimeout()` method.

In general, the performance impact of the simulation is proportional to the size of the particle data textures, and simulation update frequency.

### Fields

Check the tooltip hints for additional information of individual fields.