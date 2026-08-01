# Overview

Conduit is policy-driven asset import governance for Unity. You define per-folder rules once for texture, model, and audio import settings, and Conduit keeps every asset in that folder — and its subfolders — compliant with them automatically.

## The problem this solves

Import settings drift over time. Someone changes a texture's compression to test something, forgets to revert it, and it ships that way. A new artist adds assets without knowing the project's conventions. Nobody notices until a build is larger than it should be or a platform behaves inconsistently. Conduit replaces manual conventions and hand-rolled `AssetPostprocessor` scripts with a folder-scoped policy system: define the rules once, and every asset in scope is checked, applied, and kept in sync.

## Key Capabilities

- **Folder-scoped, cascading policies** — define rules once per folder, with per-property opt-in and inheritance down the folder tree.
- **Three enforcement levels** — `Disabled`, `Warn Only`, or `Block Build`, chosen per policy.
- **Drift detection** — scan a project (or one folder) for assets whose import settings no longer match policy, with one-click or bulk fixes.
- **Simulation** — preview exactly what Conduit would apply to a specific asset, and which policy set each value, without touching anything.
- **Reimport** — apply policy and reimport a chosen set of assets in one action, independent of a drift scan, with a before/after size summary.
- **Build Gate** — block or warn on policy violations at build time, with a CI-runnable equivalent (`CIBridge`) for headless pipelines.
- **Scripted access** — every workflow above is also available through a static C# facade; see [API Reference](api-reference.md).
- **Editor-only** — Conduit's runtime assembly holds policy data types only; nothing it does contributes to a player build.

## Screenshots

![Conduit policy editor showing folder-scoped audio rules](images/screenshot-01-policies.png)

The Policies tab governing a folder's audio rules — the folder tree on the left shows policy coverage at a glance, while the panel on the right toggles individual rules, each governed by an enforcement level of `Disabled`, `Warn Only`, or `Block Build`.

![Conduit drift scan results and import simulation cascade trace](images/screenshot-02-drift-and-simulation.png)

The Drift tab (top) lists every violation found by a project scan, with a one-click Fix per row. The Simulation tab (bottom) shows the effective policy for any asset, including which policy asset and folder each property is inherited from.

## Quick Links

- [Getting Started](getting-started.md) — your first policy, in under five minutes
- [API Reference](api-reference.md) — scripted access to every Conduit workflow
