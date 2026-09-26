# FAQ

### Is Conduit free?

No. Conduit is a paid, proprietary Unity Editor tool sold through the Unity Asset Store.

### Which license applies to Conduit?

Conduit is licensed through the Unity Asset Store under the Unity Asset Store EULA.

### Is the source code included?

Yes. The Asset Store package includes the Conduit Runtime and Editor C# source, exactly as it ships in the product. The separate GitHub repository used for Conduit's own development is private; access to it is provided separately to authorized users and is not needed to use or read the source that ships in the package.

### Which Unity versions are supported?

Conduit is verified on Unity 6000.3.10f1. Other versions have not been verified, and additional Editor verification is required before compatibility with them is claimed. See [Installation](installation.md).

### Does Conduit change my project automatically as soon as I install it?

No. Conduit does nothing until you create a policy. Even the demo content is opt-in. See [Demo Workflow](features/demo-workflow.md).

### Does Conduit add anything to my player build?

No. Conduit is Editor-only. Its runtime assembly holds only data definitions (policy and rule types), and none of it compiles into a player build.

### What happens to an asset if I disable a policy?

Its import settings stay exactly as they were the last time Conduit applied them. Disabling a policy stops Conduit from enforcing or checking those properties going forward. It does not revert anything.

### Can I have different rules for iOS and Android?

For textures and audio, yes. See the per-platform overrides sections in [Texture Rules](features/texture-rules.md) and [Audio Rules](features/audio-rules.md). Model Rules do not support per-platform overrides.

### Can I exclude one specific asset from a policy that governs its folder?

Not directly. The current workaround is moving the asset to a folder the policy does not cover, or narrowing the policy's folder scope. Per-asset exceptions are planned for a future release.

### Does Conduit work with version control?

Yes. Policies are ordinary Unity assets and commit like any other. Conduit does not hold any state outside your project, other than per-user Editor preferences for a couple of one-time interface flags, which are not meant to be shared across a team.

### Does scanning a large project block the Editor?

No. Drift scans run asynchronously and yield periodically, so the Editor stays responsive. The Build Gate's check, which runs at build time, is synchronous by design. See [Build Gate](features/build-gate.md).

### How do I update Conduit?

Updates are delivered through the Unity Asset Store. See [Installation](installation.md).

### Where do I get help?

[![Support](https://img.shields.io/badge/Support-24292f?style=for-the-badge)](https://discord.gg/tF4NSVkW6U) [![Discord](https://img.shields.io/badge/Discord-24292f?style=for-the-badge)](https://discord.gg/uaHe32VsyN)

[![Report Issue](https://img.shields.io/badge/Report%20Issue-24292f?style=for-the-badge)](https://discord.gg/XPMGcdnpmn) [![Feature Request](https://img.shields.io/badge/Feature%20Request-24292f?style=for-the-badge)](https://discord.gg/Ge99xqt5qr) [![Email Support](https://img.shields.io/badge/Email%20Support-24292f?style=for-the-badge)](mailto:support.toolsstudio@gmail.com)
