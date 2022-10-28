# Introduction

**Bullet Ballistics** combines the **accuracy** of hit-scan based systems, with the **realistic bullet drop** of rigidbody based projectiles, and additionally includes **air resistance** in its ballistics simulation.

It has been built from the ground up with **performance** in mind, making full use of **multithreading** utilizing unity's job system and burst compiler.
While also minimizing runtime garbage collection allocations wherever possible.

Another main development goal was creating **customizable** interfaces to the core ballistics simulation, allowing for custom bullet rendering, material interactions, and impact handling.

All together, Bullet Ballistics empowers you to simulate hundreds to thousands of physically accurate projectiles in your games simultaneously without a hustle.

## Links

- [Asset Store](https://assetstore.unity.com/packages/slug/72755)
- [Online Demo](https://christianschott.github.io/demos/ballistics/index.html)
- [Documentation](https://christianschott.github.io/docs/ballistics/book/index.html)
- [Video Tutorials](https://youtube.com/playlist?list=PLH6XAuNbhBuvQNtq6kag7_sbsSytAm0ft)
- E-Mail: [mr3d.cs@gmail.com](mailto:mr3d.cs@gmail.com)

## Required Unity Packages

- `Unity.Burst`
- `Unity.Mathematics`


## Features

- Ballistics Simulation
    - Gravity / Bullet Drop
    - Air Resistance
    - Wind
    - Magnus Effect (Bullet Spin)
    - Weapon Zeroing + Reticle Generation
    - Trajectory Visualization
- Material Interactions
    - Object Penetration
    - Ricochets
- High Performance
    - Multi-Threading
    - Minimal Runtime GC Allocations
    - Batched Projectile Rendering
    - Object Pooling
- Customizability
    - Bullet Rendering
    - Impact Handling
    - Material Interactions
- Clear Custom Inspectors
    - Physical Unit Selection (metric/imperial)


## Technical Details
The ballistics simulation is completely independent of the visual projectiles in the game.
This allows for a lot of customizability in the way the bullets are rendered: basic prefab instances, GPU instancing, custom renderer? Your choice!
This also makes Bullet Ballistics inherently independent of the Rendering Pipeline you target.

Internally, Bullet Ballistics uses a numerical approximation to simulate the ballistic trajectory.
For collision detection, a raycast is fired between the last two simulated positions of a given projectile.
All ballistics processing is automatically batched using unity's RaycastCommand API and job system for optimal performance.

## Limitations
Bullet Ballistics does (currently) not support ballistic effects like spin drift, coriolis force, magnus effect, or similar, as they only affect very long range projectiles, and are usually negligible for normal game scenarios.
Feel free to contact me with any feature requests!