# FFGravityMap

The `FFGravityMap` is the base for the texture-/particle-based fluid simulation.

It transform world-space gravity to UV-space gravity and saves it in an internal `RenderTexture`, which can be accessed via the `FlowTexture` property.

The `FFGravityMap` is initialized automatically as soon as the targeted `FFCanvas` is fully initialized.

## Flow Texture Generation

The flow texture is updated lazily when the `FlowTexture` property is accessed, and an update is required.
In the `CONTINUOUS` update mode, it is updated once per frame (upon request), and in `FIXED` mode after the set time in seconds.
In the `CUSTOM` mode, you have to trigger updates manually using the `UpdateGravity()` method.

Optionally, the gravity map generation can be influenced by a normal map (identified by a `Texture Channel`), and by random noise, which can be set to change over time, for less uniform flow.

### Fields

Check the tooltip hints for additional information of individual fields.