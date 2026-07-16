# Troubleshooting

## A build fails with "Build blocked: N policy violations found"

This is the Build Gate working as configured — see [Build Gate](build-gate.md). Open the Drift tab, scan, and fix the listed violations, or lower the relevant policy's enforcement level if blocking wasn't intended for it.

## "Conduit — Policy Conflict" appears on project load

Two or more `ImportPolicy` assets target the same folder. Conduit resolves this deterministically — the policy with the lexicographically lowest asset path wins, consistently across sessions and machines — but it's still worth cleaning up. Open the Policies tab, find the conflicted folder, and delete or retarget the duplicate policy.

## An asset isn't picking up policy changes

Conduit applies policy at import time and on an explicit Reimport or Fix action — it does not continuously watch already-imported assets for policy changes. After editing a policy, run a Drift scan to see which assets are now out of compliance, then Fix them, or use the Reimport tab to force affected assets to pick up the new values immediately.

## Drift scan shows an asset as ungoverned that I expect to be governed

Check that a policy actually targets an ancestor folder of the asset with **Apply to Subfolders** on, that the policy is enabled, and that at least one rule relevant to that asset's category is checked. An asset with zero applicable rules set anywhere in its inheritance chain is ungoverned by definition, not a bug.

## Install Demo Content does nothing, or reports an error about an existing folder

Conduit won't touch `Assets/Conduit/Demo/` if that folder already exists with content it didn't create. Rename or remove the conflicting folder and run **Install Demo Content** again. See [Demo Workflow](demo-workflow.md).

## Still stuck

Check [FAQ](faq.md), then see [Report Issues](../support/report-issues.md).
