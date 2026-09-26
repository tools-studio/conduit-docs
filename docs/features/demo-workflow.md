# Demo Workflow

Conduit can generate a small set of sample assets, with policies already applied and intentional drift already present, so you can see Drift Detection, Simulation, and Reimport working immediately.

## Purpose

Try every workflow on safe sample content before you set up policies for your own project.

## Workflow

### Installing demo content

Run **Tools › Conduit › Demo › Install Demo Content**. This is the only way demo content is created. Nothing is generated automatically when you install Conduit or open a project.

The command creates `Assets/Conduit/Demo/` with a texture, an audio clip, and three model assets, each generated procedurally rather than shipped as binary files, plus a matching policy for each category under `Assets/Conduit/Policies/`. Each generated asset has at least one property deliberately set to violate its policy, so **Drift › Scan Project** has real violations to show you right away.

### Resetting demo content

Run **Tools › Conduit › Demo › Reset Demo** to regenerate everything from scratch, restoring the intentional drift if you have already fixed it while experimenting.

### Checking demo status

Run **Tools › Conduit › Demo › Demo Status** for a read-only summary: whether demo content is installed, whether the demo folder and its assets are present, and how many demo policies exist.

### Removing demo content

Delete `Assets/Conduit/Demo/` and the three demo policy assets under `Assets/Conduit/Policies/` (`DemoTexturesPolicy`, `DemoAudioPolicy`, and `DemoModelsPolicy`) the same way you would delete any other assets. Nothing else in your project references them.

## What Conduit will never do

Demo installation never touches anything outside `Assets/Conduit/Demo/`. If that folder already exists with content Conduit did not create, installation stops without modifying it. Conduit only writes to a folder it created itself, and only after confirming that.
