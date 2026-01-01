# Naming Specifications

### Scoreboard Objectives

Public scoreboard objective names **MUST** be in the form `<pack ID>.<identifier>`, where `identifier` is a [public identifier](#public-identifiers).

Private scoreboard objective names (with the exception of the [private register](./scoreboards.md#TODO)) **MUST** be in the form `_<pack ID>.<identifier>` where `identifier` is a [private identifier](#private-identifiers).

### Entity Tags

Public entity tag names **MUST** be in the form `<pack ID>.<identifier>`, where `identifier` is a [public identifier](#public-identifiers).

Private entity tag names (with the exception of the [private entity indicator](./private.md#entities) and [owned entity indicator](./owned.md#entities)) **MUST** be in the form `_<pack ID>.<identifier>` where `identifier` is a [private identifier](#private-identifiers).

###NBT Struct Keys

NBT struct keys in any public data location **SHOULD** conform to the following:

* Contain only lowercase alphanumeric characters and `_`.
* Not start or end with `_`.

It **MAY** be reasonable to ignore these guidelines if an NBT key is intended to be manipulated or interacted with via macro.

NBT struct keys in private data locations have no restrictions.

---

## Identifiers

Identifiers are a concept defined by this specification for reference only and are not related to any in-game object/concept.

### Public Identifiers

Public identifiers **MUST** conform to the following:

* 1-64 characters in length.
* Contain only lowercase alphanumeric characters, `-`, and `_`.
* Not start or end with `-` or `_`.

While not strictly required, public identifiers **SHOULD** conform to the following:

* 3-32 characters in length.
* Contain no instances of multiple `-` or `_` in sequence.
* Use `_` to convey a space and `-` to convey a seperator.
* Start with a letter.

### Private Identifiers

Private identifiers **SHOULD** conform to the [public identifier requirements](#public-identifiers).