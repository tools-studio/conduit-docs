# Troubleshooting

## A build fails with "Build blocked: N policy violations found"

This is the Build Gate working as configured — see [Build Gate](features/build-gate.md). Open the Drift tab, scan, and fix the listed violations, or lower the relevant policy's enforcement level if blocking wasn't intended for it.

## "Conduit — Policy Conflict" appears on project load

Two or more `ImportPolicy` assets target the same folder. Conduit resolves this deterministically — the policy with the lexicographically lowest asset path wins, consistently across sessions and machines — but it's still worth cleaning up. Open the Policies tab, find the conflicted folder, and delete or retarget the duplicate policy.

## An asset isn't picking up policy changes

Conduit applies policy at import time and on an explicit Reimport or Fix action — it does not continuously watch already-imported assets for policy changes. After editing a policy, run a Drift scan to see which assets are now out of compliance, then Fix them, or use the Reimport tab to force affected assets to pick up the new values immediately.

## Drift scan shows an asset as ungoverned that I expect to be governed

Check that a policy actually targets an ancestor folder of the asset with **Apply to Subfolders** on, that the policy is enabled, and that at least one rule relevant to that asset's category is checked. An asset with zero applicable rules set anywhere in its inheritance chain is ungoverned by definition, not a bug.

## Install Demo Content does nothing, or reports an error about an existing folder

Conduit won't touch `Assets/Conduit/Demo/` if that folder already exists with content it didn't create. Rename or remove the conflicting folder and run **Install Demo Content** again. See [Demo Workflow](features/demo-workflow.md).

## Conduit doesn't appear in the menu bar after import

Conduit ships as compiled assemblies rather than source, so a missing menu item almost always means one of the two assemblies failed to load rather than a compile error in your own project. Check the Console for a load error naming `ToolsStudio.Conduit.dll` or `ToolsStudio.Conduit.Editor.dll`. If nothing appears there, close and reopen the project — an assembly that fails to load silently on the first domain reload after import sometimes loads correctly on the next one. If the menu item still doesn't appear, confirm both DLLs are actually present under the Conduit folder in your Project window and re-import the package from a fresh download.

## Unity reports a missing or incompatible assembly error for Conduit

**Problem.** The Console shows an error naming `ToolsStudio.Conduit` or `ToolsStudio.Conduit.Editor` as missing, invalid, or incompatible, rather than a normal C# compile error.

**Cause.** This is almost always one of three things: the package wasn't fully imported (one of the two `.dll` files, or its accompanying `.meta` file, didn't make it into the project); the DLL's Plugin Inspector platform settings were changed manually and no longer include the Editor platform; or a leftover copy of an older Conduit version — installed before you had the current one — is still present somewhere else in `Assets/` and is conflicting with the new one.

**Solution.** Select `ToolsStudio.Conduit.Editor.dll` in the Project window and confirm its Plugin Inspector has **Editor** checked under **Select platforms for plugin** — if it was unchecked, re-check it and click **Apply**. If the error persists, delete the entire Conduit folder from your project and re-import the package fresh from the Asset Store, rather than importing over an existing copy. Search your project for a second `ToolsStudio.Conduit.Editor.dll` under a different path if you've previously tested a different Conduit version, and remove the older one.

## A type or namespace from Conduit isn't recognized in my own editor script

**Problem.** Your own `.cs` script references something under `ToolsStudio.Conduit` and Unity reports it as an unknown type or namespace, even though Conduit's menu and window work normally.

**Cause.** Your script's assembly definition doesn't reference Conduit's assembly. Referencing a compiled `.dll` from your own code needs an explicit assembly reference the same way referencing another source assembly would — Conduit's own `.asmdef` doesn't exist in a `Binary Only` package for Unity to infer this from automatically.

**Solution.** In your own assembly definition asset, add `ToolsStudio.Conduit` (for the API surface — see [API Reference](api-reference.md)) or `ToolsStudio.Conduit.Editor` (for editor-only usage such as `CIBridge`) under **Assembly Definition References**. If your script isn't inside an assembly definition at all, place it under `Assets/Editor/` and let Unity's default editor assembly pick up the reference automatically, or create an `.asmdef` for it first.

## Still stuck

Check the [FAQ](faq.md). If that doesn't cover it, reach out on [Discord](https://discord.gg/zzzsw7SmUp) or by [email](mailto:support.toolsstudio@gmail.com) — see [Support](https://github.com/tools-studio/conduit-docs#support) for what to include in a report.
