# Roadmap

This is what's planned, not what's shipped — see [Changelog](https://github.com/afterix-hub/conduit/blob/main/CHANGELOG.md) for what's actually released. Dates aren't committed; version numbers and scope may change before release.

## 1.1.0

- **Video Rules applicator** — `VideoPolicyApplicator`, applying policy to `VideoClipImporter` properties. `VideoRuleSet` already exists as a policy data type; this ships the missing enforcement half. See [Limitations](../docs/limitations.md).
- **`ConduitRulePreset`** — a reusable, named rule set you can apply across multiple policies instead of configuring the same rules folder by folder.
- **CI JSON report schema 1.1** — see [Configuration Reference](../docs/configuration.md) for the current schema.

## 1.2.0

- **`ConduitAssetOverride`** — a per-asset policy exception, addressing the current limitation where only folders can be governed. See [Limitations](../docs/limitations.md).
- **`SimplygonLodAdapter`** — optional mesh decimation for LOD generation via the Simplygon SDK, when present. Conduit's own LOD generation will remain hierarchy-clone-only for projects that don't use Simplygon.
- **`LodGenerationMode.Simplygon`** activated as a selectable mode once the adapter ships.

## How this list changes

Planned items move to [Changelog](https://github.com/afterix-hub/conduit/blob/main/CHANGELOG.md) once released. This page reflects the current plan at the time you're reading it — check the [Changelog](https://github.com/afterix-hub/conduit/blob/main/CHANGELOG.md) for what's actually available in the version you have installed.

## Suggesting something not listed here

See [Feature Requests](../support/feature-requests.md).
