# Version History

## Compatibility

| Conduit version | Unity requirement | Status |
|---|---|---|
| 1.0.0 | 6000.0 or later | Current |

Conduit targets Unity 6000.0 and later exclusively. Earlier Unity versions (2022 LTS and prior) are not supported and have not been tested — several APIs Conduit depends on (including current `AudioImporterSampleSettings` behavior) changed in Unity 6.

## Versioning

Conduit follows [Semantic Versioning](https://semver.org/): a major version bump signals a breaking change, minor versions add functionality without breaking existing policies or scripted API usage, and patch versions are fixes only.

## Upgrading

Minor and patch upgrades within Conduit 1.x require no migration steps — existing `ImportPolicy` assets and project settings continue to work unchanged, since new rule fields are always opt-in by default. A future major version that introduces breaking changes will ship with its own migration guide, linked from [Changelog](changelog.md) at that point.
