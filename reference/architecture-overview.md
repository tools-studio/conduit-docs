# Architecture Overview

Conduit is built around one idea: a single resolution engine that every feature reads from.

```
Policy assets (ImportPolicy)
        ↓
Policy graph (folder → policy lookup, rebuilt when policies change)
        ↓
Resolver (cascades the folder chain, merges rule sets, caches results)
        ↓
   ┌────┴────┬──────────────┬─────────────┐
Import-time   Drift scan   Simulation   Build Gate
application
```

## Why one resolver

Texture, model, and audio import all need the same answer to the same question — "what does policy say for this asset, right now?" Import-time application, drift scanning, simulation, and the build gate all call the same resolver rather than each re-implementing folder cascading independently. This is why Simulation's preview and Drift's violation report always agree with each other, and with what actually gets applied on import.

## Editor-only by design

Conduit's Runtime assembly holds policy data types only — `ImportPolicy`, the rule sets, `ConduitProjectSettings` — with no logic and no `UnityEditor` reference. Everything that reads or writes an `AssetImporter` lives in the Editor assembly, which is excluded from player builds entirely.

## Two ways policy gets applied

**Automatically, at import time.** An `AssetPostprocessor` intercepts new and reimported assets and applies the resolved policy before the import completes, so assets never exist in an unpolicied state even briefly.

**On demand, via Reimport or Fix.** Already-imported assets that have drifted need an explicit action — Conduit doesn't watch the AssetDatabase continuously for import-setting changes. See [Reimport Workflow](../docs/reimport-workflow.md).

## Where to go deeper

This page covers enough to use the API confidently. The rule-by-rule mechanics live in [Texture Rules](../docs/texture-rules.md), [Audio Rules](../docs/audio-rules.md), and [Model Rules](../docs/model-rules.md).
