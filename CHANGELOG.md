# Changelog — Conduit
All notable changes to this project are documented in this file.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Changed
- Menu path moved from `Tools Studio/Conduit/...` to `Tools/Conduit/...`, matching Unity's own reserved-menu convention (`Window` is reserved for Unity's built-in panels). This affects **Open Window**, all three Demo commands, and the Import Policy creation menu. Update any muscle memory or team documentation referencing the old path.
- About tab now also states the version in its body, alongside the existing footer.

---

## [1.0.0] — 2026-07-13

First public release.

### Added

**Core data model**
- `PolicyProperty<T>` — generic serializable struct with `IsSet` / `Value` fields and `Unset()` / `Set(T)` factory methods
- `ImportPolicy` ScriptableObject — folder-scoped import rule authoring asset with `Create` menu at `Assets/Create/Tools/Conduit/Import Policy`
- `TextureRuleSet` — 9 governed properties: `maxTextureSize`, `compression`, `generateMipMaps`, `wrapMode`, `filterMode`, `isReadable`, `sRGB`, `textureType`, `alphaIsTransparency`
- `ModelRuleSet` — 11 governed properties: `meshCompression`, `readWriteEnabled`, `optimizeMesh`, `importNormals`, `importTangents`, `importBlendShapes`, `importVisibility`, `importCameras`, `importLights`, `importAnimations`, `scaleFactor`
- `AudioRuleSet` — governed properties: `defaultLoadType`, `compressionFormat`, `compressionQuality`, `sampleRateSetting`, `sampleRateOverride`, `forceToMono`, `preloadAudioData`, `loadInBackground`
- `VideoRuleSet` — defined with `[Experimental]` attribute; no applicator ships in 1.0.0
- `TexturePlatformOverride`, `AudioPlatformOverride` — per-platform property override lists within rule sets
- `LodGenerationRule` — `mode`, `screenRelativeHeights[]`, `qualityReductionRatios[]`, `removeSourceMeshOnImport`
- `EnforcementLevel` enum — `Disabled`, `WarnOnly`, `BlockBuild`
- `AssetCategory` enum — `Texture`, `Model`, `Audio`, `Video`, `Unknown`
- `LodGenerationMode` enum — `Disabled`, `UnityBuiltIn`
- `ConduitProjectSettings` ScriptableObject — global settings asset at `Assets/Conduit/ConduitProjectSettings.asset`
- **Audio parity audit.** Reviewed every `AudioImporter` property against Audio Rules coverage. Two properties were evaluated and deliberately excluded: `Normalize` is not exposed as a public C# property in any supported form — only reachable by reflecting into Unity's internal `m_Normalize` field, which has a documented side effect of resetting `forceToMono`, and is not safe for unattended policy application. `Ambisonic` describes how source audio was authored (true B-format 360° content) rather than a studio import convention; applying it by folder policy to non-ambisonic clips would silently corrupt playback.

**Resolution engine**
- `PolicyGraph` — builds from `AssetDatabase.FindAssets("t:ImportPolicy")`; sort-before-insert for deterministic conflict resolution across machines
- `PolicyGraphCache` — `[InitializeOnLoad]` singleton with `GraphVersion` int counter; version-aware cache invalidation
- `PolicyGraphCacheInvalidator` — `AssetPostprocessor` that marks graph dirty on `ImportPolicy` or `ConduitProjectSettings` asset changes
- `PolicyResolver` — version-checked resolution with `ResolutionCache`; `Resolve()` (no trace) and `ResolveWithTrace()` (simulation) paths
- `PolicyMerger` — cascade merge for Texture, Model, Audio; `buildTrace` parameter controls `CascadeEntry` allocation; `MergeVideo` returns `ResolvedPolicy.Empty`
- `ResolutionCache` — bounded LRU cache, capacity 512, O(1) hit/eviction via `LinkedList` + `Dictionary`
- `ResolvedPolicy`, `ResolvedTextureSettings`, `ResolvedModelSettings`, `ResolvedAudioSettings`, `ResolvedVideoSettings`, `ResolvedLodRule` — nullable-property resolved output types
- `CascadeEntry` — readonly struct recording one property assignment in a merge trace

**Import interception**
- `ConduitAssetPostprocessor` — intercepts `OnPreprocessTexture`, `OnPreprocessModel`, `OnPreprocessAudio`, `OnPostprocessModel`; all applicator calls wrapped in try/catch
- `TexturePolicyApplicator` — writes non-null resolved texture settings to `TextureImporter` including per-platform overrides
- `ModelPolicyApplicator` — writes non-null resolved model settings to `ModelImporter`
- `AudioPolicyApplicator` — writes non-null resolved audio settings to `AudioImporter` including per-platform overrides

