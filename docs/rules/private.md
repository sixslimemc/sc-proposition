
# Private Content

## Definition

**Private** content **MUST NOT** be accessed by packs other than the pack that it is defined in. *(I.e. private content does not exist to any pack outside of it's defining pack.)*

Content that is not private is **public**, and may be accessed by any pack.

---

The following are definitions of private content:

### Resources/Files

Resources in the `_` directory and all subdirectories (`<datapack>/data/<pack ID>/<resource directory>/_/**`). This includes tags (`data/<pack ID>/tags/<resource directory>/_/**`).

### Scoreboard Objectives

Scoreboard objectives that start with `_<pack ID>`.

### NBT Storage Data

Data in the `<pack ID>:_` storage location or any sub-path of it (`<pack ID>:_/**`).

### `minecraft:custom_data` Component Data

Data at path `<pack ID>._` in the `minecraft:custom_data` component. *(Applies to items, blocks, and entities.)*

*As of Minecraft version 1.21.11, entities have their `minecraft:custom_data` at path `data`, as opposed to `components."minecraft:custom_data"`.*


### Entity Tags

Entity tags that start with `_<pack ID>`.

### Items

Items with `_:true` in their `minecraft:custom_data` component.

*Pack that added the data first is the defining pack.*

### Entities

Entities with the `_` entity tag, excluding players.

*Pack that added the tag first is the defining pack.*

### Mcdoc Structure

All structure defined by mcdoc files in the `_` directory and all subdirectories (`<datapack>/mcdoc/_/**`).
