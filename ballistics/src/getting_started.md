# Getting Started

Check out the included example scene in the `Ballistics/Example/` folder.

## Minimal Setup

- create a `Assets/Create/Ballistics/Ballistic Settings` object in your project folder
- add an empty game object to your scene and attach a `Environment Provider` component to it
    - assign the `Ballistic Settings` object you have created before
- attach a `Weapon` component to a weapon in your scene
    - assign a transform at the end of the weapon barrel as the `Bullet Spawn Point` (z-axis pointing forward)
    - create a `Assets/Create/Ballistics/Bullet Info` object in your project folder
    - assign this `Bullet Info` object in the `Weapon` component
- calling `Shoot()` on the `Weapon` component will fire a bullet (you can use the `Simple Weapon Input` component, to shoot  when clicking the left mouse button)

**Note!** As so far no impact handler or visual bullet provider is set up, you will *NOT* see the bullet or any impacts!

## Visual Bullet Provider

Bullet Ballistics separates the ballistics simulation from the visual projectiles flying through the scene.
This allows to optimize the rendering when thousands of projectiles are active simultaneously, and allows you to customize the rendering to your needs.

- create a `Assets/Create/Ballistics/Visual Bullet Providers/Pooled Bullet Provider` object, and assign a bullet prefab with a `Pooled Bullet` component attached
- assign the new `Pooled Bullet Provider` object to the `Visual Bullet Provider` field of your weapon

You should now be able to see the bullet prefab when shooting the weapon.

## Impact Handler

Similar to the `Visual Bullet Provider` the `Impact Handler` allows you to customize what should happen on a projectile impact.

- create a `Assets/Create/Ballistics/Impact Handler/Generic Impact Handler` object in your project folder
    - assign the impact handler object to the `Global Impact Handler` field of your `Environment Provider` component
- enabling `Handle Rigidbody Forces` should cause rigidbodies to be affected by projectile impacts
- adding a audio clip to the `Impact Sounds` list, will cause an impact sound at any impact of a projectile with the scene

## Ballistic Materials

Ballistic Material define how a projectile will behave when interacting with a collider.

- create a `Assets/Create/Ballistic/Ballistic Material` object in your project folder
    - a Ballistic Material is a unity `Physic Material` with an associated `Ballistic Material` object
    - you can promote/demote a `Physic Material` to/from a `Ballistic Material` by right-clicking the `Physic Material` and selecting `Assets/Create/Ballistics/To BallisticMaterial` / `Unlink BallisticMaterial`
- assign a Ballistic Material to a collider by setting the `Material` field of the collider to a `Physic Material` associated with a `Ballistic Material` object
- by change the properties of the Ballistic Material to alter the bullet interactions

For more information on each of these topics check out the following chapters.