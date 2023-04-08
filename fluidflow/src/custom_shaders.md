# Custom Shaders

As the `FFCanvas` combines multiple renderers into a texture atlas, and some renderers might use the lightmapping UVs for drawing, a special material shader is required for displaying the generated textures on the models.
Depending on the rendering pipeline used, unity's shader graph can be used, or a custom surface shader can be created.

FluidFlow provides helper functions for both of these options.
A set of custom shader graph nodes are defined in `FluidFlow/Editor/CustomShaderGraphNodes`, and the `FluidFlow/Shaders/FFShaderUtil.cginc` provides useful helper functions for writing surface shaders.
Take a look at the included shaders, for an example of how these helper functions can be utilized.

On initialization the `FFCanvas` will set a `Vector4`/`float4` uv transformation property `(scale.xy, offset.xy)` to each material of its **Surfaces'** renderers.
Additional a keyword is enabled, of a float property set to `0`/`1`, if the mesh's secondary uv set should be used.
The name of these properties/keyword can be set in the **Material Property Overrides** of the `FFCanvas`. 

Unity automatically creates a tiling and offset property for each texture property, however, depending on the shader implementation, the tiling and offset transformation may not be applied to the UV before sampling the texture.
This `float4` property can accessed by adding a `_ST` postfix to the texture property name (e.g. `_MainTex` $\rightarrow$ `_MainTex_ST`).

The function for transforming a `uv` coordinate to texture atlas coordinates of the `FFCanvas` given a `float4 uvTransform;` is:

```HLSL
float2 TransformUV(float2 uv, float4 uvTransform)
{
    return uv * uvTransform.xy + uvTransform.zw;
}
```

## Shader Graph

Accessing the `Tiling` and `Offset` of a texture in Shader Graph can be done using the [Split Texture Transform Node](https://docs.unity3d.com/Packages/com.unity.shadergraph@16.0/manual/Split-Texture-Transform-Node.html).
On older versions of Shader Graph, you have to manually add a `float4` property named after the texture property + `_ST` postfix (e.g. `_FluidTex_ST`).

If your meshes may use secondary/lightmap uvs, define a local multi compile boolean keyword e.g. `FF_UV1` or `float` property, for detecting if the secondary uv should be used.

The helper `FF Atlas Transformed UV` node will automatically switch between `uv0` and `uv1`, depending on if the `Use Secondary UV` flag is set, and transform the uv using a provided transformation vector.
For performing texture coordinate transformation manually, you can use the `FF Atlas Transform` helper node.

When sampling a fluid texture, use the custom `FF Unpack Fluid` node, for easy access to the fluid's color, normal and fluid amount information.
Optionally you can use the `FF Distort UV` node, to add a small random offset to the uv before sampling the fluid texture for a more natural look.

The `FF Color Scatter` node approximates light scattering while traveling through the fluid, making sections with more fluid appear darker.

## Surface Shader

Overview of a basic surface shader with FluidFlow support:

```
Shader "FluidFlow/MyCustomShader"
{
	Properties
	{
		[...]
		[HideInInspector] _FluidTex("Fluid", 2D) = "black" {}
	}
	SubShader
	{
		Tags { "RenderType" = "Opaque" }
		LOD 200

		CGPROGRAM
		#include "UnityCG.cginc"
		#include "FFShaderUtil.cginc"

		#pragma target 3.0
		#pragma surface surf Standard fullforwardshadows vertex:vert

		#pragma multi_compile_local __ FF_UV1

		struct Input
		{
			[...]            
			FF_SURFACE_INPUT
		};

		sampler2D _FluidTex;
		float4 _FluidTex_ST;

		void vert(inout appdata_full v, out Input o) {
			UNITY_INITIALIZE_OUTPUT(Input, o);
			FF_INITIALIZE_OUTPUT(v, o, _FluidTex_ST);
		}

		void surf(Input IN, inout SurfaceOutputStandard o)
		{
            // basic shader stuff
            float3 base_color = [...]
            float3 base_normal = [...]
            [...]

            // sample fluid texture
			fixed4 fluid = tex2D(_FluidTex, IN.FF_UV_NAME);
			float3 fluid_color = fluid.FF_FLUID_COLOR;
			float fluid_height = min(fluid.FF_FLUID_AMOUNT, 1);
			float3 fluid_normal = FF_UNPACK_FLUID_NORMAL(IN, _FluidTex, 1);

            // interpolate
			o.Albedo = lerp(base_color, fluid_color, fluid_height);
			o.Normal = lerp(base_normal, fluid_normal, fluid_height);
		}
		ENDCG
	}
	FallBack "Diffuse"
}
```

In summary:

- Add a `[HideInInspector] _FluidTex("Fluid", 2D) = "black" {}` property.

- Include `"FFShaderUtil.cginc"` (may require a relative path e.g. `#include "../../FluidFlow/Shaders/FFShaderUtil.cginc"`) for access to some utility functions.

- Create a local multi compile shader variant for using the secondary UV set (`#pragma multi_compile_local __ FF_UV1`).
    
- Add a custom vertex function, which initializes the `FF_SURFACE_INPUT` using the `FF_INITIALIZE_OUTPUT` macro. Alternatively you can use the `FFAtlasTransformUV()` function to transform UVs to `FFCanvas` texture atlas UVs manually.

- Inside the surface shader, you can now access the atlas transformed texture coordinate, via the `FF_UV_NAME` field on the input struct.
This field was initialized by the `FF_INITIALIZE_OUTPUT` macro in the vertex shader.

- When sampling a fluid texture, you can use `.FF_FLUID_COLOR` (just an alias for `.rgb`) to access the color of the fluid, and `.FF_FLUID_AMOUNT` to access the fluid amount (just an alias for `.a`).

- Optionally you can use the `FFDistortUV` function, to add a small random offset to the uv before sampling the fluid texture for a more natural look.

- Use the `FF_UNPACK_FLUID_NORMAL` macro to approximate the fluid normal based on the surrounding fluid amount.
