# Policies

A policy governs the import settings of one folder. This page explains what a policy is, how policies combine, and how enforcement works.

The **Policies** tab shows your project's folder tree on the left and the selected folder's policy on the right.

<p align="center">
  <img src="../images/screenshot-01-policies.png" width="800" alt="Conduit policy editor showing folder-scoped audio rules">
</p>

The tree on the left shows policy coverage. The panel on the right toggles individual rules. Each rule is opt-in, and the whole policy carries one enforcement level.

## Purpose

A policy is an Import Policy asset (a Unity ScriptableObject) that declares which import settings are active for a folder, and what value each should have, separately for textures, models, and audio. Create one from **Assets › Create › Tools › Conduit › Import Policy**, or with the **Add Policy for This Folder** button in the Policies tab.

## Workflow

Select any folder and click **Add Policy for This Folder**. This creates a policy asset under `Assets/Conduit/Policies/` targeting that folder. A folder without a policy shows an empty state instead of an inspector.

Selecting a folder that already has a policy shows the full inspector inline. Changes take effect immediately. There is no separate save step, matching how Unity's own Inspector behaves for any ScriptableObject.

## Configuration

### Fields every policy has

| Field | Purpose |
|---|---|
| Folder Path | The folder this policy governs. Set when the policy is created. Edit this field to move the policy's target. |
| Apply to Subfolders | When on, the policy also governs every folder beneath the target. When off, it governs only the target folder itself. |
| Enforcement Level | Disabled, Warn Only, or Block Build. See below. |
| Is Enabled | Turns the whole policy on or off without deleting it. A disabled policy contributes nothing, even if some of its rules are set. |

Below these, three collapsible rule sets hold the governed properties: Texture Rules, Model Rules, and Audio Rules. See [Texture Rules](texture-rules.md), [Model Rules](model-rules.md), and [Audio Rules](audio-rules.md). Video Rules is visible but not enforced in 1.0.0.

### Opt-in rules

Every rule in a policy starts unset. An unset rule is invisible to Conduit: it does not override anything, and it does not show up as a violation. A policy can govern a single property, such as audio load type, without taking a position on everything else. This lets you layer policies without one fighting another over a setting neither cares about.

### Cascading

A policy applies to its folder and, if **Apply to Subfolders** is on, to everything beneath it. When a folder has policies at more than one level of its path, for example a project-wide policy and a more specific one for `Textures/UI`, Conduit builds an inheritance chain from `Assets` down to the asset's folder and merges them. The most specific folder wins per property. A project-wide policy can set a sensible default, and a folder-specific policy can override only the properties that need to differ.

### Enforcement levels

- **Disabled**: the property is evaluated and shown in Drift and Simulation, but has no effect on your build.
- **Warn Only**: a violation is logged as a warning. The build proceeds.
- **Block Build**: a violation prevents the build from starting. See [Build Gate](build-gate.md).

When a folder inherits from several policies, the highest enforcement level among them governs.

### Conflicts

Two policies cannot both target the same folder. If this happens, usually from a merge or a copied policy asset, Conduit shows a warning banner in the Policies tab and a one-time dialog on project load naming how many folders are affected. Resolution is deterministic: policy asset paths are sorted, and the lexicographically lowest path wins for each conflicted folder. The same policy wins every time, on every machine, until you remove or retarget the duplicate.

## Related features

[Drift Detection](drift-detection.md) finds assets that no longer match their policy. [Simulation](simulation.md) shows what a policy would apply to an asset. [Reimport Workflow](reimport-workflow.md) applies policy to chosen assets.
