# Getting Started

Conduit governs Unity import settings by folder. You define a policy once for a folder, and Conduit keeps every texture, model, and audio clip in that folder — and its subfolders — compliant with it.

## The problem this solves

Import settings drift over time. Someone changes a texture's compression to test something, forgets to revert it, and it ships that way. A new artist adds assets without knowing the project's conventions. Nobody notices until a build is larger than it should be or a platform behaves inconsistently.

Conduit replaces manual conventions and hand-rolled `AssetPostprocessor` scripts with a folder-scoped policy system: define the rules once, and every asset in scope is checked, applied, and kept in sync.

## How it fits into your workflow

1. You create an `ImportPolicy` asset targeting a folder — for example, `Assets/Textures/UI`.
2. You turn on the rules that matter for that folder — max texture size, compression, mip maps, whatever applies.
3. New assets imported into that folder get those settings automatically.
4. Existing assets that don't match get flagged by a drift scan, and you fix them in one click.
5. If you enable build-blocking enforcement, a non-compliant asset stops the build until it's fixed.

## Next steps

- [Installation](installation.md) — add Conduit to your project
- [Quick Start](quick-start.md) — your first policy, in under five minutes
- [Core Concepts](core-concepts.md) — policies, cascading, enforcement levels, and drift, explained once so the rest of the docs don't have to
