# Drift Detection

The **Drift** tab scans your project and reports every governed asset whose current import settings do not match its policy.

<p align="center">
  <img src="../images/screenshot-02-drift-and-simulation.png" width="800" alt="Conduit drift scan results and import simulation trace">
</p>

The Drift tab after a scan: each row is one property violation, listing the asset, the property, its expected value, its actual value, and the policy responsible, with a one-click **Fix** beside each one.

## Purpose

Drift is the gap between what a policy expects and what an asset's importer currently has. It happens when someone edits import settings by hand after the asset was imported, when a policy changes but the asset is not reimported, or when assets are added while Conduit is disabled.

## Workflow

### Running a scan

Click **Scan Project**. Conduit checks every texture, model, and audio asset under a policy, works out the expected value for each governed property, and compares it with the asset's actual import settings.

Assets with no governing policy are counted separately as ungoverned, not as violations. An asset Conduit does not govern cannot be out of compliance with anything.

### Reading the results

Each row is one property violation. A single asset with three misconfigured properties produces three rows. The status line above the list shows the total violation count and the number of distinct assets affected.

### Fixing violations

Click **Fix** on a single row to bring that one property back into line, or **Fix All** to resolve every violation in the list in one pass. Both show a confirmation naming the affected asset count before making any change, because fixing overwrites the asset's current import settings. See [Reimport Workflow](reimport-workflow.md).

## Configuration

The scan processes assets in batches. See Drift Scanner Batch Size and Excluded Folder Paths in [Configuration](../configuration.md).

## Performance

Scanning is asynchronous and yields periodically so the Editor stays responsive. Very large projects, with several thousand governed assets, may take some seconds. The status line updates as the scan progresses, and you can cancel mid-scan.
