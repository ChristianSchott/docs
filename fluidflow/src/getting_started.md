# Getting Started

## Demo Scenes

Example scenes using `FluidFlow` can be found in `FluidFlow/Example/Scenes`.
Each example scene contains a `RP Material Switcher` object at the top of the `Hierarchy` which allows swapping the materials in the scene to match your current rendering pipeline.

## A minimal example for setting up a `FFCanvas`

- Add a `FFCanvas` component to an object in your scene.

- Assign the renderers of the objects you want to draw on, to the **Surfaces** of the `FFCanvas`, and set up the surface as described in [FFCanvas](./components/ffcanvas.md#RenderTargets).

- Add the [**Texture Channels**](./scriptable_objects/texture_channel.md) you want to draw on, and set their [**Texture Channel Format**](./scriptable_objects/texture_channel_format.md). 

- Assign a material to your **Surfaces'** renderers, that supports drawing to the selected **Texture Channels**.
Fluid Flow provides basic shaders (integrated RP, URP, HDRP) that support drawing to the `Color`, `Normal`, and `Fluid` texture channels.

- When the internal texture channels of a `FFCanvas` are shared by multiple **Surfaces** (`UV Scale != (1,1)`, `UV Offset != (0,0)`), the renderers' materials need to transform their UVs before sampling the texture set by the `FFCanvas`. 
Most shaders support this out-of-the-box via the `Tiling` and `Offset` of the texture property.
In the **Material Property Overrides** of the `FFCanvas` you can you can set the name of this property for a given shader (if no target shader is set, it applies to all shaders).
Unity's default naming scheme for accessing the `Tiling` and `Offset` of a given texture property, e.g. `_MainTex`, is adding `_ST` to the property name (`_MainTex_ST`).

- You should now be able to draw to the `FFCanvas` as described in [Drawing](./drawing.md). For testing you can simply add a `Simple Project Decal` component, and assign a camera and `FFCanvas`. This should allow you to project a decal onto the canvas by clicking on the objects in the `Game` window while playing.

## Texture-based fluid simulation

- Add a `Fluid` texture channel using the `FluidFormat` and `BLACK` initialization to your `FFCanvas`.

- Make sure, your renderers have a material assigned, which is able to overlay the `Fluid` texture channel. 
FluidFlow provides example shaders for all rendering pipelines.

- Add a `FFGravityMap` component and assign the targeted `FFCanvas`.

- Add a **`FFSimulator`** component, assign the `FFGravityMap` created before, and select `Fluid` as the targeted texture channel.

- When paining to the `FFCanvas'` `Fluid` texture channel (using the `FLUID` type instead of `COLOR` while painting), the painted fluid should flow according to gravity.


## Particle-based fluid simulation

- Add a `Fluid` texture channel using the `FluidFormat` and `BLACK` initialization to your `FFCanvas`.

- Make sure, your renderers have a material assigned, which is able to overlay the `Fluid` texture channel. 
FluidFlow provides example shaders for all rendering pipelines.

- Add a `FFGravityMap` component and assign the targeted `FFCanvas`.

- Add a **`FFSimulatorParticle`** component, assign the `FFGravityMap` created before, and select `Fluid` as the targeted texture channel.

- Instead of drawing to the `FFCanvas`, fluid-particles have to be added directly to the `FFSimulatorParticle` component, using `AddParticle` or `ProjectParticles`. Check out the `SimpleDrawParticle` component for a simple example.