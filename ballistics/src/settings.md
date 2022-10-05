# Ballistic Settings

The `Ballistic Settings` object exposes parameters for adjusting the internal ballistics simulation.
To bind a `Ballistic Settings` object to a given scene, use an `Environment Provider` component.

Create a new `Ballistic Settings` object under `Assets/Create/Ballistics/Ballistic Settings`.

Alternatively, you have direct access to the simulations settings via the static `Ballistics.Core` class.
Make sure you call `Ballistics.Core.UpdateEnvironment()`, after changing the simulation settings, to rebuild the `Ballistics.Environment` used by the internal simulation jobs.

For additional information about the `Ballistic Settings` object fields, please refer to the inspector tooltips.
