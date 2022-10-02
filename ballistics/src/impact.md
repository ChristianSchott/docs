# Impact Handler

Similar to a `Visual Bullet Provider` a `Impact Handler` is also a scriptable object, which adds a layer of abstraction between the ballistic simulation and the rest of your game.

```C#
public abstract class ImpactHandlerObject : InitializableScriptableObject, IImpactHandler
{
    public abstract HandledFlags HandleSurfaceInteraction(in SurfaceInteractionInfo impact, HandledFlags flags);
    public abstract HandledFlags HandleImpact(in ImpactInfo impact, HandledFlags flags);
}
```

- `HandleSurfaceInteraction` is called whenever a projectile first enters, ricochets, exits, or stops inside of a collider
- `HandleImpact` is called when a projectile penetrates a collider

Similar to the `Pooled Bullet Provider` the `Pooled Impact Instantiator` instantiates a prefab with an attached `Impact Object` component at a surface interaction (and pooles the instantiated prefabs).

Alternatively, the `Generic Impact Handler` provides default handling for rigidbody interactions, a damage system, and audio effects.

## Chain Of Responsibility

As some bullet interactions might be specific to the bullet, material, or collider, the `Impact Handler` system is structured hierarchically.

The `Bullet Info`, `Ballistic Material`, `Environemnt Provider`, and collider can each provide an `Impact Handler`.
When a bullet hits a collider the impact handlers are called (if present) in this order:

- `Bullet Info` specific `Impact Handler`
- `Ballistic Material` specific `Impact Handler`
- global impact handler provided by the `Environment Provider`

The collider specific `Impact Handler` is not called by default, as this requires a `GetComponent` call!
You can query for a collider specific `Impact Handler` inside any of the above impact handling stages, by trying to get a component inheriting the `IImpactHandler` interface and calling the specific handler.

When collider specific a `Impact Handler` is needed globally, you can use a `Generic Impact Handler` as the global `Impact Handler`, and enable `CheckForPerColliderHandler`.

At each step of the `Impact Handler` chain you receive the `HandledFlags` of the previous handlers, and can return which flags have been handled by the current handler.
This allows, for example, a bullet impact handler to override the bullet hole prefab of the global impact handler, or a material impact handler to alter the impact sound effect.

## Impact Info

```C#
public readonly struct SurfaceInteractionInfo
{
    public enum InteractionType
    {
        STOP,
        ENTER,
        RICOCHET,
        EXIT,
    }
    public readonly InteractionType Type;
    public readonly RaycastHit HitInfo;
    public readonly float3 Velocity;
    public readonly float SpreadAngle;
    public readonly float SpeedFactor;      // percentage of speed lost on ricochet
    public readonly BulletInfo BulletInfo;
    public readonly Weapon Weapon;
}

public readonly struct ImpactInfo
{
    public readonly RaycastHit HitInfo;
    public readonly float3 EntryVelocity;
    public readonly float ImpactDepth;
    public readonly float EnergyLossPerUnit;
    public readonly BulletInfo BulletInfo;
    public readonly Weapon Weapon;
}
```