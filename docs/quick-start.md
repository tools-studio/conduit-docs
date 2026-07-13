# Quick Start

This walks through governing one folder, start to finish. It takes about five minutes.

## Option A — use the demo content

If you'd rather see Conduit working before setting up your own policies, run **Tools Studio › Conduit › Demo › Install Demo Content**. This generates a small set of sample assets with policies already applied and intentional drift already present, so **Drift › Scan Project** immediately has something to show you. Nothing is installed until you run this command. See [Demo Workflow](demo-workflow.md) for details.

## Option B — govern your own folder

### 1. Create a policy

Open **Tools Studio › Conduit › Open Window**, go to the **Policies** tab, select a folder in the tree (for example `Assets/Textures`), and click **Add Policy for This Folder**.

### 2. Turn on the rules you care about

In the policy inspector, expand **Texture Rules** and check **Max Texture Size**. Set it to `1024`. Leave every other rule unchecked — an unchecked rule is not enforced and doesn't affect assets in this folder.

### 3. Scan for drift

Go to the **Drift** tab and click **Scan Project**. Any texture under `Assets/Textures` that isn't already 1024 or smaller shows up as a violation, with its current value and the value the policy expects.

### 4. Fix it

Click **Fix** next to a single violation, or **Fix All** to bring every drifting asset into compliance in one pass. Conduit shows a confirmation with the affected asset count before making any change.

### 5. Optional — block builds on violation

Back in the Policies tab, set **Enforcement Level** to `Block Build`. Now a build fails if any asset under this policy is out of compliance, with an error naming the asset, the property, and the expected value. See [Build Gate](build-gate.md).

## What's next

- [Core Concepts](core-concepts.md) — how cascading and enforcement actually work
- [Texture Rules](texture-rules.md), [Audio Rules](audio-rules.md), [Model Rules](model-rules.md) — every rule Conduit supports, per category
