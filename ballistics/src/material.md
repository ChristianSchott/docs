# Ballistic Material

A `Ballistic Material` specifies how a projectile interacts with a collider.

Custom `Ballistic Materials` can be created by implementing the `IBallisticMaterial` interface, and associating them with a `Physic Material` at runtime, by adding them to the `BallisticMaterialCache`.

However, for must cases it will be sufficient to create a `Ballistic Material` scriptable object via `Assets/Create/Ballistics/Ballistic Material`.
These will be associated with a `Physic Material` automatically.

```C#
public interface IBallisticMaterial
{
    MaterialImpact HandleImpact(in float3 velocity, BulletInfo info, in RaycastHit rayHit);
    float GetEnergyLossPerUnit();
    float GetSpreadAngle();
    IImpactHandler GetImpactHandler();
}
```

Combining `Ballistic Material`s with unity's `Physic Material`s has the advantage that unity directly provides the `Physic Material` of the hit collider from a raycast query, so no `GetComponent` call to something like a *`Ballistic Material Provider`* component on the object is required, which would come with a performance impact.

The main method of the `IBallisticMaterial` interface is `HandleImpact`.
It provides all the available information about the impact, and the `Ballistic Material` decides how the projectile should continue, by returning a `MaterialImpact` struct.

```C#
public readonly struct MaterialImpact
{
    public enum Result
    {
        STOP,
        ENTER,
        RICOCHET
    }
    public readonly Result ImpactResult;
    public readonly float SpreadAngle;
    public readonly float SpeedFactor;
}
```

The bullet can either be stopped right away, be reflected (ricochet), or penetrate (enter) the collider.
The `SpeedFactor` is multiplied with the current bullet velocity to result in the new bullet velocity.

The `SpreadAngle` only applies to `Ricochet` bullets. 
When a bullet penetrates a collider, the spread is applied on the exit and is determined by the `GetSpreadAngle` method.

For additional information about the `Ballistic Material` object fields, please refer to the inspector tooltips.