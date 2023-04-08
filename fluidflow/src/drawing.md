# Drawing

In order to draw on a texture channel of a `FFCanvas` a set of extension methods for the `FFCanvas` are provided.
There are currently two main approaches for drawing:

- **Decal projection**: a decal, defined by a `FFDecal`, is projected on the surfaces.
The projection is defined by a projection-view matrix, similar to a camera, that describes a projection volume or frustum in world-space ([`Matrix4x4.Perspective`](https://docs.unity3d.com/ScriptReference/Matrix4x4.Perspective.html)).
To project a decal on a `FFCanvas`, call the `ProjectDecal()` extension method.
    
- **Brushes**: a brush is a 3D shape in the scene (e.g. a sphere) and the parts of the surfaces' renderers intersecting this shape are painted. How exactly the texture is painted is defined using a `FFBrush`. To draw a brush on a `FFCanvas`, call the `DrawSphere()`, `DrawDisc()`, or `DrawCapsule()` extension methods.

For example code snippets on how these methods can be used, take a look at `FluidFlow/Example/Scripts/DrawExamples.cs`.