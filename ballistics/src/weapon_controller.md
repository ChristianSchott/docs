# Weapon Controller

The `Weapon Controller` component is a convenience wrapper around a basic `Weapon` component.

It allows your `Weapon` to behave like one of the most common weapon types (`Full Auto`, `Single Shot`, `Shotgun`, or `Burst`).
You only have to pull/release the trigger using: 
``` C# 
WeaponController.SetTrigger(bool held);
```

It also allows you to add basic spread and bullet management.

## Zeroing

The `Weapon Controller` also includes zeroing the weapon, to counteract bullet drop caused by gravity for a given distance.

Specify the `Distances` you want to be able to zero your weapon at, and set the `CurrentZeroingIndex`.
**Note!** when updating the `Weapon`/`Bullet Info` or `Distances` at runtime, you have to call `UpdateZeroing()`, to recalculate the zeroing angles.

The `Weapon Controller` simply angles the bullet's shoot direction slightly upwards to hit targets further away.

## Spread Controller

```C#
public class SpreadController : MonoBehaviour
{
    public virtual Vector3 CalculateShootDirection(Weapon weapon) { return weapon.BulletSpawnPoint.forward; }
    public virtual void BulletFired() { }
}
```

`CalculateShootDirection` is called for every bullet fired by the weapon and specifies in which direction it is fired.
`BulletFired` is called by the `WeaponController` each time  is shoots (regardless of the number of bullets fired in the single shot).
This can be used for e.g. increasing the spread angle of following bullets.

As an example for a custom spray controller, `Default Spread Controller` implements a Counter-Strike-style spray pattern controller, where you can specify the spray pattern via unity's `Animation Curve`s.

## Magazine Controller

```C#
public class MagazineController : MonoBehaviour
{
    public virtual bool IsBulletAvailable() { return true; }
    public virtual void BulletFired() { }
}
```

The `Magazine Controller` is queried each time before the `Weapon Controller` is fired, to check if `IsBulletAvailable()`.

`DefaultMagazineController` implements a very basic magazine based bullet management.