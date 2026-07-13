# Changelog

This is a user-facing summary. The complete, detailed changelog ships inside the package as `CHANGELOG.md`.

## Unreleased

### Fixed
- A build-time scan could hang the Editor or a CI build on a large project. Builds now use a safe, non-blocking scan path.
- Demo content installed itself automatically on project load and could delete folders it didn't create. Installing demo content is now an explicit menu action that only ever touches its own folder.
- Fix, Fix All, and Reimport now show a confirmation with the affected asset count before overwriting import settings.
- The runtime assembly is now correctly restricted to the editor, matching Conduit's "no runtime overhead" claim.
- Generated demo model assets no longer produce import warnings.

### Added
- Audio Rules: **Preload Audio Data** and **Load In Background** — see [Audio Rules](../docs/audio-rules.md).

### Changed
- Demo assets are generated on demand instead of shipping as binary files, reducing package size significantly.
- **License changed to MIT.** Conduit is now free and open source. See [LICENSE.md](../LICENSE.md).

## 1.0.0 — 2026-06-09

Initial release. Texture, Model, and Audio Rules with cascading folder policies, three enforcement levels, drift detection, simulation, manual reimport, and a build-time gate with CI support. See [Supported Features](../reference/supported-features.md) for the complete list and [Limitations](../reference/limitations.md) for what's not yet covered.