**LOD generation**
- `LodGenerationCoordinator` — wraps `LodGenerator` in error-contained try/catch; `LodGenerationException` never propagates to Unity's import pipeline
- `LodGenerator` — builds `LODGroup` hierarchy on model root; LOD 0 = source renderers; LOD N = named child `GameObject` with cloned `MeshRenderer` + `MeshFilter`; no mesh decimation in 1.0.0

**Drift detection**
- `DriftScanner` — async, batched, `Stopwatch`-gated time-sliced scan; separate `AssetDatabase.FindAssets` queries per type with GUID deduplication (no combined multi-type queries); synchronous `ScanSync` path for build-time use
- `AssetPropertyComparator` — compares live importer settings against `ResolvedPolicy` for Texture, Model, Audio categories
- `DriftViolation` — readonly struct with `AssetPath`, `PropertyName`, `ExpectedValue`, `ActualValue`
- `DriftReport` — immutable result with `FilterByCategory()` and `FilterByFolder()` projections

**Import simulation**
- `ImportSimulator` — calls `PolicyResolver.ResolveWithTrace()`; never caches results
- `AssetCategoryDetector` — extension-to-category mapping via static `HashSet<string>` per category
- `SimulationResult` — `Unsupported()`, `NoPolicyApplies()`, `FromPolicy()` factory methods
- `SimulatedProperty` — readonly struct with effective value, source policy path, source folder, inheritance flag

**Bulk reimport**
- `ReimportCoordinator` — executes inside `AssetDatabase.StartAssetEditing()` / `StopAssetEditing()`; uses `EditorUtility.GetStorageMemorySizeLong` for on-disk size delta with `FileInfo.Length` fallback
- `ReimportJob` — category-ordered batching (Textures → Models → Audio → Video → Unknown)
- `ReimportResult` — per-asset before/after bytes, total bytes saved; `Empty` static singleton
- `ReimportResult.Empty` — returned by `Conduit.ReimportDriftingAssetsAsync()` when report has no violations

**Build gate**
- `ConduitBuildPreprocessor` — `IPreprocessBuildWithReport`, `callbackOrder = 0`; synchronous drift scan at build time; `CONDUIT_CI_SKIP` define support
- `GateValidationResult` — classifies violations by `EnforcementLevel`; `WillBlockBuild` property

**CI integration**
- `CIBridge.Run()` — headless `-executeMethod` entry point; exit codes 0 (clean), 1 (violations), 2 (failure)
- `CIReportWriter` — JSON report output, schema version `1.0`
- CLI arguments: `-conduitReport <path>`, `-conduitFolder <path>`, `-conduitWarnAsError`

**Public API**
- `Conduit` static facade — `ResolvePolicy`, `GetAllPolicies`, `RebuildPolicyGraph`, `Simulate`, `ScanForDriftAsync`, `ScanFolderAsync`, `ReimportAssetsAsync`, `ReimportDriftingAssetsAsync`, `RunBuildGateCheck`, `GetSettings`, `IsEnabled`
- `#error` compiler directive prevents `Conduit.cs` from compiling outside the Editor assembly

**Editor window**
- `ConduitWindow` — docked `EditorWindow`, minimum size 720×520, header + tab strip + footer
- `ConduitWindowContext` — shared service bundle injected into all panels
- `PoliciesPanel` — `PolicyFolderTreeView` (folder tree with coverage indicators) + `PolicyEditor` (cascade preview below property editor); folder tree implementation supports Unity 2021.3 LTS through current 6000.x releases via a version-conditional compatibility shim
- `DriftPanel` — scan trigger, async scan with cancel, violation list, per-asset fix, fix-all, CSV export
- `SimulationPanel` — drag-drop asset input, cascade trace table (property / effective value / source policy / source folder)
- `ReimportPanel` — selection mode (folder filter, asset type filter, drifting-only toggle) + execution mode (progress bar, log, size-delta summary); confirmation required before any batch reimport runs
- `BuildGatePanel` — read-only policy-by-enforcement-level gate status
- `AboutPanel` — product identity, documentation and support links, and links to other Tools Studio products

**Inspector and drawers**
- `ImportPolicyEditor` — `[CustomEditor(typeof(ImportPolicy))]`; five foldout sections with live `PolicyValidator` feedback; calls `PolicyGraphCache.MarkDirty()` on modification
- `PolicyPropertyDrawer` — toggle + greyed-out value field; registered for all `PolicyProperty<T>` specializations used in rule sets

