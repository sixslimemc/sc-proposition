# Top-Level Function Tags

Packs **MUST** define the following top-level function tags. These tags are automatically called by SlimeCore when appropriate. Packs **MUST NOT** define other top-level function tags.

## Load Tag

A pack's **load tag** is `#<pack ID>:load`.

This tag is called on [load](./rebuilding.md#loading) and is functionally a replacement for `#minecraft:load`.

Functions within the scope of a load tag **SHOULD** setup/initialize it's pack, but **SHOULD NOT** run the `/schedule` command or start any self-scheduling function loops ([entrypoints](./manifest.md#entrypoints) should be used for this purpose).

If a pack has any [dependencies](./manifest.md/#dependencies), the dependent pack's load tag is always called *after* all of their dependencies' load tags are called. 

## Disable Tag

A pack's **disable tag** is `#<pack ID>:disable`.

This tag is called during [rebuilding](./rebuilding.md#rebuilding) just before it is disabled (and was not already disabled before rebuilding).

Functions within the scope of a disable tag **SHOULD** attempt to temporarily and cleanly remove it's packs influence/content with the assumption that it will be re-enabled in the future--such that the pack "picks back up where it left off" when re-enabled.

A pack's disable tag is *not* called when it is uninstalled, only when it is explicitly disabled.

## Uninstall Tag

A pack's **uninstall tag** is `#<pack ID>:uninstall`.

This tag is called during [rebuilding](./rebuilding.md#rebuilding) just before it is uninstalled (disabled permanently and untracked by SlimeCore).

Functions within the scope of an uninstall tag **MUST** permanently and cleanly remove as much content defined by it's pack as reasonably possible--such that re-enabling/re-installation of the pack would be no different than installing it for the first time. Content removal includes but is not limited too: scoreboard objectives, NBT storage data, entities, items, etc.