# Zeroing Crosshair Generator

Due to bullet drop when gravity is enabled, bullets will hit below where you have aimed.
To correct for this, you have to aim above your target.

Given a `Weapon` and a set of distances, this component will generate and draw reticle 'dots' for hitting targets at the given distances.

The other fields of this component allow you to specify the appearance of the generated indicator mesh.
You can use this generated mesh directly, or use it as a template for drawing a custom reticle with the proper offsets in an image creation program of your choice.
