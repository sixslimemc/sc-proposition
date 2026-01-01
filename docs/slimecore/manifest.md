# Pack Manifests

Packs **MUST NOT** add to the function tags `#minecraft:load` or `#minecraft:tick`. Instead, Packs inculde a **manifest** that declares information on how to load them.

Manifests declare following metadata about a pack:

* Pack ID
* Author ID
* Pack Version
* Download URL
* Dependencies
* Entrypoints
* Preload Entrypoints
* Abstract Interface Declarations
* Abstract Interface Implementations
* Library Indicator
* Display information

---

## Including a Manifest

Each pack **MUST** call the function `slimecore:api/manifest` *exactly once* within the scope of the function tag `#slimecore:manifest`.

The following is a minimal template for a manifest function:

```mcfunction
#> MANIFEST

data modify storage slimecore:in manifest.pack.pack_id set value "PACK ID"
data modify storage slimecore:in manifest.pack.author_id set value "AUTHOR ID"
data modify storage slimecore:in manifest.pack.version set value {major:-1, minor:0, patch:0}
# download MUST be of this exact version.
data modify storage slimecore:in manifest.pack.url set value "DOWNLOAD URL"
data modify storage slimecore:in manifest.pack.dependencies set value []
data modify storage slimecore:in manifest.pack.entrypoints set value []
data modify storage slimecore:in manifest.pack.preload_entrypoints set value []
data modify storage slimecore:in manifest.pack.abstract_declarations set value []
data modify storage slimecore:in manifest.pack.abstract_implementations set value []
data modify storage slimecore:in manifest.pack.is_library set value false

data modify storage slimecore:in manifest.pack.display.name set value "DISPLAY NAME"
data modify storage slimecore:in manifest.pack.display.summary set value "DISPLAY SUMMARY"
data modify storage slimecore:in manifest.pack.display.author_name set value "DISPLAY AUTHOR NAME"

# optional
data modify storage slimecore:in manifest.pack.display.links.author set value "AUTHOR URL"
data modify storage slimecore:in manifest.pack.display.links.info set value "INFO URL"
data modify storage slimecore:in manifest.pack.display.links.versions set value "LATEST/ALL VERSIONS URL"

function slimecore:api/manifest
```

*This function should be private and added to the function tag `#slimecore:manifest`.*

---

# Manifest Elements

## Pack ID (`pack_id`) & Author ID (`author_id`)

**Pack ID**: Must exactly match the pack's [primary namespace](TODO).

**Author ID**: A string that identifies the pack author (though technically is just an arbitrary string).

Together, a pack's pack ID and author ID uniquely identify it. Two packs that share the same pack ID and author ID are recognized as the same pack. No two installed packs can share the same pack ID; this would indicate a namespacing conflict.

## Pack Version (`version`)

