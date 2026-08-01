# API Reference

Everything Conduit exposes to your own editor scripts lives on the static `ToolsStudio.Conduit.Editor.Conduit` class. It's editor-only — none of it is callable from a player build.

## Policy resolution

**Description.** Resolves the full cascading policy chain for an asset path and returns the merged result — the same resolution used internally for import-time application, drift scanning, and simulation.

```csharp
ResolvedPolicy policy = Conduit.ResolvePolicy("Assets/Textures/UI/icon.png");
```

Also available: `GetAllPolicies()` to list every `ImportPolicy` in the project, and `RebuildPolicyGraph()` to force a rebuild if you've changed policy assets through a script rather than the Editor UI.

**Notes.** See [Core Concepts](features/core-concepts.md) for how the cascade and enforcement levels resolve.

## Simulation

**Description.** Returns the same information the Simulation tab shows: every governed property, its resolved value, and which policy set it. Read-only.

```csharp
SimulationResult result = Conduit.Simulate("Assets/Textures/UI/icon.png");
```

**Notes.** See [Simulation](features/simulation.md).

## Drift scanning

**Description.** Scans for assets whose current import settings don't match their governing policy. Runs asynchronously with the same progress reporting and cancellation the Drift tab uses internally.

```csharp
DriftReport report = await Conduit.ScanForDriftAsync();
DriftReport folderReport = await Conduit.ScanFolderAsync("Assets/Textures");
```

**Parameters.** `ScanFolderAsync` takes a project-relative folder path to scope the scan instead of scanning the whole project.

**Notes.** See [Drift Detection](features/drift-detection.md).

## Reimport

**Description.** Applies policy and reimports the given assets, or every asset present in a `DriftReport`. This is the scripted equivalent of the Reimport tab's Apply Policy + Reimport action — it overwrites current import settings the same way.

```csharp
ReimportResult result = await Conduit.ReimportAssetsAsync(new[] { "Assets/Textures/UI/icon.png" });
ReimportResult result = await Conduit.ReimportDriftingAssetsAsync(report);
```

**Notes.** See [Reimport Workflow](features/reimport-workflow.md).

## Build Gate

**Description.** Runs the same synchronous, non-blocking check Unity's build pipeline runs automatically via `IPreprocessBuildWithReport`. Useful for a custom pre-build script that wants to check gate status without triggering an actual build.

```csharp
GateValidationResult result = Conduit.RunBuildGateCheck();
if (result.WillBlockBuild) { /* ... */ }
```

**Notes.** See [Build Gate](features/build-gate.md) for the interactive tab, and [Configuration](configuration.md) for the CI command-line interface, JSON report schema, and exit codes.

## Settings

**Description.** Reads the project's Conduit settings.

```csharp
ConduitProjectSettings settings = Conduit.GetSettings();
bool enabled = Conduit.IsEnabled;
```

`GetSettings()` returns the project's `ConduitProjectSettings` singleton asset. `IsEnabled` is a shortcut for its `ConduitEnabled` field.

## Return types

`ResolvedPolicy`, `SimulationResult`, `DriftReport`, `ReimportResult`, and `GateValidationResult` are plain data types — no methods, safe to inspect and serialize for your own reporting. See [Configuration](configuration.md) for the JSON shape used by `CIBridge`, which mirrors `DriftReport` and `GateValidationResult` closely.
