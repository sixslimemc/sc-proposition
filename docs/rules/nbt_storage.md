# NBT Storage Locations

Packs **MUST NOT** store data in any NBT storage locations except for those specified below.

## Valid Locations

### `<pack ID>:data`

For general-purpose public data.

Data within this storage location is public and **SHOULD** be persistent.

### `<pack ID>:_` (and any subpath)

For all private data. (By [definition](./private.md#nbt-storage-data), this is the only storage location that can hold private data.)

Packs **MAY** store data in any subpath of `<pack ID>:_` (e.g. `<pack ID>:_/foo/bar`). 

### `<pack ID>:api`

For specific/specialized-use public, ephemeral data.

Data within this storage location is public and **SHOULD** be ephemeral.

> Conceptually similar to `<pack ID>:hook` but allows for flexibility if a pack does not use defined hooks (such as calling an externally provided function via macro).

### `<pack ID>:hook`

For passing data through hooks *(see [Hooks](TODO))*.

### `<pack ID>:in`

For passing inputs to developer functions *(see [Developer Functions](./functions.md#developer-functions))*.

### `<pack ID>:out`

For passing outputs from developer functions *(see [Developer Functions](./functions.md#developer-functions))*.

### `<pack ID>:abstract/in`

For passing inputs to abstract functions *(see [Abstract Functions](TODO))*.

### `<pack ID>:abstract/out`

For passing outputs from abstract functions *(see [Abstract Functions](TODO))*.

### `<pack ID>:config`

For specifying pack configuration *(see [Pack Config](TODO))*.

