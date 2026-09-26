# Installation

Conduit is a paid product distributed through the Unity Asset Store. Install it from your Unity account's purchased assets.

## Requirements

- A Conduit purchase on the Unity Asset Store, made with the Unity account you use in the Editor
- Unity 6000.3.10f1 (verified). See Compatibility below.
- No additional packages

## Install from the Asset Store

1. In Unity, sign in with the Unity account that purchased Conduit.
2. Open **Window › Package Manager**.
3. Set the package source to **My Assets**.
4. Select **Conduit** and click **Download**.
5. When the download finishes, click **Import**.
6. In the import dialog, leave every item selected and click **Import**.

Conduit installs into `Assets/Conduit/`. The package contains the Conduit Runtime and Editor C# source, offline documentation, a README, and a changelog.

## Verify the install

1. Confirm that **Tools › Conduit** appears in the menu bar.
2. Open **Tools › Conduit › Open Window**. The Conduit window opens with six tabs: Policies, Drift, Simulation, Reimport, Build Gate, and About.
3. Conduit creates `Assets/Conduit/ConduitProjectSettings.asset` the first time it runs. This is expected. It stores your project-level settings.

Conduit is Editor-only. It adds nothing to your player builds.

## Compatibility

| Item | Status |
|---|---|
| Unity Editor | Verified on 6000.3.10f1 |
| Other Unity versions | Not verified. Additional Editor verification is required before compatibility is claimed. |
| Render pipelines | Not separately verified |
| Player platforms | Not applicable. Conduit is Editor-only and adds nothing to player builds. |
| Editor host operating systems | Not separately verified |

Conduit is version 1.0.0. If you use a Unity version other than the verified one, treat the result as untested and report what you find through the support channels below.

## Updating

Updates are delivered through the Unity Asset Store. Open **Window › Package Manager**, choose **My Assets**, and update Conduit when a newer version is listed. Your policies and settings live in your own project under `Assets/Conduit/`.

## Getting help

[![Support](https://img.shields.io/badge/Support-24292f?style=for-the-badge)](https://discord.gg/tF4NSVkW6U) [![Discord](https://img.shields.io/badge/Discord-24292f?style=for-the-badge)](https://discord.gg/uaHe32VsyN)

[![Report Issue](https://img.shields.io/badge/Report%20Issue-24292f?style=for-the-badge)](https://discord.gg/XPMGcdnpmn) [![Feature Request](https://img.shields.io/badge/Feature%20Request-24292f?style=for-the-badge)](https://discord.gg/Ge99xqt5qr) [![Email Support](https://img.shields.io/badge/Email%20Support-24292f?style=for-the-badge)](mailto:support.toolsstudio@gmail.com)

## Next step

[Getting Started](getting-started.md): create your first policy.
