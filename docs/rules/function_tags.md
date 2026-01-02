# Function Tags

## Definition

A datapack **MUST NOT** define any [function tags](https://minecraft.wiki/w/Function_tag_(Java_Edition)) that are not described by one of the following:

* [Hook](#hooks)
* [Abstract Function](#abstract-functions)
* [Top-Level Function Tag](../slimecore/special_tags.md)
* [Entrypoint](../slimecore/manifest.md#entrypoints)
* [Preload Entrypoint](../slimecore/manifest.md#preload_entrypoints)

Function tags (regardless of type) **SHOULD NOT** be called in-game.

Functions included/specified by a function tag are referred to as the tag's **subscribers**.

---

## Hooks

A **hook** is a function tag within the `hook` directory or any subdirectory therein (`#<pack ID>:hook/<path...>/<hook name>`).

Hooks are public by [definition](./private.md). Any datapack that writes to hook tag (including it's definition) **MUST** set `"replace": false` (or omit `replace`).

While other packs can write to hook tags, hooks **MUST NOT** be called by any namespace other than the one that they are defined in.

### Passing Data

Dedicated data **MAY** be passed to/from subscribers. If so, it **MUST** be specified through a struct at path `<hook name>` (i.e. `<hook name>.<data key name>`) in the NBT storage location `<pack ID>:hook`. It is the responsibility of the developer to define how subscribers are allowed to interact with this data.

### Example Usage

*File for `#foo:hook/my_hook` in datapack `foo` (hook definition)*:
```json
{
    "values": []
}
```

*Function `foo:_/call_my_hook` in datapack `foo` (calling function):*
```mcfunction
// initial value of 'some_value' is 5.
data modify storage foo:hook my_hook set value {some_data:5}
// call hook:
function #foo:hook/my_hook
```

*File for `#foo:hook/my_hook` in datapack `bar` (specifying subscriber)*:
```json
{
    "values": [
        "bar:_/my_subscriber"
    ]
}
```

*Function `bar:_/my_implementor` in datapack `bar` (subscriber function):*
```mcfunction
// subscriber recieves 'some_value' as 5.

// multiply 'some_data' by 2:
execute store result storage foo:hook my_hook.some_data int 2 run data get storage foo:hook my_hook.some_data
// 'some_value' now 10.

// end of hook (or next subscriber in pipeline) would recieve 'some_value' as 10.
```

## Abstract Functions

### Concept

An abstract function represents a function with defined inputs and outputs, but an implementation that must be provided by another pack. Abstract functions are analagous to a [developer functions](./functions.md#developer-functions) but with the responsibilities of the defining and external pack reversed.

The following sequence is proper usage of an abstract function:

1. Caller (defining pack) sets inputs.
2. Caller calls abstract function tag (which calls it's subscriber/implementation).
3. Implementation (subscriber) performs it's function body and sets outputs.
4. Caller reads and interprets outputs.

### Definition
An **abstract function** is a function tag within the `abstract` directory or any subdirectory therein (`#<pack ID>:abstract/<path...>/<abstract function name>`). 

Abstract functions are public by [definition](./private.md). Any datapack that writes to an abstract function tag (excluding where it's defined) **MUST** set `"replace": true`.

Abstract functions **MUST** be defined empty and `"replace":false`.

While other packs can write to abstract function tags, abstract functions **MUST NOT** be called by any namespace other than the one that they are defined in.

If a pack define any abstract functions, it **MUST** declare at least one [abstract interface](../slimecore/manifest.md#abstract_declarations) (with the assumption that, for all abstract functions defined, providing a subscriber is part of the implementation requirements of some abstract interface).

#### Input and Output

Dedicated abstract function input(s) **MUST** be specified through a struct at path `<abstract function name>` (i.e. `<abstract function name>.<input name>`) in the NBT storage location `<pack ID>:abstract/in`.

Likewise, dedicated abstract function outputs(s) **MUST** be specified through a struct at path `<abstract function name>` (i.e. `<abstract function name>.<output name>`) in the NBT storage location `<pack ID>:abstract/out`.

Output struct **MUST** be null before it's abstract function is called.

Input and output structure **MUST** be declared through [mcdoc documentation](TODO).

### Example Usage

*File for `#foo:abstract/my_abstract` in datapack `foo` (abstract function definition)*:
```json
{
    "values": []
}
```

*Function `foo:_/call_my_abstract` in datapack `foo` (calling function):*
```mcfunction
// set inputs:
data modify storage foo:abstract/in my_abstract set value {a:3, b:5}
// clear output:
data remove storage foo:abstract/out my_abstract
// call abstract function:
function #foo:abstract/my_abstract
```

*File for `#foo:abstract/my_abstract` in datapack `bar` (specifying subscriber)*:
```json
{
    "replace": true,
    "values": [
        "bar:_/my_implementor"
    ]
}
```

*Function `bar:_/my_implementor` in datapack `bar` (subscriber function):*
```mcfunction
// add 'a' and 'b':
execute store result score *a _bar run data get storage foo:abstract/in my_abstract.a
execute store result score *b _bar run data get storage foo:abstract/in my_abstract.b
scoreboard players operation *a _bar += *b _bar
// perhaps another implementation would do something different.

// set output:
execute store result storage foo:abstract/out my_abstract.result int 1 run scoreboard players get *a _bar
```