The version of this pack, adhering to [semantic versioning](https://semver.org). Represented by a 3-key struct with keys `major`, `minor`, and `patch` (representing the version `<major>.<minor>.<patch>`). All 3 keys **MUST** be positive integers, with the minimum version being `0.1.0`.

## Download URL (`url`)

A direct download URL of this pack. Must download the exact version of this pack specified by `version`.

See *[Download URLs](#download-urls)*.

## Dependencies (`dependencies`)

List of this pack's dependency declarations.

A **dependency** is another pack that has it's resources and/or definitions referenced or used by this pack in *any way*. A dependency is said to be *required* if this pack cannot function without it, and *optional* if not (e.g. is only required for additional functionality).

each element of this list declares a dependency, and **MUST** be a struct with the following keys:

| Key | Description |
| --- | --- |
| `pack_id` | The [pack ID](#pack-id-pack_id--author-id-author_id) of the dependency. |
| `author_id` | The [author ID](#pack-id-pack_id--author-id-author_id) of the dependency. |
| `version` | The [version](#pack-version-version) of this dependency that is required, according to [semantic versioning](https://semver.org.). Includes keys `major` and `minor` (not `patch`). |
| `download.url` | The direct [download URL](#download-url-url) of the dependency. Download **MUST** be an exact version download, and a downloaded pack version must fulfill the version requirement specified by `version`. |
| `download.version` | The exact [version](#pack-version-version) of the dependency that `download.url` downloads. |
| `optional` | Boolean, `true` if dependency is optional, `false` if it is required. |

The following is a template for a dependency declaration:
```
# in manifest function...
data modify storage slimecore:in manifest.pack.dependencies append value {pack_id:"DEP PACK ID", author_id:"DEP AUTHOR ID", version:{major:-1, minor:0}, download:{url:"DEP DOWNLOAD URL", version:{major:-1, minor:0, patch:0}}, optional:false}
```

## Entrypoints (`entrypoints`) 

List of this pack's entrypoint declarations.

An **entrypoint** is a function tag matching `#<pack ID>:entrypoint/<entrypoint ID>`. Entrypoints are called after all packs are [loaded](TODO) in the order that they appear in `entrypoints`, while also respecting all 'before'/'after' relationships. Packs can declare any number of entrypoints.

each element of this list declares an entrypoint, and **MUST** be a struct with the following keys:

| Key | Description |
| --- | --- |
| `id` | This entrypoint's ID. A matching function tag **MUST** be defined at `#<pack ID>/entrypoint/<id>`. |
| `before` | List of entrypoints from other packs that this entrypoint must be *before* in call order. Key ***MAY** be omitted if empty.* |
| `after` | List of entrypoints from other packs that this entrypoint must be *after* in call order. Key ***MAY** be omitted if empty.* |

Each element of `before` and `after` keys **MUST** be a struct with the following keys:

| Key | Description |
| --- | --- |
| `pack_ref` | [Pack reference](#manifest-pack-references) (Referenced pack ID). |
| `id` | Entrypoint ID of referenced entrypoint. |

The following is a template for an entrypoint declaration:
```
# in manifest function...
data modify storage slimecore:in manifest.pack.entrypoints append value {id:"ENTRYPOINT ID"}
data modify storage slimecore:in manifest.pack.entrypoints[-1].before append value {pack_ref:"OTHER PACK ID", id:"OTHER ENTRYPOINT ID 1"}
data modify storage slimecore:in manifest.pack.entrypoints[-1].after append value {pack_ref:"OTHER PACK ID", id:"OTHER ENTRYPOINT ID 2"}
```

### Notes

Because packs cannot add to `#minecraft:tick`, it is standard to declare entrypoint(s) that start self-scheduling function loops. A common pattern is to declare a single entrypoint with ID `tick`.

With that being said, while one entrypoint is usually sufficient for a pack to function on it's own, a pack **SHOULD** declare multiple entrypoints if it performs multiple, conceptually distinct processes in it's tick loop. Declaring multiple entrypoints allows other packs more control over where they can insert their entrypoints, increasing integratability.

## Preload Entrypoints (`preload_entrypoints`)

List of this pack's preload entrypoint declarations.

A **preload entrypoint** is a function tag matching `#<pack ID>:preload_entrypoint/<entrypoint ID>`. Preload entrypoints are functionally identical to regular [entrypoints](#entrypoints-entrypoints), except they are called *before* any packs are [loaded](TODO) (including the declaring pack).

Because they are called before any packs are loaded, anything within a preload entrypoint's scope **MUST** assume that no other packs exist, and that their own pack has not been loaded.

Preload entrypoint declarations follow the same format as [entrypoint](#entrypoints-entrypoints) declarations, but their `id` key must match to a `#<pack ID>:preload_entrypoint/<id>` function tag and their `before`/`after` keys must only reference other preload entrypoints.

### Notes

Preload entrypoints are intended for technical/meta pack processing and generally **SHOULD NOT** be declared without very good reason.

Preload entrypoints **SHOULD NOT** start any self-scheduling function loops.

## Abstract Interface Declarations (`abstract_declarations`)

List of this pack's abstract interface declarations.

An **abstract interface** is a *representation* of a contract that must be [fulfilled (implemented)](#abstract-interface-implementations-abstract_implementations) by another installed pack. For every abstract interface a pack declares, there must exist exactly one other installed pack that implements it.

Generally, abstract interfaces are used to represent [abstract functions](TODO), however, abstract interfaces can be used to represent *any* developer-defined contract that must be fulfilled by exactly one other pack.

It is the responsibility of the author to define and document the fulfillment requirements of each of their pack's declared abstract interfaces.

Packs that define any [abstract functions](TODO) **MUST** declare at least one abstract interface.

each element of this list declares an abstract interface, and **MUST** be a string indicating it's ID.

The following is a template for an abstract interface declaration:
```
# in manifest function...
data modify storage slimecore:in manifest.pack.abstract_interfaces append value "ABSTRACT INTERFACE ID"
```

## Abstract Interface Implementations (`abstract_implementations`)

List of this pack's declared abstract interface implementations.

If this pack implements any [abstract interfaces](#abstract-interface-declarations-abstract_declarations) from other packs, it **MUST** be indicated here. It is the responsibility of the developer to ensure that abstract interface implementation requirements are properly fulfilled.

each element of this list declares an abstract interface implementation, and **MUST** be a struct with the following keys:

| Key | Description |
| --- | --- |
| `pack_ref` | [Pack reference](#manifest-pack-references) (Referenced pack ID). |
| `id` | ID of implemented abstract interface. |

The following is a template for an abstract interface implementation declaration:
```
# in manifest function...
data modify storage slimecore:in manifest.pack.abstract_implementations append value {pack_ref:"OTHER PACK ID", id:"ABSTRACT INTERFACE ID"}
```

## Library Indicator (`is_library`)

Boolean indicating whether or not this pack is a library or not.

A pack **SHOULD** be indicated as a **library** if it does not provide any meaningful functionality or features on it's own and is intended to be used as a dependency of other packs; "Meaningful" is to the descretion of the developer to decide.

From a user's perspective, if a pack is a library, it **SHOULD** be uninstalled if there are no packs that declare it as a dependency.

## Display Information (`display`)

Informal information that is useful to users but is not used in processing.

**MUST** be a struct with the following keys:

| Key | Description |
| --- | --- |
| `name` | Display name of this pack. |
| `author_name` | Author's display name. |
| `summary` | Breif 1-2 sentence length summary of this pack. |
| `links` | Struct containing external URLs of the author's choosing (see below). |

`links` **MUST** be a struct with the following keys, any/all of which **MAY** be omitted:

| Key | Description |
| --- | --- |
| `author` | URL intended to represent the author in a datapack or Minecraft related context (e.g. github profile, modrinth profile, etc.). |
| `versions` | URL where the latest version or all versions of this pack can be found (e.g. github repository, modrinth page, etc.).
| `info` | URL to find information about this pack (e.g. documentation, wiki page, etc.).

See *[External URLs](#external-urls).*

---

## Footnotes

### Download URLs

> **TENTATIVE**

URLs **MUST** use the https protocol and be a direct download (i.e. no clicking required). While no standard download service is enforced, it is **RECOMMENDED** to use [GitHub releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository) or [Modrinth](https://modrinth.com). Redirect services or shortened URLs **SHOULD NOT** be used.

The downloaded file **MUST** be a compressed archive (.zip, .tar, .tar.gz, etc.) that contains the contents of the datapack itself and **SHOULD** be named according to [standard datapack names](TODO).

### External URLs

URLs **MUST** use the https protocol. Redirect services or shortened URLs **SHOULD NOT** be used. Users are expected to navigate external URLs with the same amount of caution as they would with any other URL on the internet.

### Manifest Pack References

The `pack_ref` key contains another pack's pack ID, indicating a reference to that pack in some way. Any pack referenced this way **MUST** be included in `dependencies`.