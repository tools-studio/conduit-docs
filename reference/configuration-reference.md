# Configuration Reference

## Project settings

Conduit creates `Assets/Conduit/ConduitProjectSettings.asset` the first time it runs. Edit it via **Tools Studio › Conduit › Open Window › Settings (gear icon)**.

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

## CIBridge JSON report

`CIBridge.Run()` (see [Build Gate](../docs/build-gate.md)) writes a JSON report to the path given via `-conduitReport`:

```json
{
  "violationCount": 2,
  "violations": [
    {
      "assetPath": "Assets/Textures/UI/icon.png",
      "propertyName": "maxTextureSize",
      "expectedValue": "512",
      "actualValue": "2048",
      "governingPolicyName": "UI Textures"
    }
  ]
}
```

## CIBridge exit codes

| Code | Meaning |
|---|---|
| 0 | No blocking violations. |
| 1 | One or more `Block Build` violations found. See the JSON report. |
| 2 | The scan itself failed to complete. Check the Editor log for the underlying error. |
