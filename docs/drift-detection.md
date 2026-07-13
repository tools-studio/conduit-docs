# Drift Detection

The **Drift** tab scans your project and reports every governed asset whose current import settings don't match its policy.

## Running a scan

Click **Scan Project**. Conduit walks every texture, model, and audio asset under a policy, resolves the expected value for each governed property, and compares it against the asset's actual importer settings. The result is a list of violations, each showing the asset, the property, the expected value, the actual value, and which policy set the expectation.

Assets with no governing policy are counted separately as ungoverned, not as violations — an asset Conduit doesn't govern can't be out of compliance with anything.

## Reading the results

Each row is one property violation. A single asset with three misconfigured properties produces three rows. The status line above the list shows the total violation count and the number of distinct assets affected.

## Fixing violations

Click **Fix** on a single row to bring that one property back into line, or **Fix All** to resolve every violation in the list in one pass. Both show a confirmation naming the affected asset count before making any change, since fixing overwrites the asset's current import settings — see [Reimport Workflow](reimport-workflow.md) for what happens during a fix.

## Performance

Scanning is asynchronous and yields periodically so the Editor stays responsive during a large scan. Very large projects (several thousand governed assets) may take some seconds; the status line updates as the scan progresses, and you can cancel mid-scan.
