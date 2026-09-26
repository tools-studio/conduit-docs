# Model Rules

Model Rules govern the import settings of 3D models. Each rule is opt-in: check it to enforce it, leave it unchecked to ignore that property entirely.

## Purpose

Skip unnecessary import work and keep mesh settings consistent across a folder, for example by turning off animation, cameras, and lights for static props.

## Configuration

| Rule | Governs | Notes |
|---|---|---|
| Mesh Compression | Mesh compression | Off, Low, Medium, or High. |
| Read/Write Enabled | Read/Write access | Required for runtime mesh manipulation or collision baking from script. Doubles memory usage otherwise. |
| Optimize Mesh | Polygon order optimization | Reorders vertices for GPU cache efficiency. |
| Import Normals | Normals | Import, Calculate, None, or Flip. |
| Import Tangents | Tangents | Import, Calculate Legacy, Calculate Legacy With Split Tangents, None, or Calculate Mikk. |
| Import BlendShapes | Blend shapes | Off for static props that do not need morph targets. |
| Import Visibility | Node visibility | Node visibility flags from the source file. |
| Import Cameras | Cameras | Rarely needed outside cutscene or cinematic imports. |
| Import Lights | Lights | Rarely needed outside cutscene or cinematic imports. |
| Import Animations | Animation | Off for static meshes to skip animation-clip processing entirely. |
| Scale Factor | Import scale | The uniform scale applied at import. |

Model Rules do not support per-platform overrides. The same values apply on every platform.

### LOD generation

A policy also carries LOD Generation settings: Mode (Disabled or Unity Built-In), Screen Relative Heights, Quality Reduction Ratios, and Remove Source Mesh On Import.

LOD generation clones the model's existing hierarchy into LOD levels at the screen-relative height thresholds you set. It does not reduce polygon count. Each LOD level uses the same mesh as the source. Mesh decimation is not part of 1.0.0.

## Examples

A static-prop folder commonly wants Import BlendShapes, Import Cameras, Import Lights, and Import Animations all off. None of them apply to a prop, and turning them off skips unnecessary import work.

A character-rig folder wants the opposite: Import BlendShapes on, and Import Tangents set appropriately for the shader in use.
