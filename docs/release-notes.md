# Release Notes

What's new for Conduit, release by release. For day-to-day product documentation, start at [Overview](overview.md).

## 1.0.0 — 2026-07-13

Initial release.

**What's new**

- Policy-driven governance for texture, model, and audio import settings, applied automatically per folder.
- Drift detection — find assets whose import settings no longer match policy, and fix them individually or in bulk.
- Simulation — preview exactly what a policy would apply to an asset before committing to anything.
- A build-time gate that can block or warn on policy violations, with a CI-runnable equivalent for headless pipelines.
- Automatic LOD generation on model import, and full Audio Rules coverage including Preload Audio Data and Load In Background.
- Optional demo content, so you can try every workflow on a small, realistic project without setting anything up yourself.
- Scripted access to every workflow through a static C# API — see [API Reference](api-reference.md).

**Notable fixes**

- Resolved a deadlock risk in the Build Gate on large projects.
- Fix, Fix All, and Reimport now ask for confirmation before applying changes, since they overwrite existing import settings.
- Fixed a crash installing demo content into a brand-new project, and cleaned up import warnings that followed it.

**Compatibility**

- Unity 2021.3 LTS or later, through Unity 6000.x (build-verified against 6000.3.10f1).
- Editor-only — Conduit adds nothing to your player builds.
