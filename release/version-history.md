# Version History

## Unity Compatibility

| Unity Version | Support Status | Notes |
|---|---|---|
| 2021.3 LTS | **Supported — minimum** | Verified via static source audit against Unity's own documented API history. Requires the compatibility shims below. |
| 2022.3 LTS | Supported | No API differences found between 2022.3 and 2021.3 for anything Conduit uses. |
| 2023.x | Supported | No API differences found between 2023.x and 2021.3 for anything Conduit uses. |
| 6000.0 – 6000.1 | Supported | Uses the classic (pre-generic) `TreeView` API branch of the compatibility shim. |
| 6000.2 and newer | Supported | Uses the generic `TreeView<int>` API branch of the compatibility shim. |
| **6000.3.10f1** | **Supported — build-verified** | The exact Editor version recorded in the source repository's `ProjectSettings/ProjectVersion.txt`; the only version this codebase has actually been built with. |
| Other 6000.x releases | Expected to work | No version-conditional compilation exists beyond the two documented shims below. |
| Unity 7.x / future majors | Unknown | Not evaluated. No claim is made either way. |

| Conduit version | Unity requirement | Status |
|---|---|---|
| 1.0.0 | 2021.3 LTS or later | Current |

- **Minimum required:** Unity 2021.3 LTS.
- **Recommended:** the latest Unity 6 LTS release.
- **Build-verified:** Unity 6000.3.10f1 — the only version this codebase has actually been compiled and run against.
- **Render pipeline:** Unaffected. Conduit governs `TextureImporter`, `ModelImporter`, and `AudioImporter` — the asset-import layer — independent of URP, HDRP, or the Built-in pipeline. Conduit's `package.json` declares no package dependencies.

### Compatibility shims

A full static audit — every Unity API Conduit calls, checked against Unity's documented API history — found exactly two APIs that differ between 2021.3 and 6000.x. Both are isolated behind version guards; nothing else in the codebase branches by Unity version.

| API | Issue | Shim |
|---|---|---|
| `TreeView` / `TreeViewItem` / `TreeViewState` (Policies tab folder tree) | Unity 6000.2 added a generic `TreeView<int>` API; 6000.3 made the classic non-generic forms obsolete. Neither version has both available without a compiler error or a deprecation warning. | `PoliciesPanel.cs` type-aliases all three names, via `#if UNITY_6000_2_OR_NEWER`, to whichever API the running Unity version has. The `PolicyFolderTreeView` implementation itself is written once and is identical on every version. |
| `AssetDatabase.IsAssetImportWorkerProcess()` | Ships with Parallel Import, a Unity 2022.1+ feature. Doesn't exist on 2021.3. | `UnityCompat.IsImportWorkerProcess()`, in `Editor/Scripts/UnityCompat.cs`, wraps the real call behind `#if UNITY_2022_1_OR_NEWER` and returns `false` otherwise — correct, since import worker processes don't exist on 2021.3 either. |

Everything else audited — every `TextureImporter`/`ModelImporter`/`AudioImporter` property Conduit reads or writes (including `AudioImporterSampleSettings.preloadAudioData` and the platform-override methods, confirmed via Unity's own issue tracker to have existed since at least 2019.4), `IPreprocessBuildWithReport`, `BuildReport`, `Profiler.GetRuntimeMemorySizeLong`, the async `Task`-based scanning APIs, and every C# language feature in use (including switch expressions, confirmed available by default since Unity 2021.1) — was confirmed to already work unchanged on 2021.3 LTS.

**Audit method and limitations:** every API call site was checked against Unity's documentation, scripting reference, source (`UnityCsReference`), and official release changelogs — including confirming the exact version each shim's boundary is checked against (Unity 6000.2 for the generic `TreeView<T>` API, Unity 2022.1 for `IsAssetImportWorkerProcess()`) directly against Unity's own changelog entries for those releases. This audit was not validated by compiling or running the project in an actual Unity Editor; no Editor instance was available to do so. Before shipping 2021.3 support, run an actual compile-and-smoke-test pass across Policies, Drift, Simulation, Reimport, Build Gate, the Demo installer, and CI Bridge in real 2021.3 LTS, 2022.3 LTS, 2023.x, 6000.0–6000.1, and 6000.2+ Editors — the last two exercise different branches of the `TreeView` shim and should both be checked. The two shims above are the only places version-specific behavior is expected to matter.

Earlier Unity versions (2021.2 and prior) are not supported and have not been audited against this codebase.

## Versioning

Conduit follows [Semantic Versioning](https://semver.org/): a major version bump signals a breaking change, minor versions add functionality without breaking existing policies or scripted API usage, and patch versions are fixes only.

## Upgrading

Minor and patch upgrades within Conduit 1.x require no migration steps — existing `ImportPolicy` assets and project settings continue to work unchanged, since new rule fields are always opt-in by default. A future major version that introduces breaking changes will ship with its own migration guide, linked from [Changelog](https://github.com/afterix-hub/conduit/blob/main/CHANGELOG.md) at that point.
