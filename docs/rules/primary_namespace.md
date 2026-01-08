# Primary Namespace

## Definition

A datapack **MUST** have exactly one **primary namespace** (also known as it's [pack ID](../slimecore/manifest.md#pack_id-and-author_id)).

A datapack **MUST NOT** define new resources in namespaces other than it's primary namespace.

A datapack **MAY** overwrite/include resources in other namespaces if the resource already exists, given all [dependencies](../slimecore/manifest.md#dependencies) are fulfilled. Resources manipulated in this way have their namespace referred to as **secondary namespaces** of the datapack.

For example, including the block tag [`#minecraft:dampens_vibrations`](https://minecraft.wiki/w/Block_tag_(Java_Edition)#dampens_vibrations) in your datapack is allowed, and would make `minecraft` a secondary namespace. Adding a new block tag that does not already exist in the `minecraft` namespace, like `#minecraft:foo_bar`, is not allowed.

## Naming

Primary namespace names **MUST** fulfill the following criteria:

* Valid [namespace](https://minecraft.wiki/w/Identifier#Namespaces) name.
* 1-64 characters in length.
* Contain only lowercase alphanumeric characters and `-`.
* Not start or end with `-`.
* Not be `minecraft` or `slimecore`.

### Recommendations

While not strictly required, a good primary namespace name **SHOULD** follow the guidelines:

* 4-32 characters in length.
* Uses `-` only as a "module" separator (e.g. `foo-feature1` and `foo-feature2`).
* Avoids numbers.
* Is as long/specific as reasonably possible if pack is not a [library](../slimecore/manifest.md#is_library) (or is not expected to be accessed frequently).
* Is easy to type but still reasonably specific if pack is a [library](../slimecore/manifest.md#is_library) (or is expected to accessed frequently).
