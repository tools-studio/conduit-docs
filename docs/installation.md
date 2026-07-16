# Installation

## Requirements

- Unity 2021.3 LTS or later, through Unity 6000.x (build-verified against 6000.3.10f1)
- No additional packages required

See [Unity Compatibility](../release/version-history.md#unity-compatibility) for the full verified version matrix.

## Asset Store

Import Conduit from the [Asset Store](https://assetstore.unity.com/publishers/139782) via the Package Manager's My Assets tab.

## Unity Package Manager (Git URL)

Open **Window › Package Manager**, click **+**, select **Add package from git URL**, and enter:

    https://github.com/Afterix-Hub/conduit.git

## Verifying the install

1. Confirm **Tools Studio › Conduit** appears in the menu bar.
2. Open **Tools Studio › Conduit › Open Window**. The Conduit window opens with six tabs: Policies, Drift, Simulation, Reimport, Build Gate, About.
3. Conduit creates `Assets/Conduit/ConduitProjectSettings.asset` the first time it runs. This is expected — it stores your project-level settings.

Conduit is editor-only. It adds nothing to your player builds.

## Next step

[Quick Start](quick-start.md) — create your first policy.
