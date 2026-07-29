# Texture Rules

Texture Rules govern `TextureImporter` settings. Each rule is opt-in — check it to enforce it, leave it unchecked to ignore that property entirely.

| Rule | Governs | Notes |
|---|---|---|
| Max Texture Size | `maxTextureSize` | Applied per-platform on the default platform settings. |
| Compression | `textureCompression` | None, Low Quality, Normal Quality, or High Quality. |
| Generate Mip Maps | `mipmapEnabled` | Off is common for UI textures that are always shown at native resolution. |
| Wrap Mode | `wrapMode` | Repeat or Clamp. |
| Filter Mode | `filterMode` | Point, Bilinear, or Trilinear. |
| Is Readable | `isReadable` | Enabling this doubles the texture's memory footprint at runtime. Only enable where you actually call `GetPixels` or similar. |
| sRGB (Color Texture) | `sRGBTexture` | Off for normal maps, masks, and other non-color data. |
| Texture Type | `textureType` | Default, Normal Map, Sprite, and the other standard Unity texture types. |
| Alpha Is Transparency | `alphaIsTransparency` | Only meaningful when the texture actually has an alpha channel. |

## A typical setup

A UI folder commonly wants: Max Texture Size capped low, Generate Mip Maps off (UI doesn't get viewed at a distance), and Texture Type set to Sprite. A normal map folder commonly wants: sRGB off and Texture Type set to Normal Map, with everything else left to a project-wide default.

## Per-platform overrides

Beyond the default settings above, a texture policy can hold a list of per-platform overrides (Max Texture Size, Compression, Format, and Allows Alpha Splitting, keyed by platform name — `iPhone`, `Android`, and so on, matching Unity's own platform tab names). These apply on top of the default settings for that one platform, the same way Unity's own per-platform tabs work. Platform overrides appear as a standard list in the policy inspector; there's no dedicated per-platform UI yet, so add entries by platform name exactly as Unity's Texture Importer names them.
