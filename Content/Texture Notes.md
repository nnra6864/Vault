---
created: 2025-12-12 05:44:36
tags:
  - note
  - journal
  - 3d
  - 2d
  - asset
  - blender
  - gamedev
  - tip
  - unity
---
## General

### Types

- Albedo/Base/Color - stores only color data
- Diffuse - similar to Albedo, but can also store data from AO, Metallic, and possibly other maps
- Normal - stores normal data which affects how light interacts with the object, usually making a flat surface appear 3D
- Displacement - unlike normal, displacement actually displaces object vertices in 3D space
- Roughness/Smoothness - higher roughness leads to a more blurred reflection, while lower roughness leads to a clear reflection, smoothness is the opposite
- Metallic - affects how metallic the surface looks
- Ambient Occlusion/AO - adds shadows where surfaces are close to each other

### Different Texture Sizes

Using different texture sizes for maps is a smart idea for optimizing [[VRAM]] and storage usage.
An example would be:
8K Albedo/Diffuse/Base/Color
4K Normal
2K Roughness/Smoothness/Metallic/AO
Although it heavily depends on the asset.

### Exporting

Everything except Albedo should be exported as a noncolor image, and normals(and optionally others) should also be 16/32 bit.
Using 100% png compression is also recommended to save up space and [[Git]]/[[LFS]] storage.

### Black Diffuse With Metallic

When using Metallic, you should really reconsider exporting Albedo instead of Diffuse.
For example, Blender takes into account how real materials work due to its [[Physically Based Rendering]] workflow, which results in a black Diffuse render if Metallic is used.
Another option is to set Metallic to 0 before baking.

### Green Color Channel Precision

Green color channel offers highest precision in images/maps with lossy [[Compression]] and can be used to store sensitive data.

## [[Blender]]

### Bake faster

Setting samples down to 10 should result in a nice speed increase with good quality.
On top of that, increasing Properties -> Render -> Performance -> Memory -> Tile Size can reduce bake times from minutes to seconds, [source](https://www.youtube.com/watch?v=McALCOr39rY&t=204s).

## [[Unity]]

### Skybox Mipmaps

These can cause a weird seam, disable them, [source](https://youtu.be/Oatl69iaS80?t=279).

### Max Texture Size

Max Texture Size in Texture Import Settings is just that, a **max** size, it won't magically upscale the texture to 16k, will just make the image smaller if it exceeds that value.
