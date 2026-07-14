# FAQ

## Does Conduit change my project automatically after installation?

No. Conduit does not modify your project immediately after installation.

Conduit only starts applying changes after you create and enable a policy. Demo content is also completely opt-in.

See: [Demo Workflow](demo-workflow.md)

---

## Does Conduit add anything to my player build?

No. Conduit is an editor-only tool.

Its runtime assembly only contains policy and rule definitions. No Conduit systems are included in your final player build.

---

## What happens when I disable a policy?

Disabling a policy stops Conduit from enforcing or checking that policy in the future.

Previously applied import settings remain unchanged. Conduit does not automatically revert assets to their previous state.

---

## Can I create different rules for different platforms?

Yes, for supported asset types.

Texture and Audio rules support platform-specific overrides.

Model rules currently do not support per-platform overrides.

See:
- [Texture Rules](texture-rules.md)
- [Audio Rules](audio-rules.md)

---

## Can I exclude a single asset from a policy?

Not currently.

The recommended workaround is moving the asset into a folder that is not covered by the policy.

Per-asset exceptions are planned for a future release.

See: [Roadmap](../release/roadmap.md)

---

## Does Conduit work with version control?

Yes.

`ImportPolicy` assets are standard Unity assets and can be committed normally with your project files.

Conduit does not store shared project state outside your project. The only external data is a small amount of user-specific EditorPrefs data used for local UI preferences.

---

## Does scanning a large project block the Unity Editor?

No.

Drift scans run asynchronously and periodically yield execution to keep the Editor responsive during scanning.

The Build Gate check runs synchronously during build validation by design.

See: [Build Gate](build-gate.md)

---

## Where can I ask questions or report issues?

For questions, workflow help, or troubleshooting, visit the Tools Studio Community Discord.

For bugs and technical issues, use the issue reporting workflow.

See:
- [Report Issues](../support/report-issues.md)
- [Community Discord](https://discord.gg/VrbxQ9vnrT)
