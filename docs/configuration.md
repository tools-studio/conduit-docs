# Configuration Reference

## Project settings

Conduit creates `Assets/Conduit/ConduitProjectSettings.asset` the first time it runs. Edit it via **Tools › Conduit › Open Window › Settings (gear icon)**.

| Setting | Default | Purpose |
|---|---|---|
| Conduit Enabled | On | Master switch. When off, Conduit applies nothing at import time — every other feature stays available, but assets import with their own settings untouched. |
| Suppress Build Gate On CI | Off | Skips the build gate check, but only when the `CONDUIT_CI_SKIP` scripting define is also set. Both are required together, so the gate can't be silently suppressed in a normal build. |
| Log Applied Policies | Off | When on, every import-time policy application logs an info-level entry — useful while debugging why an asset ended up with the settings it has. |
| Drift Scanner Batch Size | 100 | Assets processed per iteration before the Drift scan checks its time budget and yields. Range 10–500. Lower this if the Editor feels unresponsive during a scan on a very large project; raise it to finish a scan faster on a project where responsiveness isn't a concern. |
| Excluded Folder Paths | (empty) | Path prefixes Conduit never processes, regardless of policy — for example, `Assets/ThirdParty/`. |

## Scripting defines

| Define | Effect |
|---|---|
| `CONDUIT_CI_SKIP` | Combined with the Suppress Build Gate On CI setting above, skips the build gate. Set this only on the specific CI configuration that runs Conduit's gate check as its own separate step. |
| `CONDUIT_DEBUG` | Enables `[INFO]` and `[WARN]` diagnostic logging in addition to errors. Set via **Edit › Project Settings › Player › Scripting Define Symbols**. |

## Console error messages

| Message | Meaning |
|---|---|
| `[TS][Conduit][ERROR] Scan failed.` | `DriftScanner.ScanAsync()` threw an unhandled exception. Full stack trace follows in the console entry. |
| `[TS][Conduit][ERROR] Gate check failed.` | The build gate's scan task faulted. Full stack trace follows in the console entry. |
| `[TS][Conduit][ERROR] Reimport failed.` | `ReimportCoordinator` threw during a reimport. Check the affected asset's path and write permissions. |

## CIBridge command-line arguments

| Argument | Effect |
|---|---|
| `-conduitReport <path>` | Writes the JSON report to the given path. If omitted, no report file is written. |
| `-conduitFolder <path>` | Scopes the scan to one folder instead of the whole project. |
| `-conduitWarnAsError` | Treats `Warn Only` violations as blocking for this run — exit code `1` is returned if any exist, even though they wouldn't block an interactive build. |

## CIBridge JSON report

`CIBridge.Run()` (see [Build Gate](features/build-gate.md)) writes a JSON report to the path given via `-conduitReport`:

```json
{
  "schemaVersion": "1.0",
  "generatedAt": "2026-06-09T12:00:00.0000000Z",
  "wasCompleted": true,
  "summary": {
    "totalViolations": 2,
    "blockingViolations": 1,
    "warnOnlyViolations": 1
  },
  "violations": [
    {
      "assetPath": "Assets/Textures/UI/icon.png",
      "propertyName": "maxTextureSize",
      "expected": "512",
      "actual": "2048"
    }
  ]
}
```

`summary.blockingViolations` counts violations under a `Block Build` policy; `summary.warnOnlyViolations` counts violations under a `Warn Only` policy. `wasCompleted` is `false` if the scan was interrupted before finishing.

## CIBridge exit codes

| Code | Meaning |
|---|---|
| 0 | No blocking violations (or Conduit is disabled project-wide). |
| 1 | One or more `Block Build` violations found, or `Warn Only` violations found with `-conduitWarnAsError` set. See the JSON report. |
| 2 | The scan itself failed to complete. Check the Editor log for the underlying error. |
