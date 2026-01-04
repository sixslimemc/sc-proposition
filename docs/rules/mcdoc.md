# Mcdoc

> TENTATIVE: [Mcdoc](https://spyglassmc.com/user/mcdoc/) is an unfinished project (and the SC author's fluency with it could be better).

[Mcdoc](https://spyglassmc.com/user/mcdoc/) is a language used to define and document NBT structure/types.

Datapacks **MUST** include a top-level `mcdoc` directory (`<datapack>/mcdoc/`) with contents that conform to the specifications mentioned below.

All public mcdoc definitions **SHOULD** be properly documented via [documentation comments](https://spyglassmc.com/user/mcdoc/specification.html#doc-comments).

> TENTATIVE: Mcdoc paths may be misunderstood by the SC author.

All [mcdoc path references](https://spyglassmc.com/user/mcdoc/specification.html#path) **MUST NOT** be absolute. *(I.e. mcdoc paths must start with `super::`)*.

## `def.mcdoc`

Mcdoc file for defining shared/global, named data types. If a data type is referenced multiple times, not tied to any specific context, or is intended to be referenced by other packs, it **SHOULD** be defined in `def.mcdoc`.

Data types defined in `def.mcdoc` **MAY** be referenced by any other mcdoc file.

`def.mcdoc` **MUST NOT** contain any `dispatch` or `inject` statements.

A datapack **MAY** omit the file `def.mcdoc` entirely if it would otherwise be empty.

## `data.mcdoc`

Mcdoc file for defining `<pack ID>:data` structure.

`data.mcdoc` **MUST** define a struct named `Data` and **MUST** dispatch it to `minecraft:storage[<pack ID>:data]` (`Data` defines the structure of `<pack ID>:data`).

`data.mcdoc` **SHOULD NOT** define any other named data types and **MUST NOT** include any other `dispatch` or `inject` statements.

##
## Private Structure (`_`)

From *[Private](./private#mcdoc-structure)*:

> All structure defined by mcdoc files in the `_` directory and all subdirectories (`<datapack>/mcdoc/_/**`) [is private].