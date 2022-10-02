# Visual Bullet Provider

A `Visual Bullet Provider` is just a scriptable object, with a `GetVisualBullet()` method, which returns a new `IVisualBullet`.

```C#
public abstract class VisualBulletProviderObject : InitializableScriptableObject
{
    public abstract IVisualBullet GetVisualBullet();
}
```

The `IVisualBullet` interface consists of:

```C#
public interface IVisualBullet
{
    void InitializeBullet(in BulletPose pose, in float3 visualOffset);  // start of ballistic simulation
    void UpdateBullet(in BulletPose pose);                              // pose update, each simulation cycle
    void DestroyBullet();                                               // bullet has been destroyed (stopped or timeout)          
}
```

The `visualOffset` argument of `InitializeBullet()` will be explained further in the chapter about the [Weapon](./weapon.md) component.

The `Visual Bullet Provider` adds a layer of abstraction between the simulation and the rendering.
This allows switching between rendering strategies easily.

For example, the `Pooled Bullet Provider` uses basic prefab instanciation for rendering the visual bullet (+ pooling instead of destroying bullets, to minimize garbage collection overhead).

Switching the visual bullet of a `Weapon` to the `Tracer Bullet Provider` can be done, by simply replacing the `Visual Bullet Provider` of the `Weapon` component.
The `Tracer Bullet Provider` draws bullets, by rendering a trail between the last three bullet positions.
The `Tracer Bullet Provider` can render bullets very efficiently, as it is able to combine ~10.000 bullets into a single draw call. 

With this flexibility it is very easy to create your own `Visual Bullet Provider`s which fit your game's requirements optimally!