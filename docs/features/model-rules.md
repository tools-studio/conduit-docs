# Model Rules

Model Rules govern `ModelImporter` settings. Each rule is opt-in — check it to enforce it, leave it unchecked to ignore that property entirely.

| Rule | Governs | Notes |
|---|---|---|
| Mesh Compression | `meshCompression` | Off, Low, Medium, or High. |
| Read/Write Enabled | `isReadable` | Required for runtime mesh manipulation or collision baking from script. Doubles memory usage otherwise. |
| Optimize Mesh | `optimizeMeshPolygonOrder` | Reorders vertices for GPU cache efficiency. |
| Import Normals | `importNormals` | Import, Calculate, None, or Flip. |
| Import Tangents | `importTangents` | Import, Calculate Legacy, Calculate Legacy With Split Tangents, None, or Calculate Mikk. |
| Import BlendShapes | `importBlendShapes` | Off for static props that don't need morph targets. |
| Import Visibility | `importVisibility` | Node visibility flags from the source file. |
| Import Cameras | `importCameras` | Rarely needed outside cutscene or cinematic imports. |
| Import Lights | `importLights` | Rarely needed outside cutscene or cinematic imports. |
| Import Animations | `importAnimation` | Off for static meshes to skip animation-clip processing entirely. |
| Scale Factor | `globalScale` | The uniform scale applied at import. |

## A typical setup

A static-prop folder commonly wants Import BlendShapes, Import Cameras, Import Lights, and Import Animations all off — none of them apply to a prop, and turning them off skips unnecessary import work. A character-rig folder wants the opposite: Import BlendShapes on, Import Tangents set appropriately for the shader in use.

## What isn't covered

Model Rules apply to Unity's built-in model importer only. LOD generation is a related but separate feature — see the LOD settings on the policy, and note that mesh decimation (reducing polygon count per LOD level, not just hiding renderers) requires a third-party library and isn't available in the current version. See [Limitations](../limitations.md).
