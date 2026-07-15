# Supported Features

<p align="center">
  <img src="../images/screenshot-05-rule-coverage.png" width="800" alt="Full Model, Texture, Audio, and LOD rule coverage alongside Conduit project settings">
</p>

Shows the full set of Model, Texture, Audio, and LOD Generation rules Conduit can enforce, alongside the Conduit Settings panel where Conduit is enabled or disabled project-wide, the Build Gate can be suppressed on CI, and specific folders can be excluded from scanning.

## Governed today

| Category | Governed via | Details |
|---|---|---|
| Textures | `TextureImporter` | See [Texture Rules](../docs/texture-rules.md), including per-platform overrides. |
| Models | `ModelImporter` | See [Model Rules](../docs/model-rules.md). |
| Audio | `AudioImporter` | See [Audio Rules](../docs/audio-rules.md), including per-platform overrides. |
| LOD generation | `LODGroup` creation | Hierarchy cloning with configurable screen-relative height thresholds. Does not reduce polygon count per level — see [Limitations](limitations.md). |

## Workflow features

- Folder-scoped, cascading policy inheritance with per-property opt-in
- Three enforcement levels: Disabled, Warn Only, Block Build
- Asynchronous, cancellable drift scanning across the whole project or a chosen folder
- Read-only simulation of resolved policy for any single asset
- Manual reimport with type and drift-only filtering
- Build-time gate via `IPreprocessBuildWithReport`, with a CI-runnable equivalent (`CIBridge`)
- Scripted access to every workflow through the `Conduit` static facade — see [API Overview](api-overview.md)
- Editor-only: contributes nothing to player builds

## Not governed

Anything outside `TextureImporter`, `ModelImporter`, and `AudioImporter` — video clips, fonts, ScriptableObjects, prefabs, and any other asset type — is untouched by Conduit. See [Limitations](limitations.md) for what's planned versus what's out of scope entirely.
