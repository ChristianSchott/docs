# Environment Provider

Each scene using Bullet Ballistics should have a single game object with a `Environment Provider` component attached.

The `Environment Provider` configures the ballistic simulation setting for the current scene.
Including:
- Ballistic Settings
- Wind Velocity
- Global Impact Handler
- Bullet Path Debugging (Editor only)

In theory these settings can also be adjusted manually in the static `Ballistics.Core` class.
The `Environment Provider` automatically configures the `Ballistics.Core` on startup/ scene change.

The fields of the `Environment Provider` will be explained further in the respective chapters.
