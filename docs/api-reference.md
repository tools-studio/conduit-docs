# API Reference

Everything Conduit exposes to your own Editor scripts lives on the static `ToolsStudio.Conduit.Editor.Conduit` class. It is Editor-only. None of it is callable from a player build.

Add the namespace to your Editor script:

```csharp
using ToolsStudio.Conduit.Editor;
```

## ResolvePolicy

Resolves the full cascading policy chain for an asset path and returns the merged result. This is the same resolution used for import-time application, drift scanning, and simulation.

```csharp
public static ResolvedPolicy ResolvePolicy(string assetPath)
```

**Parameters**

- `assetPath`: the asset path, for example `Assets/Textures/UI/icon.png`. Must not be empty.

**Returns** a `ResolvedPolicy`.

**Example**

```csharp
ResolvedPolicy policy = Conduit.ResolvePolicy("Assets/Textures/UI/icon.png");
```

**Notes** Throws `ArgumentException` if `assetPath` is null, empty, or whitespace.

## GetAllPolicies

Lists every policy asset in the project.

```csharp
public static IReadOnlyList<ImportPolicy> GetAllPolicies()
```

**Returns** a read-only list of `ImportPolicy` assets.

## RebuildPolicyGraph

Forces Conduit to rebuild its view of the project's policies. Call it if you changed policy assets from a script rather than through the Editor interface.

```csharp
public static void RebuildPolicyGraph()
```

**Notes** The rebuild runs synchronously on the calling thread.

## Simulate

Returns what the Simulation tab shows: every governed property, its resolved value, and which policy set it. Read-only. See [Simulation](features/simulation.md).

```csharp
public static SimulationResult Simulate(string assetPath)
```

**Parameters**

- `assetPath`: the asset path to simulate. Must not be empty.

**Returns** a `SimulationResult`.

**Example**

```csharp
SimulationResult result = Conduit.Simulate("Assets/Textures/UI/icon.png");
```

**Notes** Throws `ArgumentException` if `assetPath` is null, empty, or whitespace.

## ScanForDriftAsync

Scans the whole project for drift.

```csharp
public static Task<DriftReport> ScanForDriftAsync(
    IProgress<DriftScanProgress> progress = null,
    CancellationToken cancellation = default)
```

**Parameters**

- `progress`: optional progress reporting, the same the Drift tab uses.
- `cancellation`: optional cancellation token.

**Returns** a task that produces a `DriftReport`.

**Example**

```csharp
DriftReport report = await Conduit.ScanForDriftAsync();
```

## ScanFolderAsync

Scans a single folder for drift instead of the whole project.

```csharp
public static Task<DriftReport> ScanFolderAsync(
    string folderPath,
    IProgress<DriftScanProgress> progress = null,
    CancellationToken cancellation = default)
```

**Parameters**

- `folderPath`: the folder to scan, for example `Assets/Textures`. Must not be empty.
- `progress`: optional progress reporting.
- `cancellation`: optional cancellation token.

**Returns** a task that produces a `DriftReport`.

**Example**

```csharp
DriftReport folderReport = await Conduit.ScanFolderAsync("Assets/Textures");
```

**Notes** Throws `ArgumentException` if `folderPath` is null, empty, or whitespace.

## ReimportAssetsAsync

Applies policy to the given assets and reimports them. This is the scripted equivalent of the Reimport tab's Apply Policy + Reimport action, and it overwrites current import settings the same way. See [Reimport Workflow](features/reimport-workflow.md).

```csharp
public static Task<ReimportResult> ReimportAssetsAsync(
    IReadOnlyList<string> assetPaths,
    string jobLabel,
    IProgress<ReimportProgress> progress = null,
    CancellationToken cancellation = default)
```

**Parameters**

- `assetPaths`: the asset paths to reimport. Must not be null or empty.
- `jobLabel`: a label for this reimport job.
- `progress`: optional progress reporting.
- `cancellation`: optional cancellation token.

**Returns** a task that produces a `ReimportResult`.

**Example**

```csharp
ReimportResult result = await Conduit.ReimportAssetsAsync(
    new[] { "Assets/Textures/UI/icon.png" }, "MyReimport");
```

**Notes** Throws `ArgumentNullException` if `assetPaths` is null, and `ArgumentException` if it is empty.

## ReimportDriftingAssetsAsync

Applies policy to, and reimports, every asset that appears in a `DriftReport`. An asset with several violations is reimported once.

```csharp
public static Task<ReimportResult> ReimportDriftingAssetsAsync(
    DriftReport driftReport,
    IProgress<ReimportProgress> progress = null,
    CancellationToken cancellation = default)
```

**Parameters**

- `driftReport`: a report returned by one of the scan methods. Must not be null.
- `progress`: optional progress reporting.
- `cancellation`: optional cancellation token.

**Returns** a task that produces a `ReimportResult`. If the report has no violations, the result is empty.

**Example**

```csharp
ReimportResult result = await Conduit.ReimportDriftingAssetsAsync(report);
```

## RunBuildGateCheck

Runs the same synchronous check the build gate runs automatically before a build. Use it in a custom pre-build script to check gate status without starting a build. See [Build Gate](features/build-gate.md).

```csharp
public static GateValidationResult RunBuildGateCheck()
```

**Returns** a `GateValidationResult`. Its `WillBlockBuild` property is `true` when at least one Block Build violation exists.

**Example**

```csharp
GateValidationResult result = Conduit.RunBuildGateCheck();
if (result.WillBlockBuild) { /* handle */ }
```

## GetSettings

Returns the project's Conduit settings asset.

```csharp
public static ConduitProjectSettings GetSettings()
```

## IsEnabled

A shortcut for the Conduit Enabled setting.

```csharp
public static bool IsEnabled { get; }
```

**Example**

```csharp
bool enabled = Conduit.IsEnabled;
```

## Return types

`ResolvedPolicy`, `SimulationResult`, `DriftReport`, `ReimportResult`, and `GateValidationResult` are plain result objects that you can inspect and use in your own reporting. The JSON report written by the command-line runner closely mirrors the drift report and gate result. See [Configuration](configuration.md).
