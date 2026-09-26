# Getting Started

This page takes you from a fresh install to your first governed folder in about five minutes.

## Requirements

- Unity 6000.3.10f1 is the verified Editor version. Other versions have not been verified. See [Installation](installation.md).
- No additional packages are required.
- A Conduit purchase from the Unity Asset Store.

## Installation

Import Conduit from the Package Manager's My Assets tab. See [Installation](installation.md) for the steps.

## How Conduit fits into your workflow

1. You create a policy targeting a folder, for example `Assets/Textures/UI`.
2. You turn on the rules that matter for that folder, such as maximum texture size or compression.
3. New assets imported into that folder get those settings automatically.
4. Existing assets that do not match are flagged by a drift scan, and you fix them in one click.
5. If you enable build-blocking enforcement, a non-compliant asset stops the build until it is fixed.

## Your first workflow

### Option A: use the demo content

To see Conduit working before setting up your own policies, run **Tools › Conduit › Demo › Install Demo Content**. It generates a small set of sample assets with policies already applied and intentional drift already present, so **Drift › Scan Project** has something to show you immediately. Nothing is installed until you run this command. See [Demo Workflow](features/demo-workflow.md).

### Option B: govern your own folder

1. **Create a policy.** Open **Tools › Conduit › Open Window**, go to the **Policies** tab, select a folder in the tree (for example `Assets/Textures`), and click **Add Policy for This Folder**.
2. **Turn on the rules you care about.** In the policy inspector, expand **Texture Rules** and check **Max Texture Size**. Set it to `1024`. Leave every other rule unchecked. An unchecked rule is not enforced and does not affect assets in this folder.
3. **Scan for drift.** Go to the **Drift** tab and click **Scan Project**. Any texture under `Assets/Textures` that is not already 1024 or smaller appears as a violation, with its current value and the value the policy expects.
4. **Fix it.** Click **Fix** next to a single violation, or **Fix All** to bring every drifting asset into compliance in one pass. Conduit shows a confirmation with the affected asset count before making any change.
5. **Optional: block builds on violation.** In the Policies tab, set **Enforcement Level** to `Block Build`. A build now fails if any asset under this policy is out of compliance, with an error naming the asset, the property, and the expected value. See [Build Gate](features/build-gate.md).

## Next steps

- [Policies](features/policies.md): policies, cascading, and enforcement levels
- [Texture Rules](features/texture-rules.md), [Audio Rules](features/audio-rules.md), and [Model Rules](features/model-rules.md): every rule Conduit supports
- [Troubleshooting](troubleshooting.md)
