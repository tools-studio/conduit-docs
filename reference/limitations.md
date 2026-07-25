# Limitations

## Video import isn't governed yet

`VideoRuleSet` exists as a policy data type, and it appears in the policy inspector, but no applicator ships in the current version — video import settings are visible in a policy but not enforced. Planned for a future release; see [Roadmap](../release/roadmap.md).

## LOD generation doesn't reduce polygon count

Conduit's LOD generation clones the existing hierarchy into LOD levels at configurable screen-relative height thresholds. It does not decimate geometry — each LOD level uses the same mesh as the source. Actual mesh decimation requires a third-party library and is planned as an optional integration; see [Roadmap](../release/roadmap.md).

## No per-asset exceptions

A policy governs a folder; there's no way to exempt one specific asset within an otherwise-governed folder. The current workaround is moving the asset to a folder the policy doesn't cover, or narrowing the policy's folder scope. Per-asset exceptions are planned.

## No continuous watching of already-imported assets

Conduit applies policy at import time and on an explicit Reimport or Fix action. It does not run in the background checking already-imported assets against policy changes as they happen — run a Drift scan after changing a policy to see what's now out of scope. See [Reimport Workflow](../docs/reimport-workflow.md).

## Model Rules have no per-platform overrides

Unlike Texture Rules and Audio Rules, Model Rules apply the same values across every platform. Per-platform model overrides aren't currently supported.

## Two audio settings are intentionally excluded

Normalize and Ambisonic are visible in Unity's own Audio Import Settings but aren't exposed as Conduit rules — see [Audio Rules](../docs/audio-rules.md) for why. This isn't a gap to be filled later; it's a deliberate exclusion based on what's safe to apply automatically across a folder.

## Out of scope, not on the roadmap

Two ideas come up occasionally and are worth addressing directly: **AI-assisted policy suggestion** (generating a starting policy from a folder's existing assets) and **source-control PR integration** (surfacing drift or policy changes as part of a pull request). Neither is planned — see [Roadmap](../release/roadmap.md) for what actually is. Both would be substantial, separate efforts rather than incremental additions to the current architecture, so they're noted here as deliberately out of scope rather than left unaddressed.
