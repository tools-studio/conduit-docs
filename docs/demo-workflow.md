# Demo Workflow

Conduit can generate a small set of sample assets — with policies already applied and intentional drift already present — so you can see Drift Detection, Simulation, and Reimport working immediately, without setting anything up yourself.

## Installing demo content

Run **Tools Studio › Conduit › Demo › Install Demo Content**. This is the only way demo content is created — nothing is generated automatically when you install Conduit or open a project. The command creates `Assets/Conduit/Demo/` with a texture, an audio clip, and three model assets, each generated procedurally rather than shipped as binary files, plus a matching policy for each category under `Assets/Conduit/Policies/`.

Each generated asset has one or more properties deliberately set to violate its policy, so **Drift › Scan Project** has real violations to show you right away.

## Resetting demo content

Run **Tools Studio › Conduit › Demo › Reset Demo** to regenerate everything from scratch, restoring the intentional drift if you've already fixed it while experimenting.

## Checking demo status

Run **Tools Studio › Conduit › Demo › Demo Status** for a read-only summary: whether demo content is installed, whether the demo folder and its assets are present, and how many demo policies exist.

## What Conduit will never do

Demo installation never touches anything outside `Assets/Conduit/Demo/`. If that folder already exists with content Conduit didn't create, installation stops without modifying it — Conduit only writes to a folder it created itself, and only after confirming that.

## Removing demo content

Delete `Assets/Conduit/Demo/` and the three demo policy assets under `Assets/Conduit/Policies/` (`DemoTexturesPolicy`, `DemoAudioPolicy`, `DemoModelsPolicy`) the same way you'd delete any other assets. Nothing else in your project references them.
