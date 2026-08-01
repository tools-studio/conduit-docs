# Core Concepts

## Policy

An `ImportPolicy` is a Unity ScriptableObject that governs one folder. It declares which import settings are active for that folder — and what value each one should have — separately for textures, models, and audio. Create one via **Assets › Create › Tools Studio › Conduit › Import Policy**, or through the Policies tab's **Add Policy for This Folder** button.

## Opt-in rules

Every rule in a policy starts unset. An unset rule is invisible to Conduit — it doesn't override anything, and it doesn't show up as a violation. A policy can govern a single property, like audio load type, without taking a position on everything else. This is what lets you layer policies without one policy fighting another over a setting neither cares about.

## Cascading

Policies apply to a folder and, if **Apply to Subfolders** is on, to everything beneath it. When a folder has policies at more than one level in its path — a project-wide policy and a more specific one for `Textures/UI`, say — Conduit builds an inheritance chain from `Assets` down to the asset's folder and merges them, with the most specific folder winning per property. A project-wide policy can set a sensible default; a folder-specific policy can override just the properties that need to differ there.

## Enforcement level

Each policy has one of three enforcement levels:

- **Disabled** — the property is evaluated and shown in Drift and Simulation, but has no effect on your build.
- **Warn Only** — a violation is logged as a warning. The build proceeds.
- **Block Build** — a violation prevents the build from starting. See [Build Gate](build-gate.md).

When a folder inherits from multiple policies, the highest enforcement level among them governs.

## Drift

Drift is the gap between what a policy expects and what an asset's importer currently has. It happens when someone edits import settings by hand after the asset was already imported, when a policy changes but the asset isn't reimported, or when assets are added while Conduit is inactive. [Drift Detection](drift-detection.md) covers scanning for it; [Reimport Workflow](reimport-workflow.md) covers fixing it.

## Simulation

Simulation answers "what would Conduit apply to this asset right now, and which policy set each value?" without touching the asset or the AssetDatabase. Useful for understanding why a specific asset ends up with the settings it has when several policies apply to it. See [Simulation](simulation.md).

## Why one resolver

Texture, model, and audio import all need the same answer to the same question — what does policy say for this asset, right now? Import-time application, drift scanning, simulation, and the build gate all read from the same resolution engine rather than each re-implementing folder cascading on its own. This is why Simulation's preview and Drift's violation report always agree with each other, and with what actually gets applied on import — there's one source of truth for what a policy resolves to, not four separate ones that could drift apart from each other.

## Editor-only by design

Conduit's runtime assembly holds policy data only — `ImportPolicy`, the rule sets, project settings — with no import logic in it. Everything that reads or writes an asset importer lives in the Editor assembly, which Unity excludes from player builds entirely. This is why Conduit adds nothing to a build's size or runtime behavior regardless of how many policies a project defines.
