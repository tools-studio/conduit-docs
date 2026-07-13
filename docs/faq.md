# FAQ

**Does Conduit change my project automatically as soon as I install it?**
No. Conduit does nothing until you create a policy. Even the demo content is opt-in — see [Demo Workflow](demo-workflow.md).

**Does Conduit add anything to my player build?**
No. Conduit is editor-only. Its runtime assembly holds only data definitions (policy and rule types); none of it compiles into a player build.

**What happens to an asset if I disable a policy?**
Its import settings stay exactly as they were the last time Conduit applied them. Disabling a policy stops Conduit from enforcing or checking those properties going forward — it doesn't revert anything.

**Can I have different rules for iOS vs. Android?**
For textures and audio, yes — see the per-platform overrides sections in [Texture Rules](texture-rules.md) and [Audio Rules](audio-rules.md). Model Rules don't currently support per-platform overrides.

**Can I exclude one specific asset from an otherwise-governing policy?**
Not directly. The current workaround is moving the asset to a folder the policy doesn't cover. Per-asset exceptions are on the roadmap — see [Roadmap](../release/roadmap.md).

**Does Conduit work with version control?**
Yes. `ImportPolicy` assets are ordinary Unity assets and commit like any other. Conduit doesn't hold any state outside your project other than the per-user EditorPrefs it uses for a couple of one-time UI flags, which aren't meant to be shared across a team.

**Does scanning a large project block the Editor?**
No. Drift scans run asynchronously and yield periodically, so the Editor UI stays responsive during a scan. The Build Gate's check, which runs at build time, is synchronous by design — see [Build Gate](build-gate.md).

**Where do I ask something not covered here?**
See [Report Issues](../support/report-issues.md) or the [Community Discord](https://discord.gg/VrbxQ9vnrT).
