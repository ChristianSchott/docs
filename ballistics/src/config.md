# Ballistics Configuration

Bullet Ballistics allows you to disable parts of its functionality you do not need, in order to optimize performance for your use case.

Navigate to `Window/Ballistics/Ballistic Config` to open the configuration window.
There you can select which features you want to disable for which build target.
Do not forget to hit `Apply` to apply the changes.

## Scripting Define Symbols

The configuration window is just a convenience utility for setting compiler define symbols.
You can also set them manually under `Project Settings/Player/Other Settings/Script Compilation`.

Currently supported define symbols:
- `BB_NO_AIR_RESISTANCE`: disable drag force on projectiles
- `BB_NO_SPIN`: disable forces due to bullet spin (depends on air resistance)
- `BB_NO_DISTANCE_TRACKING`: disable tracking the distance a bullet has traveled