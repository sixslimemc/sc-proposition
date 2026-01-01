# Loading & Rebuilding

SlimeCore completely overhauls the way datapacks are loaded.

## Loading
**Loading** *always* occurs upon world reload (`/reload`), and refers to the following sequence:

1. Call all [preload entrypoints](./manifest.md#preload_entrypoints) in the order specified by the world's current build.
2. Call all [load tags](#load-tags) in the order specified by the world's current build.
3. Call all [entrypoints](./manifest.md#entrypoints) in the order specified by the world's current build.

### Load Tags

A **load tag** is a function tag matching `#<pack ID>:load`. All packs **MUST** define a load tag.

Functions within the scope of load tag **SHOULD** setup/initialize the pack (analagous to `#minecraft:load`), but **SHOULD NOT** run the `/schedule` command or start any self-scheduling function loops ([entrypoints](./manifest.md#entrypoints) should be used for this purpose).

If a pack has any [dependencies](./manifest.md/#dependencies), the dependent pack's load tag is always called *after* all of their dependencies' load tags are called. 

## Rebuilding

**Rebuilding** refers to the following sequence:

1. Gather all [pack manifests](./manifest.md) of all installed packs (including disabled).
2. Verify that all packs have a valid and [known datapack path](TODO).
3. Evaluate a build with pack manifests of enabled packs.
      1. Verify that pack manifests are individually valid.
      2. Verify that there are no duplicate [packs IDs](./manifest.md/#pack_id-and-author_id).
      3. Verify that no [abstract interfaces](./manifest.md#abstract_declarations) are implemented more than once.
      4. Verify that all [dependencies](./manifest.md#dependencies) are fulfilled.
      5. Verify that all abstract interfaces are [implemented](./manifest.md#abstract_implementations).
      6. Verify that there are no dependency cycles.
      7. Verify that there exists possible [entrypoint](./manifest.md#entrypoints) and [preload entrypoint](./manifest.md#preload_entrypoints) orderings that fulfills all restrictions.
      8. Create a valid call order of preload entrypoints, load tags, and entrypoints.
4. Uninstall/disable packs in reverse load order (if any packs are being disabled/uninstalled).
5. Set the world's current build to the evaluated build.

If any step in the rebuild process fails, the world's previous build remains applied.

