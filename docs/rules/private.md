
# Private Content

## Definition

**Private** content **MUST NOT** be accessed by packs other than the pack that it is defined in.

If content is not declared private, it is **public**, and may be accessed by any pack.

---

The following are definitions of private content:

### Resources/Files

Resources under the `_` directory and all subdirectories (`data/<pack ID>/<resource directory>/_/**`). This includes tags (`data/<pack ID>/tags/<resource directory>/_/**`).

### Scoreboard Objectives

Scoreboard objectives that start with `_<pack ID>`.

### NBT Storage Data

Data in the `<pack ID>:_` storage location or any sub-path of it (`<pack ID>:_/**`).

### Custom Item Data

Data at NBT path `components."minecraft:custom_data"._<pack ID>`.

### Custom Entity Data

Data at NBT path `data._<pack ID>`.

### Entity Tags

Entity tags that start with `_<pack ID>`.

### Items

Items with `_:true` in their `minecraft:custom_data` component.

*Pack that added the data first is the defining pack.*

### Entities

Entities with the `_` entity tag, excluding players.

*Pack that added the tag first is the defining pack.*