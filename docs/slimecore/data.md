# SlimeCore Data

After a successful [rebuild](./rebuilding.md#rebuilding), SlimeCore stores the build data in `slimecore:data`, referred to as the "world's current build", seperated into 2 paths `build` and `world`.

All data in `slimecore:data` **MUST** be treated as read-only. *Direct manipulation of data within `slimecore:data` may result in unexpected behavior.*

## `build`

Data stored at NBT path `build` is "pure" build data; it is evaluated solely using the data that [pack manifests](./manifest.md).

The following are keys within the `build` struct:

### `packs`

The array of all enabled packs' [pack manifests](./manifest.md), in order in which their [load tags](./special_tags.md#load-tag) are called.

### `order.load`

An array of indexed pack references indicating the order in which pack [load tags](./special_tags.md#load-tag) are called.

Each element is a struct with the following keys:

| Key | Description |
| --- | --- |
| `pack_ref` | Pack ID of the represented pack. |
| `index` | Index of this pack in load order. |

### `order.entrypoint`

An array of indexed entrypoint references indicating the order in which [entrypoints](./manifest.md#entrypoints) are called.

Each element is a struct with the following keys:

| Key | Description |
| --- | --- |
| `pack_ref` | Pack ID of the pack this entrypoint is from. |
| `id` | Entrypoint ID of the represented entrypoint. |
| `index` | Index of this entrypoint in load order. |

### `order.preload_entrypoint`

An array of indexed entrypoint references indicating the order in which [preload_entrypoints](./manifest.md#preload_entrypoints) are called.

Each element is a struct with the following keys:

| Key | Description |
| --- | --- |
| `pack_ref` | Pack ID of the pack this preload entrypoint is from. |
| `id` | Preload entrypoint ID of the represented entrypoint. |
| `index` | Index of this entrypoint in load order. |

### `aux.pack_map`

*Auxilary value--is just a useful re-representation of data, does not contain any new information.*

Struct that contains a key for every pack in the current build, it's key name being a pack ID, and it's value being the corresponding pack's [pack manifest](manifest.md) (`{<pack ID...>:<pack manifest>}`).

### `aux.impl_map`

*Auxilary value--is just a useful re-representation of data, does not contain any new information.*

Struct that contains a path, `<pack ID>.<abstract interface ID>`, for every [declared abstract interface](./manifest.md#abstract_declarations), with the corresponding value being the [pack manifest](./manifest.md) of the pack that [implements](./manifest.md#abstract_implementations) it (`{<pack ID...>:{<abstract interface ID...>:<implementor's pack manifest>}}`).

## `world`

Data stored at NBT path `build` is world-specific data; it may differ between two worlds even if they have exactly the same installed packs.

The following are keys within the `world` struct:

### `disabled_packs`

Array of partial pack information that indicates which packs are installed but [disabled](./rebuilding.md#explicitly-rebuilding).

Each element is a partial [pack manifest](manifest.md), only containing the [`pack_id`](./manifest.md#pack_id-and-author_id), [`author_id`](./manifest.md#pack_id-and-author_id), and [`version`](./manifest.md#version) keys.

### `datapack_links`

Array that contains each installed pack's datapack path along with it's pack ID.

Each element is a struct with the following keys:

| Key | Description |
| --- | --- |
| `pack_ref` | Pack ID of the represented pack. |
| `path` | Datapack path of the represented pack (e.g. `file/foo`). |

### `aux.datapack_link_map`

*Auxilary value--is just a useful re-representation of data, does not contain any new information.*

Struct that contains a key for every installed pack, it's key name being it's pack ID and it's value being it's datapack path (`{<pack ID...>:<datapack path>}`).

---

## The SlimeCore Manifest

The NBT path `slimecore` in `slimecore:data` contains a mock/reference [pack manifest](./manifest.md) for SlimeCore itself.