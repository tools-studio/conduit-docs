# Simulation

The **Simulation** tab answers one question: for a specific asset, what would Conduit apply right now, and which policy decided each value?

<p align="center">
  <img src="../images/screenshot-02-drift-and-simulation.png" width="800" alt="Conduit import simulation cascade trace" />
</p>

Shows the Simulation tab (bottom panel) after simulating an asset: every governed property is listed with its effective value and the exact policy asset and folder it was inherited from.

## Running a simulation

Drag an asset into the drop target, or type its path into the Asset Path field, and click **Simulate**. Conduit resolves the full policy chain for that asset's folder — same resolution logic used for import-time application and drift scanning — and shows every governed property with the value that would be applied and the name of the policy that set it.

Nothing is written to disk or to the AssetDatabase. Simulation is read-only; running it repeatedly, or on an asset you have no intention of changing, has no effect on your project.

## Why this exists

When more than one policy applies to a folder — a project-wide default plus a more specific override further down the tree — it isn't always obvious from looking at either policy alone what the final resolved value will be. Simulation shows the resolved outcome directly, along with the cascade that produced it, instead of asking you to trace the inheritance chain by hand.

## When to use it

Use Simulation while authoring policies, to check the effect of a new rule before it applies to real assets. Use [Drift Detection](drift-detection.md) to check your whole project against policies that are already in effect.
