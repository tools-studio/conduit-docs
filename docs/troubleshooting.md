# Troubleshooting

## A build fails with "Build blocked: N policy violations found"

**Cause.** The Build Gate is working as configured. See [Build Gate](features/build-gate.md).

**Solution.** Open the Drift tab, scan, and fix the listed violations. If blocking was not intended for the policy, lower its enforcement level.

## "Conduit — Policy Conflict" appears on project load

**Cause.** Two or more policies target the same folder. Conduit resolves this deterministically, so the policy with the lexicographically lowest asset path wins consistently across sessions and machines, but the duplicate is still worth cleaning up.

**Solution.** Open the Policies tab, find the conflicted folder, and delete or retarget the duplicate policy.

## An asset is not picking up policy changes

**Cause.** Conduit applies policy at import time and on an explicit Reimport or Fix action. It does not continuously watch already-imported assets for policy changes.

**Solution.** After editing a policy, run a drift scan to see which assets are now out of compliance, then fix them. Or use the Reimport tab to force affected assets to pick up the new values immediately.

## The drift scan shows an asset as ungoverned that I expect to be governed

**Cause.** An asset with no applicable rule set anywhere in its inheritance chain is ungoverned by definition.

**Solution.** Check that a policy targets an ancestor folder of the asset with **Apply to Subfolders** on, that the policy is enabled, and that at least one rule relevant to the asset's category is checked.

## Install Demo Content does nothing, or reports an error about an existing folder

**Cause.** Conduit will not touch `Assets/Conduit/Demo/` if that folder already exists with content it did not create.

**Solution.** Rename or remove the conflicting folder and run **Install Demo Content** again. See [Demo Workflow](features/demo-workflow.md).

## The Conduit menu or window does not appear after import

**Cause.** The import may have been incomplete, or the Editor may still be compiling.

**Solution.** Wait for compilation to finish and check the Console for errors. Confirm that `Assets/Conduit/` contains the Runtime and Editor folders, then reimport Conduit from **Window › Package Manager › My Assets** if it does not. Conduit has been verified on Unity 6000.3.10f1 only. If you are on another version, include that in your report below.

## Still stuck?

Check the [FAQ](faq.md), then contact support.

When you report a problem, include:

- Your Conduit version and Unity version. The **Copy Diagnostic Info** button on the About tab copies both.
- The steps that reproduce the problem.
- What you expected to happen and what happened instead.
- Any relevant Console output.

[![Support](https://img.shields.io/badge/Support-24292f?style=for-the-badge)](https://discord.gg/tF4NSVkW6U) [![Discord](https://img.shields.io/badge/Discord-24292f?style=for-the-badge)](https://discord.gg/uaHe32VsyN)

[![Report Issue](https://img.shields.io/badge/Report%20Issue-24292f?style=for-the-badge)](https://discord.gg/XPMGcdnpmn) [![Feature Request](https://img.shields.io/badge/Feature%20Request-24292f?style=for-the-badge)](https://discord.gg/Ge99xqt5qr) [![Email Support](https://img.shields.io/badge/Email%20Support-24292f?style=for-the-badge)](mailto:support.toolsstudio@gmail.com)
