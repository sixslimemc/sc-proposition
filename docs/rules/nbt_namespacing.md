# Content Namespacing

## Direct Namespacing

All content a pack defines that is namespaced directly (matches the format `<namespace>:<...>`) **MUST** have the namespace match the pack's pack ID (`<pack ID>:<...>`). *(This is already mostly enforced by the rest of the specification, but this includes things like random number sequences, stopwatches, etc.)*

## Custom Data Component

Packs **MUST NOT** define any data in the `minecraft:custom_data` component outside of the path/struct `<pack ID>`. *(I.e. all data defined in `minecraft:custom_data` must have it's path start with `<pack ID>.`.)*

*As of Minecraft version 1.21.11, this applies to items, blocks, and entities. Entities have their `minecraft:custom_data` at path `data`, as opposed to `components."minecraft:custom_data"`.*

From [Private](./private.md#minecraftcustom_data-component-data):

> Data at path `<pack ID>._` in the `minecraft:custom_data` component [is defined as private].

## Pack-Defined Data Locations

In instances where a pack defines it's own public shared data location with the intention of other packs storing/defining arbitrary data (i.e. serves a similar function of the `minecraft:custom_data` component), the same rules for `minecraft:custom_data` mentioned above **SHOULD** be enforced/followed. This includes the indication of private; data at relative path `<pack ID>._` **SHOULD** be treated as private.