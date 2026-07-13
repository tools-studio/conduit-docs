# Policies

The **Policies** tab shows your project's folder tree on the left and the selected folder's policy on the right.

## Creating a policy

Select any folder and click **Add Policy for This Folder**. This creates an `ImportPolicy` asset under `Assets/Conduit/Policies/` targeting that folder. A folder without a policy shows an empty state instead of an inspector.

## Fields every policy has

| Field | Purpose |
|---|---|
| Folder Path | The folder this policy governs. Set when the policy is created; move the asset's target by editing this field. |
| Apply to Subfolders | When on, the policy also governs every folder beneath the target. When off, it governs only the target folder itself. |
| Enforcement Level | `Disabled`, `Warn Only`, or `Block Build`. See [Core Concepts](core-concepts.md). |
| Is Enabled | Turns the whole policy on or off without deleting it. A disabled policy contributes nothing to resolution, even if some of its rules are set. |

Below these, three collapsible rule sets — Texture Rules, Model Rules, Audio Rules — hold the actual governed properties. See [Texture Rules](texture-rules.md), [Model Rules](model-rules.md), and [Audio Rules](audio-rules.md) for what each one controls. Video Rules is visible but not yet enforced — see [Limitations](../reference/limitations.md).

## Conflicts

Two policies cannot both target the same folder. If this happens — usually from a merge or a copy-pasted policy asset — Conduit shows a warning banner in the Policies tab and a one-time dialog on project load naming how many folders are affected. Conduit uses the first policy found for each conflicted folder until you remove the duplicate; behavior is not guaranteed to be consistent between sessions while a conflict exists.

## Editing a policy

Selecting a folder with a policy already assigned shows that policy's full inspector inline, in the right-hand column. Changes take effect immediately — there's no separate save step, matching how Unity's own Inspector behaves for any ScriptableObject.
