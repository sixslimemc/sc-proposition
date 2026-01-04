# Loading & Rebuilding

SlimeCore completely overhauls the way datapacks are loaded.

As stated in *[Pack Manifests](./manifest.md)*:
> Packs **MUST NOT** add to the function tags `#minecraft:load` or `#minecraft:tick` Instead, packs **MUST** include a manifest that declares information on how to load them.

The `/datapack enable` and `/datapack disable` commands **SHOULD NOT** be used under any circumstance. Safe pack disabling/enabling/uninstalling is provided through [explicit rebuilding](#explicitly-rebuilding).

## Loading
**Loading** *always* occurs upon world reload (`/reload`), and refers to the following sequence:

1. Call all [preload entrypoints](./manifest.md#preload_entrypoints).
2. Call all [load tags](.//special_tags.md#load-tag).
3. Call all [entrypoints](./manifest.md#entrypoints).

Call order in each step is determined by the [world's current build](TODO).

## Rebuilding

**Rebuilding** is the process of re-evaluating the [world's current build](TODO) given the currently installed/enabled/disabled packs. More specifically, it refers to the following sequence:

1. Gather all [pack manifests](./manifest.md) of all installed packs (including disabled).
2. Verify that all packs have a [known datapack path](TODO).
3. Evaluate a build with pack manifests of enabled packs:
      1. Verify that pack manifests are individually valid.
      2. Verify that there are no duplicate [packs IDs](./manifest.md/#pack_id-and-author_id).
      3. Verify that no [abstract interfaces](./manifest.md#abstract_declarations) are implemented more than once.
      4. Verify that all [dependencies](./manifest.md#dependencies) are fulfilled.
      5. Verify that all abstract interfaces are [implemented](./manifest.md#abstract_implementations).
      6. Verify that there are no dependency cycles.
      7. Verify that there exists possible [entrypoint](./manifest.md#entrypoints) and [preload entrypoint](./manifest.md#preload_entrypoints) orderings that fulfills all restrictions.
      8. Create a valid call order of preload entrypoints, load tags, and entrypoints.
4. If any packs are being disabled/uninstalled, uninstall/disable packs in reverse load order.
5. Set the [world's current build](TODO) to the evaluated build.
6. Trigger a [load](#loading).

If any step in the rebuild process fails, the world's previous build remains applied.

By default, rebuilding occurs upon world reload (`/reload`) *iff* any changes to installed [pack manifests](./manifest.md) are detected (including additions/removals).

Rebuilding can be [explicitly triggered](#explicitly-rebuilding).

### Explicitly Rebuilding

Calling the [developer function](../rules/functions.md#developer-functions) `slimecore:rebuild` triggers an explicit rebuild. Explicit rebuilds allows for safe and tracked disabling/enabling/uninstalling of packs. Information on `slimecore:rebuild`'s usage can be found in it's respective [mcdoc](TODO) file within the [SlimeCore datapack](./index.md).