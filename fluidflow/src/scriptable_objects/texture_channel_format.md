# Texture Channel Format

- `Assets/Create/Fluid Flow/Texture Channel Format`

Provides a list of possible [`GraphicsFormat`](https://docs.unity3d.com/ScriptReference/Experimental.Rendering.GraphicsFormat.html) for a **Texture Channel** of a `FFCanvas`.

On startup the first `GraphicsFormat` in the list available on the current platform is selected.

If none from the list are available, similar compatible formats are searched.