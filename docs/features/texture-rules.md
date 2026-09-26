# Texture Rules

Texture Rules govern the import settings of textures. Each rule is opt-in: check it to enforce it, leave it unchecked to ignore that property entirely.

## Purpose

Keep texture size, compression, and sampling consistent across a folder so a stray setting cannot inflate build size or change how a texture renders.

## Configuration

| Rule | Governs | Notes |
|---|---|---|
| Max Texture Size | Maximum size | Applied on the default platform settings. |
| Compression | Texture compression | None, Low Quality, Normal Quality, or High Quality. |
| Generate Mip Maps | Mip map generation | Off is common for UI textures that are always shown at native resolution. |
| Wrap Mode | Wrap mode | Repeat or Clamp. |
| Filter Mode | Filter mode | Point, Bilinear, or Trilinear. |
| Is Readable | Read/Write access | Enabling this doubles the texture's memory footprint at runtime. Only enable it where you actually read pixels from script. |
| sRGB (Color Texture) | sRGB sampling | Off for normal maps, masks, and other non-color data. |
| Texture Type | Texture type | Default, Normal Map, Sprite, and the other standard Unity texture types. |
| Alpha Is Transparency | Alpha handling | Only meaningful when the texture actually has an alpha channel. |

### Per-platform overrides

A texture policy can also hold a list of per-platform overrides: Max Texture Size, Compression, Format, and Allows Alpha Splitting, keyed by platform name (`iPhone`, `Android`, and so on, matching the platform tab names in Unity's Texture Importer). These apply on top of the default settings for that one platform, the same way Unity's own per-platform tabs work.

Platform overrides appear as a standard list in the policy inspector. There is no dedicated per-platform interface in 1.0.0, so add entries by platform name exactly as Unity's Texture Importer names them.

## Examples

A UI folder commonly wants Max Texture Size capped low, Generate Mip Maps off (UI is not viewed at a distance), and Texture Type set to Sprite.

A normal map folder commonly wants sRGB off and Texture Type set to Normal Map, with everything else left to a project-wide default.
