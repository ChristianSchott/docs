# Texture Channel

- `Assets/Create/Fluid Flow/Texture Channel`

A `TextureChannel` abstracts away a shader's texture property name from the usage of the texture.
For example a `Color` `TextureChannel` may correspond to the `_MainTex` texture property of one shader, but to the `_BaseMap` property of another shader.

The name of a `TextureChannel` has to be unique within a project, as the name can be used for indirectly referencing a `TextureChannel` using `TextureChannel.TryResolve("name", out var textureChannel)`.

A `TextureChannelReference` allows referencing a `TextureChannel` directly, or indirectly and resolving at runtime.
