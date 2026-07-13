# Build Gate

The Build Gate stops a Unity build before it starts if any asset governed by a `Block Build` policy is out of compliance.

## How it triggers

Conduit hooks into Unity's build pipeline via `IPreprocessBuildWithReport`. Every build — from the Build Settings window, a build script, or CI — runs the gate check first. If any governed asset is out of compliance with a policy at `Block Build` enforcement, the build fails immediately with an error naming the asset, the property, and the expected value. Violations at `Warn Only` are logged but don't stop the build; violations under `Disabled` policies aren't checked at build time at all.

## The Build Gate tab

Use the **Build Gate** tab to run the same check on demand, without starting a real build. This is the fastest way to check whether your project would currently pass before you actually kick off a build.

## CI usage

Run Conduit's gate check in batch mode as part of your CI pipeline:

    Unity -batchmode -quit \
      -projectPath /path/to/project \
      -executeMethod ToolsStudio.Conduit.Editor.CIBridge.Run \
      -conduitReport reports/conduit-gate.json

Exit code `0` means no blocking violations. Exit code `1` means one or more `Block Build` violations were found — the JSON report at the path you specified contains the full list. Exit code `2` means the scan itself couldn't complete; check the Editor log. See the JSON report schema and exit code table in [Configuration Reference](../reference/configuration-reference.md).

## Disabling the gate temporarily

Project Settings has a **Suppress Build Gate On CI** toggle for cases where you need a build to proceed despite known violations — for example, cutting a build from a branch mid-cleanup. It only takes effect when the `CONDUIT_CI_SKIP` scripting define is also set, so it can't silently suppress the gate in a normal editor build — both the toggle and the define are required together, intentionally, for a CI step that runs the gate check separately from the main build.
