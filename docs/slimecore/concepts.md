# Concepts

SC completely redefines and significantly extends Minecraft's datapack loading system; this page provides conceptual explanations in order to supplement further reading.

## Pack Manifests
All packs are required to define a **manifest**. A manifest contains metadata about a pack, notably: 

* Pack ID
* Author ID
* Pack version
* Dependencies
* Entrypoints
* Abstract interface declarations/implementations

### Pack & Author ID

A pack's **pack ID** must exactly match the name of its [primary namespace](TODO). A pack's **author ID** is a string that identifies it's author (though technically it's just an arbitrary string).

A given pack's pack ID and author ID together uniquely identify it; two packs with the same pack ID and author ID are recognized as the same pack.

No two installed packs can share the same pack ID--this would be a primary namespace conflict.

### Dependencies

A **dependency** is a pack that must be installed in order for another pack to work (or provide additional functionality). 

### Entrypoints

**Entrypoints** are function tags that are called after all packs are loaded, most generally used to initiate tick loops

## Load & Entrypoints

Packs do not add to the `#minecraft:load` and `#minecraft:tick` function tags. Instead, loading is split into 3 phases, each datapack defining it's own function tags for each. Phases are called in the following order, finishing entirely before the next is called:

1. **Preload Entrypoints**: Intended for technical/meta processes; it should be assumed that no resources from other packs exist at this phase.
2. **Load**: Standard load; analagous to `#minecraft:load`. Setup/initialization of packs should be done in this phase.
3. **Entrypoints**: Arbitrary function tags to be called after all packs are loaded.

Each pack has **exactly 1** `load` function tag. Load tag call order is based on dependency relations (dependencies must be loaded before their dependents).

Each pack can have **any number** of entrypoints and/or preload entrypoints. Entrypoint call order is based on order specifications made by each pack's manifest.

## Abstract Interfaces

Packs can declare any number of **abstract interfaces**. In the most general sense, an abstract interface represents a contract that must be fulfilled (implemented) by another installed pack. In other words, For every abstract interface a pack declares, there must exist exactly one other installed pack that implements it.

The implementation requirements of an abstract interface are entirely defined by the declaring pack's author; the abstract interface itself is just a representation/indicator.

Generally, abstract interfaces are used to represent [abstract functions](TODO), however, any behavior that requires exactly 1 external implementation can be represented by an abstract interface.

## Loading & Rebuilding

**Loading** (or a "load") refers to the process of calling all packs' load tags and entrypoints in the order specified by the current build.

**Rebuilding** (or a "rebuild") refers to the following sequence of processes:

1. Gather all pack manifests of all installed packs (including disabled).
2. Verify that all packs have a valid and known datapack path.
3. Evaluate a build with pack manifests of enabled packs.
      1. Verify that pack manifests are individually valid.
      2. Verify that there are no duplicate packs
      3. Verify that no abstract interface is implemented more than once.
      4. Verify that all dependencies are fulfilled.
      5. Verify that all abstract interfaces are implemented.
      6. Verify that there are no dependency cycles.
      7. Verify that there exists possible entrypoint and preload entrypoint orderings that fulfills all restrictions.
      8. Create pack load order and entrypoint/preload entrypoint order.
4. Uninstall/disable packs in reverse load order (if any packs are being disabled/uninstalled).
5. Set the world's current build to the evaluated build.

If any step in the rebuild process fails, the world's previous build remains applied.

Upon world reload (`/reload`), a rebuild will occur if changes to pack manifests are detected or new packs are enabled/disabled; a load will always occur regardless of if a rebuild occurred/succeeded.