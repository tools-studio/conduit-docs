# API Overview

Everything Conduit exposes to your own editor scripts lives on the static `ToolsStudio.Conduit.Editor.Conduit` class. It's editor-only — none of it is callable from a player build.

## Policy resolution

```csharp
ResolvedPolicy policy = Conduit.ResolvePolicy("Assets/Textures/UI/icon.png");
```

Resolves the full cascading policy chain for a path and returns the merged result — the same resolution used internally for import-time application, drift scanning, and simulation. Also available: `GetAllPolicies()` to list every `ImportPolicy` in the project, and `RebuildPolicyGraph()` to force a rebuild if you've changed policy assets through a script rather than the Editor UI.

## Simulation

```csharp
SimulationResult result = Conduit.Simulate("Assets/Textures/UI/icon.png");
```

Returns the same information the Simulation tab shows: every governed property, its resolved value, and which policy set it. Read-only — see [Simulation](../docs/simulation.md).

## Drift scanning

```csharp
DriftReport report = await Conduit.ScanForDriftAsync();
DriftReport folderReport = await Conduit.ScanFolderAsync("Assets/Textures");
```

Both run asynchronously and support the same progress reporting and cancellation the Drift tab uses internally. Use `ScanFolderAsync` to scope a scan instead of scanning the whole project.

## Reimport

```csharp
ReimportResult result = await Conduit.ReimportAssetsAsync(new[] { "Assets/Textures/UI/icon.png" });
ReimportResult result = await Conduit.ReimportDriftingAssetsAsync(report);
```

Applies policy and reimports the given assets, or every asset present in a `DriftReport`. This is the scripted equivalent of the Reimport tab's Apply Policy + Reimport action — it overwrites current import settings the same way. See [Reimport Workflow](../docs/reimport-workflow.md).

## Build Gate

```csharp
GateValidationResult result = Conduit.RunBuildGateCheck();
if (result.WillBlockBuild) { /* ... */ }
```

Runs the same synchronous, non-blocking check Unity's build pipeline runs automatically via `IPreprocessBuildWithReport`. Useful for a custom pre-build script that wants to check gate status without triggering an actual build. See [Build Gate](../docs/build-gate.md).

## Settings

```csharp
ConduitProjectSettings settings = Conduit.GetSettings();
bool enabled = Conduit.IsEnabled;
```

`GetSettings()` returns the project's `ConduitProjectSettings` singleton asset. `IsEnabled` is a shortcut for its `ConduitEnabled` field.

## Return types

`ResolvedPolicy`, `SimulationResult`, `DriftReport`, `ReimportResult`, and `GateValidationResult` are plain data types — no methods, safe to inspect and serialize for your own reporting. See [Configuration Reference](configuration-reference.md) for the JSON shape used by `CIBridge`, which mirrors `DriftReport` and `GateValidationResult` closely.
