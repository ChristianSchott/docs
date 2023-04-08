# FFModelCache

- `Assets/Create/Fluid Flow/Model Cache`

A `FFModelCache` allows to pregenerate and cache secondary UV transformation and stitching data, required for the gravity map generation and fluid simulation.
This can reduce performance overhead and time needed for initializing a `FFCanvas`/`FFGravityMap`. 

After selecting a model asset in the `Project` window, navigate to `Assets/Create/Fluid Flow/Model Cache` to generate a cache asset for the selected model.
When the `FFModelCache` is set up, hit the `Apply` button, to start generation and save the settings.

## Secondary UV Caches

When using secondary UV for fluid simulation, a UV1 to UV0 tangent-space transformation matrix has to be calculated for each vertex, so the gravity is projected to secondary-UV-space properly.
Additionally, when approximating the fluid normal, this transformation is needed to transform the fluid normal back to UV0 tangent-space.

Simply add a mesh to the `Secondary UV Caches` list, by clicking the `+` button.
If the selected mesh does not have a secondary UV set, Unity's internal [Unwrapping Utility](https://docs.unity3d.com/ScriptReference/Unwrapping.html) is used for automatically generating lightmap UVs.

## Stitch Caches

As fluid is simulated in UV space, seams in the UV unwrap would stop the flow.
Therefore FluidFlow analyzes the meshes in a `FFCanvas` to find seams in the UV map, and generates 'stitches' for stitching UV islands together.

Add a new entry, and set the desired mesh/UV-set, for which stitches should be pre-generated for.
When `UV1` is selected, but the mesh does not have a secondary UV set, the mesh is automatically added to the **Secondary UV Caches**.

## Global Cache

The cached data from all loaded `FFModelCache` scriptable objects is loaded during the start of the game to a global cache manager internally.
To ensure a `FFModelCache` is included in the build, it either has to be put in a 'Resources' folder in the project, or be referenced by an object in the scene.
So in theory, a `FFModelCache` only has to be referenced once by a `FFCanvas`, and the cached data can still be accessed by all other `FFCanvas` components in the game.
However, as a best practice, the `FFModelCache` should be referenced by all `FFCanvas` components which need to access the data, to make sure the `FFModelCache` is included in the final build.

All mesh/stitch data generated at runtime is automatically also added to this global cache, so the same data is not compute multiple times.

In the 'Global Settings' section, an update of all `FFModelCache` objects in the project can be triggered.
This checks for each `FFModelCache` if the model it targets has been modified, and if so recalculates the cache.

This updating of the caches can also be scheduled each time before entering the play mode, by enabling the 'Auto Update Before Play' toggle.
However, please note that this adds an overhead to entering the play mode.
