# Installation

## Requirements

- Unity 2021.3 LTS or later, through Unity 6000.x (build-verified against 6000.3.10f1)
- No additional packages required
- Editor-only — Conduit adds nothing to your player builds

Render pipeline is not a factor: Conduit governs `TextureImporter`, `ModelImporter`, and `AudioImporter` — the asset-import layer — independent of URP, HDRP, or the Built-in pipeline.

## Installing from the Unity Asset Store

Conduit is distributed exclusively through the [Unity Asset Store](https://assetstore.unity.com/publishers/139782). There is no Git URL or Package Manager registry install path.

1. Purchase or acquire Conduit from its Asset Store listing.
2. In Unity, open **Window › Package Manager**, switch to the **My Assets** tab, find Conduit, and click **Download**, then **Import**.
3. In the Import dialog, leave every item checked and click **Import**.

Conduit ships as compiled assemblies (`ToolsStudio.Conduit.dll` and `ToolsStudio.Conduit.Editor.dll`), not editable `.cs` source — see [Licensing & Source Availability](faq.md#does-conduit-include-source-code) in the FAQ for what that means and how to request source-level access.

## Verifying the install

1. Confirm **Tools Studio › Conduit** appears in the menu bar.
2. Open **Tools Studio › Conduit › Open Window**. The Conduit window opens with six tabs: Policies, Drift, Simulation, Reimport, Build Gate, About.
3. Conduit creates `Assets/Conduit/ConduitProjectSettings.asset` the first time it runs. This is expected — it stores your project-level settings.

If the menu item doesn't appear or the window fails to open, see [Troubleshooting](troubleshooting.md#conduit-doesnt-appear-in-the-menu-bar-after-import).

## Next step

[Getting Started](getting-started.md) — create your first policy.