**Policy authoring safety**
- `PolicyValidator` — validates `FolderPath` format, folder existence, LOD array length parity, no-duplicate enforcement
- `PolicyGraphConflictNotifier` — `[InitializeOnLoad]` startup dialog when policy graph has conflicts; single notification per editor session; conflicts are resolved deterministically (lexicographically lowest asset path wins) and clearly flagged for cleanup

**Settings service**
- `ConduitProjectSettings` — created automatically at `Assets/Conduit/ConduitProjectSettings.asset` on first access if absent
- `package.json` — valid UPM package manifest

**Demo content**
- Demo texture, audio, and model assets are generated on demand via **Tools › Conduit › Demo › Install Demo Content**, rather than shipped as binary files in the package
- Demo installation and cleanup never touch a folder Conduit doesn't own

### Fixed
- **Build Gate deadlock risk.** The build-time drift scan blocked synchronously on an
  async `Task` (`.GetAwaiter().GetResult()`), which could hang the Editor or a CI build
  on any project large enough for the scan to yield mid-scan. Replaced with a genuinely
  synchronous scan path (`DriftScanner.ScanSync`); interactive scanning is unaffected.
- **Fix / Fix All / Reimport had no confirmation.** These reimport a batch of assets and
  overwrite their current import settings; there was no confirmation step and no way to
  undo. All three now show the affected asset count and require confirmation first.
- **Runtime assembly was not scoped to the Editor platform**, contradicting this
  package's own "Editor-only, no runtime overhead" claim. `ToolsStudio.Conduit.asmdef`
  now targets Editor only.
- **Install Demo Content threw `DirectoryNotFoundException` on a fresh project.** The
  ownership-marker check wrote its marker file before the demo root folder existed.
  The folder is now created first when it's genuinely new; the refusal behavior for
  folders Conduit doesn't own is unchanged.
- **Demo model generation logged import warnings on every install.** The generated
  cube `.obj` files had no vertex normals, so Unity had to recalculate them on every
  import. The generator now writes real per-face normals (flat-shaded, hard-edge cube,
  `v//vn` indexed), verified by an independent winding/normal-direction check. Demo
  install now produces a clean console.
- **`PoliciesPanel.cs` did not compile below Unity 6000.2.** The Policies tab's folder
  tree used Unity's generic `TreeView<int>` API unconditionally; that API doesn't exist
  before Unity 6000.2. Implemented a version-conditional compatibility shim so the same
  code compiles correctly across the full supported range (Unity 2021.3 LTS through
  current 6000.x releases).
- **`AssetDatabase.IsAssetImportWorkerProcess()` was called directly and
  unconditionally.** This API doesn't exist prior to Unity 2022.1, so the direct call
  would fail to compile on 2021.3 LTS — the documented minimum version. Added
  `Editor/Scripts/UnityCompat.cs` with a version-guarded wrapper and updated both call
  sites to use it.
- Feature Request now routes to the same GitHub Issues tracker as Report Issue,
  consistently, in both the About panel and README.
- About panel's license text and product-links section reworded for clarity and
  accuracy — no longer references internal file names or ambiguous section titles.

### Architecture
- [ADR-001] Conduit V1 Initial Architecture accepted

### Performance (measured on reference hardware, 5,000-asset project)

| Operation | Median | P95 |
|---|---|---|
| `PolicyResolver.Resolve()` — warm | < 0.1ms | < 0.3ms |
| `PolicyResolver.Resolve()` — cold | < 2ms | < 5ms |
| `PolicyGraph.Build()` | < 300ms | < 500ms |
| `DriftScanner.ScanAsync()` | < 8s | < 12s |

### Known Limitations

- `VideoRuleSet` is defined but no applicator ships. Video import settings are not governed in 1.0.0.
- LOD generation uses hierarchy cloning only. Mesh decimation requires Simplygon (planned, v1.2.0).
- Per-asset policy exception overrides are not supported. Moving the asset to a different folder is the workaround.

---

## [1.1.0] — TBD (Planned)

*See the [Roadmap](https://github.com/tools-studio/conduit-docs/blob/main/release/roadmap.md) for planned scope.*

### Planned additions
- `VideoPolicyApplicator` — `VideoClipImporter` property application
- `ConduitRulePreset` ScriptableObject — reusable named rule sets
- JSON report schema bumped to `1.1`

---

## [1.2.0] — TBD (Planned)

*See the [Roadmap](https://github.com/tools-studio/conduit-docs/blob/main/release/roadmap.md) for planned scope.*

### Planned additions
- `ConduitAssetOverride` ScriptableObject — per-asset policy exception
- `SimplygonLodAdapter` — mesh decimation via optional Simplygon SDK
- `LodGenerationMode.Simplygon` activated

---

[Unreleased]: https://github.com/afterix-hub/conduit/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/afterix-hub/conduit/releases/tag/v1.0.0
