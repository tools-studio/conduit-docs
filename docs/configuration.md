# Configuration

## Project settings

Conduit creates `Assets/Conduit/ConduitProjectSettings.asset` the first time it runs. Edit it from **Tools › Conduit › Open Window** using the settings (gear) icon.

| Setting | Default | Purpose |
|---|---|---|
| Conduit Enabled | On | Master switch. When off, Conduit applies nothing at import time. Every other feature stays available, but assets import with their own settings untouched. |
| Suppress Build Gate On CI | Off | Skips the build gate check, but only when the `CONDUIT_CI_SKIP` scripting define is also set. Both are required together, so the gate cannot be silently suppressed in a normal build. |
| Log Applied Policies | Off | When on, every import-time policy application logs an info-level entry. Useful while debugging why an asset ended up with the settings it has. |
| Drift Scanner Batch Size | 100 | Assets processed per iteration before the drift scan checks its time budget and yields. Range 10 to 500. Lower it if the Editor feels unresponsive during a scan on a very large project. Raise it to finish a scan faster when responsiveness is not a concern. |
| Excluded Folder Paths | (empty) | Path prefixes Conduit never processes, regardless of policy. For example, `Assets/ThirdParty/`. |

## Scripting defines

| Define | Effect |
|---|---|
| `CONDUIT_CI_SKIP` | Combined with the Suppress Build Gate On CI setting, skips the build gate. Set it only on the specific CI configuration that runs Conduit's gate check as its own separate step. |
| `CONDUIT_DEBUG` | Enables `[INFO]` and `[WARN]` diagnostic logging in addition to errors. Set it in **Edit › Project Settings › Player › Scripting Define Symbols**. |

## Console error messages

| Message | Meaning |
|---|---|
| `[TS][Conduit][ERROR] Scan failed.` | The drift scan hit an unhandled error. The full stack trace follows in the Console entry. |
| `[TS][Conduit][ERROR] Gate check failed.` | The build gate's scan failed. The full stack trace follows in the Console entry. |
| `[TS][Conduit][ERROR] Reimport failed.` | An error occurred during a reimport. Check the affected asset's path and write permissions. |

## Command-line arguments (CI)

Run the gate check in batch mode with `-executeMethod ToolsStudio.Conduit.Editor.CIBridge.Run`. See [Build Gate](features/build-gate.md) for a full example command.

| Argument | Effect |
|---|---|
| `-conduitReport <path>` | Writes the JSON report to the given path. If omitted, no report file is written. |
| `-conduitFolder <path>` | Scopes the scan to one folder instead of the whole project. |
| `-conduitWarnAsError` | Treats Warn Only violations as blocking for this run. Exit code `1` is returned if any exist, even though they would not block an interactive build. |

## JSON report

The command writes a JSON report to the path given with `-conduitReport`:

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

`summary.blockingViolations` counts violations under a Block Build policy. `summary.warnOnlyViolations` counts violations under a Warn Only policy. `wasCompleted` is `false` if the scan was interrupted before finishing.

## Exit codes

| Code | Meaning |
|---|---|
| 0 | No blocking violations, or Conduit is disabled project-wide. |
| 1 | One or more Block Build violations were found, or Warn Only violations were found with `-conduitWarnAsError` set. See the JSON report. |
| 2 | The scan itself failed to complete. Check the Editor log for the underlying error. |
