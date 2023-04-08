# Frequently Asked Questions

Feel free to send me your questions, so I can add them here ([mr3d.cs@gmail.com](mailto:mr3d.cs@gmail.com)).

- **Does FluidFlow support URP, HDRP, etc.?**

    Yes, FluidFlow is largely independent of the rendering pipeline your project is using.
    However, for overlaying the fluid texture you will need a shader, supported by your rendering pipeline, which is able to overlay the fluid texture.
    
- **How can I increase the speed at which the fluid flows?**

    In each fluid simulation step, the fluid moves from one pixel to the neighboring pixels depending on gravity.
    Therefore, the maximum fluid speed is limited by one pixel per simulation step.
    The trivial way for altering the fluid speed is decreasing the fluid texture resolution, so each pixel is larger, or increasing the update rate of the fluid simulation.
    However, keep in mind, that the latter has a performance cost, so you should not go too far with this approach.
    
- **Does FluidFlow work on mobile platforms?**

    Yes, FluidFlow was successfully tested on a Google Pixel 3a, and should also be able to run on older devices.
    iOS based devices should also work, however, I am not able to test this myself. 
    I would greatly appreciate it, if you share your experiences using FluidFlow on Apple devices.
    
- **How can I access/safe the generated textures?**

    You can access the internal texture, by accessing the `TextureChannels` property on your `FFCanvas`. E.g. `myCanvas.TextureChannels["Fluid"]`.
    As the texture data is stored inside a RenderTexture, it does only exist in GPU memory.
    If you want to access the texture data from C#, or save the texture, you will have to read it to CPU memory first.
    FluidFlow allows you to call the `myRenderTexture.RequestReadback()` method, to simplify this process. 
    Check the `SaveTextureChannel()` function in `Scripts/Extensions/FFCanvasExtensions.cs` for a more elaborate example.