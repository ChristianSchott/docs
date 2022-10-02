# Introduction

**Bullet Ballistics 2** allows you to simulate realistic bullet physics in your games with minimal effort.

Unlike a rigidbody based system, it can handle high velocity projectiles without any problems, while also taking into account physical forces like gravity and air resistance.
Additionally, projectiles can penetrate or ricochet of colliders in the scene realistically, depending on physical properties assigned to the colliders.

The main develop goals for version 2 of the Bullet Ballistics asset pack were:
- performance 
    - multithreading via unity's job system
    - reducing runtime GC allocations
    - reducing draw calls
- customizability
    - customizable impact handling
    - customizable projectile rendering
    - adjusting simulation parameters
- ease of use