
# Public & Private Content

## Definition
**Public** content **MAY** be accessed by any datapack.

**Private** content **MUST NOT** be accessed/referenced by datapacks other than the datapack that it is defined in.

If content is not declared private, it is public.

## Declaring Private Content

The following describes private content declarations.

### Resources/Files

Resources under the `_` directory and all subdirectories (`data/<pack id>/<resource directory>/_/**`). This includes tags (`data/<pack id>/tags/<resource directory>/_/**`).

### Scoreboard Objectives

Scoreboard objectives that start with `_<pack id>`.

### NBT Storage Data

Storage data in the `<pack id>:_` storage location or any sub-path of it (`<pack id>:_/**`).

### Custom Item Data

All data under `_<pack id>` in `minecraft:custom_data`.

### Marker Data

All data under `data._<pack id>`.

### Entity Tags

Entity tags that start with `_<pack id>`.

### Items

Items with `_:true` in their `minecraft:custom_data` component.

*Datapack that added the data first is the defining datapack.*

### Entities

Entities with the `_` entity tag, excluding players.

*Datapack that added the tag first is the defining datapack.*