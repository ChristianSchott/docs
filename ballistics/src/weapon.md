# Weapon

The `Weapon` component is usually the main entry-point for interacting with the ballistics simulation.

It is mainly just a convenience component, that allows you to combine a `Bullet Info`, `Visual Bullet Provider`, and spawn point transform, so you only have to call `Weapon.Shoot()` to fire a bullet. 

## Visual vs. Bullet Spawn Point

The `Weapon` component exposes two spawn point transforms, the `Bullet Spawn Point` and `Visual Spawn Point`.

- `Bullet Spawn Point` is the position used for the internal ballistics simulation and collision detection (z-axis of the transform pointing forward)
- `Visual Spawn Point` provides the initial position for the `Visual Bullet` (if present)

This distinction is often useful for first person shooter games, because the hit detection should usually originate from the center of the camera, while the visual bullet representation should appear to originate at the end of the weapon's barrel.

This obviously causes a discrepancy between the visual bullet and the internal hit detection.
To mitigate this offset, Bullet Ballistics automatically moves the visual bullet position towards the internal bullet position over a given distance set in the `BallisticSettings` `Visual To Physical Distance`.

## Manually Queuing Bullets

Internally, `Weapon.Shoot()` calls `Ballistics.Core.AddBullet()` with a new `BulletInstance`.

``` C#
public readonly struct BulletInstance
{
    public readonly Weapon Weapon;
    public readonly Vector3 Position;
    public readonly Vector3 Direction;
    public readonly BulletInfo Info;
    public readonly IVisualBullet Projectile;
}
```

You can also manually add bullets to the ballistics simulation using `Ballistics.Core.AddBullet()` at any point.
Note however, that `AddBullet()` just queues bullets for processing, so the ballistics calculations can be batched together with all other active bullets.

Queued bullet instances are added to the simulation just after the main `Update()` loop.
So all bullets queued after `Update()` (e.g. `LateUpdate()`) will not be processed until the next frame.
