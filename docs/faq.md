# FAQ

### Does Conduit change my project automatically as soon as I install it?

No. Conduit does nothing until you create a policy. Even the demo content is opt-in — see [Demo Workflow](features/demo-workflow.md).

### Does Conduit add anything to my player build?

No. Conduit is editor-only. Its runtime assembly holds only data definitions (policy and rule types); none of it compiles into a player build.

### What happens to an asset if I disable a policy?

Its import settings stay exactly as they were the last time Conduit applied them. Disabling a policy stops Conduit from enforcing or checking those properties going forward — it doesn't revert anything.

### Can I have different rules for iOS vs. Android?

For textures and audio, yes — see the per-platform overrides sections in [Texture Rules](features/texture-rules.md) and [Audio Rules](features/audio-rules.md). Model Rules don't currently support per-platform overrides.

### Can I exclude one specific asset from an otherwise-governing policy?

Not directly. The current workaround is moving the asset to a folder the policy doesn't cover.

### Does Conduit govern video import settings?

Not yet. `VideoRuleSet` exists as a policy data type and appears in the policy inspector, but no applicator ships in the current version — video import settings are visible in a policy but not enforced.

### Does LOD generation reduce polygon count?

No. Conduit's LOD generation clones the existing hierarchy into LOD levels at configurable screen-relative height thresholds; each LOD level uses the same mesh as the source. Actual mesh decimation requires a third-party library and isn't included.

### Does Conduit work with version control?

Yes. `ImportPolicy` assets are ordinary Unity assets and commit like any other. Conduit doesn't hold any state outside your project other than the per-user EditorPrefs it uses for a couple of one-time UI flags, which aren't meant to be shared across a team.

### Does scanning a large project block the Editor?

No. Drift scans run asynchronously and yield periodically, so the Editor UI stays responsive during a scan. The Build Gate's check, which runs at build time, is synchronous by design — see [Build Gate](features/build-gate.md).

### Does Conduit include source code?

No. Conduit is a **Proprietary**, `Binary Only` product: the Asset Store package contains compiled assemblies (`ToolsStudio.Conduit.dll` and `ToolsStudio.Conduit.Editor.dll`), not editable `.cs` source. Conduit is not open source, and its source repository is private.

If you need source-level access — for an internal fork, a deep integration, or another reason source alone can address — contact Tools Studio directly at support.toolsstudio@gmail.com to discuss whether that's possible for your situation. Reaching out doesn't guarantee access is granted; it starts a direct conversation about what you need.

### What license governs my Conduit purchase?

Your purchase and use of the Conduit Asset Store package is governed by Unity's own Standard End User License Agreement, the same as any other Asset Store product — Tools Studio doesn't ship a separate license file inside the package. Conduit's source repository is private and unlicensed for public use, since it isn't publicly distributed.

### Where do I ask something not covered here?

See [Troubleshooting](troubleshooting.md), or reach out on [Discord](https://discord.gg/zzzsw7SmUp) or by [email](mailto:support.toolsstudio@gmail.com).
