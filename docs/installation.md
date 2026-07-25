# Installation

## Requirements

- Unity 2021.3 LTS or later, through Unity 6000.x (build-verified against 6000.3.10f1)
- No additional packages required

See [Unity Compatibility](../release/version-history.md#unity-compatibility) for the full verified version matrix.

## Unity Package Manager (Git URL)

Open **Window › Package Manager**, click **+**, select **Add package from git URL**, and enter:

    https://github.com/afterix-hub/conduit.git?path=/Assets/Conduit

The `path` query is required — Conduit's `package.json` lives at `Assets/Conduit` within the repository, not at its root.

## Asset Store

A Unity Asset Store listing is planned but not yet live — see [Roadmap](../release/roadmap.md). Once it ships, Conduit will be importable from the Package Manager's My Assets tab like any other Asset Store package.

## Verifying the install

1. Confirm **Tools Studio › Conduit** appears in the menu bar.
2. Open **Tools Studio › Conduit › Open Window**. The Conduit window opens with six tabs: Policies, Drift, Simulation, Reimport, Build Gate, About.
3. Conduit creates `Assets/Conduit/ConduitProjectSettings.asset` the first time it runs. This is expected — it stores your project-level settings.

Conduit is editor-only. It adds nothing to your player builds.

## Next step

[Quick Start](quick-start.md) — create your first policy.
