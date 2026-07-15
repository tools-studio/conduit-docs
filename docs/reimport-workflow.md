# Reimport Workflow

The **Reimport** tab applies policy to a chosen set of assets and reimports them, independent of a drift scan.

<p align="center">
  <img src="../images/screenshot-03-reimport.png" width="800" alt="Conduit full and targeted reimport workflow">
</p>

Shows both modes: a full reimport across every governed asset in scope (top), and a targeted reimport limited to only the assets a Drift scan flagged (bottom) — each with a before/after size summary once it runs.

## Scope

Filter by asset type — All, Texture, Model, or Audio — and optionally limit the list to drifting assets only, which requires a Drift scan to have run first. Click **Refresh List** to populate the list under the current filter.

## Selecting assets

Check the assets you want to reimport. The **Apply Policy + Reimport** button shows the selected count and is disabled until at least one asset is selected.

## What happens on execution

For each selected asset, Conduit resolves its governing policy, applies every governed property directly to the importer, and calls `SaveAndReimport()` — the same mechanism Unity's own Inspector uses when you click Apply on an importer. This is a destructive operation: it overwrites the asset's current import settings with whatever the policy specifies, for every governed property, whether or not that property was previously drifting. Conduit shows a confirmation naming the affected asset count before proceeding, since this can't be undone through Conduit itself — use Unity's standard undo/version control workflow if you need to revert.

## Reimport vs. Drift's Fix All

Both apply policy and reimport. Drift's Fix All operates on the current scan's violation list. Reimport lets you choose a broader or narrower set directly — including assets that aren't currently drifting, useful after changing a policy and wanting to force every asset in scope to pick up the new values immediately rather than waiting for the next scan.
