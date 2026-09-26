# Build Gate

The Build Gate stops a Unity build before it starts if any asset governed by a Block Build policy is out of compliance.

## Purpose

Make policy compliance a condition of shipping, so a drifting asset cannot reach a build unnoticed.

## Workflow

### When it runs

Every build, whether from the Build Settings window, a build script, or CI, runs the gate check first. If any governed asset is out of compliance with a policy at Block Build enforcement, the build fails immediately with an error naming the asset, the property, and the expected value.

Violations under Warn Only policies are logged but do not stop the build. Violations under Disabled policies are not checked at build time at all.

### The Build Gate tab

Use the **Build Gate** tab to run the same check on demand, without starting a real build. It is the fastest way to find out whether your project would currently pass.

## Configuration

### CI usage

Run the gate check in batch mode as part of your CI pipeline:

```
Unity -batchmode -quit \
  -projectPath /path/to/project \
  -executeMethod ToolsStudio.Conduit.Editor.CIBridge.Run \
  -conduitReport reports/conduit-gate.json
```

Exit code `0` means no blocking violations. Exit code `1` means one or more Block Build violations were found, and the JSON report at the path you specified contains the full list. Exit code `2` means the scan itself could not complete, so check the Editor log. Two more flags are available: `-conduitFolder <path>` scopes the scan to one folder, and `-conduitWarnAsError` makes Warn Only violations blocking for that run. See [Configuration](../configuration.md) for the full argument list, the JSON report format, and the exit codes.

### Disabling the gate temporarily

Project settings include a **Suppress Build Gate On CI** toggle for cases where a build must proceed despite known violations, for example cutting a build from a branch mid-cleanup. It only takes effect when the `CONDUIT_CI_SKIP` scripting define is also set, so it cannot silently suppress the gate in a normal Editor build. Both are required together, intentionally, for a CI step that runs the gate check separately from the main build.
