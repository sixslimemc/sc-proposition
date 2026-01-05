# Mcdoc

> TENTATIVE: [Mcdoc](https://spyglassmc.com/user/mcdoc/) is an unfinished project (and the SC author's fluency with it could be better).

[Mcdoc](https://spyglassmc.com/user/mcdoc/) is a language used to define and document NBT structure/types.

Datapacks **MUST** include a top-level `mcdoc` directory (`<datapack>/mcdoc/`) with contents that conform to the specifications mentioned below.

All public mcdoc definitions **SHOULD** be properly documented via [documentation comments](https://spyglassmc.com/user/mcdoc/specification.html#doc-comments).

> TENTATIVE: Mcdoc paths may be misunderstood by the SC author.

All [mcdoc path references](https://spyglassmc.com/user/mcdoc/specification.html#path) **MUST NOT** be absolute. *(I.e. mcdoc paths must start with `super::`)*.

## `def.mcdoc`

For defining shared/global, named data types.

If a data type is referenced multiple times, not tied to any specific context, or is intended to be referenced by other packs, it **SHOULD** be defined in `def.mcdoc`.

Data types defined in `def.mcdoc` **SHOULD** be referenced by other mcdoc file(s).

`def.mcdoc` **MUST NOT** contain any `dispatch` or `inject` statements.

A datapack **MAY** omit the file `def.mcdoc` entirely if it would otherwise be empty.

## `data.mcdoc`

For defining [`<pack ID>:data`](./nbt_storage.md#pack-iddata) structure.

`data.mcdoc` **MUST** define a struct named `Data` and dispatch it to `minecraft:storage[<pack ID>:data]` (`Data` defines the structure of `<pack ID>:data`).

`data.mcdoc` **SHOULD NOT** define any other named data types and **MUST NOT** include any other `dispatch` or `inject` statements.

## `config.mcdoc`

For defining [`<pack ID>:config`](./nbt_storage.md#pack-idconfig) structure.

`config.mcdoc` **MUST** define a struct named `Config` and dispatch it to `minecraft:storage[<pack ID>:config]` (`Config` defines the structure of `<pack ID>:config`).

`data.mcdoc` **SHOULD NOT** define any other named data types and **MUST NOT** include any other `dispatch` or `inject` statements.

## `func.mcdoc`

> TENTATIVE: This may not need to exist once `inject` is implemented in mcdoc; dedicated function mcdoc files could just inject into the in/out structs.

For defining [`<pack ID>:in` and `<pack ID>:out`](./functions.md#developer-functions) structure via the structs defined in [function mcdoc files](#function).

`func.mcdoc` **MUST** define structs named `In` and `Out` and dispatch them to `minecraft:storage[<pack ID>:in]` and `minecraft:storage[<pack ID>:out]` respectively.

For every [developer function](./functions.md#developer-functions), `<pack ID>:<path...>/<function name>`, a pack defines, `In`/`Out` **MUST** contain the key `<function name>` that's type is the respective [function mcdoc file's](#function) definition of `In`/`Out` respectively--or a union of them if multiple functions share the same `<function name>`. *(This is just an enforcement of the [input/output specification](./functions.md#developer-functions).)*

The `In` and `Out` structs **MUST NOT** define any keys outside of the specification described above.

`func.mcdoc` **MUST NOT** define any other named data types or include any other `dispatch` or `inject` statements.

A datapack **MAY** omit the file `func.mcdoc` entirely if it does not define any [developer functions](./function_tags.md#hooks).

#### Example

Pack `foo` defines the developer functions:

* `foo:bar`
* `foo:baz`
* `foo:qux/baz`

it's `func.mcdoc` is:

```mcdoc
dispatch minecraft:storage[foo:in] to struct In {
    bar: super::function::bar::In,
    baz: (super::function::baz::In, super::function::qux::baz::In)
}
dispatch minecraft:storage[foo:out] to struct Out {
    bar: super::function::bar::Out,
    baz: (super::function::baz::Out, super::function::qux::baz::Out)
}
```

## `abstract.mcdoc`

> TENTATIVE: This may not need to exist once `inject` is implemented in mcdoc; dedicated function mcdoc files could just inject into the in/out structs. 

For defining [`<pack ID>:abstract/in` and `<pack ID>:abstract/out`](./function_tags.md#abstract-functions) structure via the structs defined in [abstract function mcdoc files](#abstract). *(Very similar to [`func.mcdoc`](#funcmcdoc).)*

`abstract.mcdoc` **MUST** define structs named `In` and `Out` and dispatch them to `minecraft:storage[<pack ID>:abstract/in]` and `minecraft:storage[<pack ID>:abstract/out]` respectively.

For every [abstract function](./function_tags.md#abstract-functions), `#<pack ID>:<path...>/<abstract function name>`, a pack defines, `In`/`Out` **MUST** contain the key `<abstract function name>` that's type is the respective [abstract function mcdoc file's](#abstract) definition of `In`/`Out` respectively--or a union of them if multiple functions share the same `<abstract function name>`. *(This is just an enforcement of the [input/output specification](./function_tags.md#abstract-functions).)*

The `In` and `Out` structs **MUST NOT** define any keys outside of the specification described above.

`abstract.mcdoc` **MUST NOT** define any other named data types or include any other `dispatch` or `inject` statements.

A datapack **MAY** omit the file `abstract.mcdoc` entirely if it does not define any [abstract functions](./function_tags.md#abstract-functions).

#### Example

Pack `foo` defines the abstract functions:

* `foo:abstract/bar`
* `foo:abstract/baz`
* `foo:abstract/qux/baz`

it's `abstract.mcdoc` is:

```mcdoc
dispatch minecraft:storage[foo:abstract/in] to struct In {
    bar: super::abstract::bar::In,
    baz: (super::abstract::baz::In, super::abstract::qux::baz::In)
}
dispatch minecraft:storage[foo:abstract/out] to struct Out {
    bar: super::abstract::bar::Out,
    baz: (super::abstract::baz::Out, super::abstract::qux::baz::Out)
}
```

## `hook.mcdoc`

For defining [`<pack ID>:hook`](./function_tags.md#hooks) structure via the structs defined in [hook mcdoc files](#hook).

`hook.mcdoc` **MUST** define a struct named `Hook` and dispatch it to `minecraft:storage[<pack ID>:hook]`.

for every [hook](./function_tags.md#hooks), `#<pack ID>:<path...>/<hook name>`, a pack defines, `Hook` **MUST** contain the key `<hook name>` that's type is the respective [hook mcdoc file's](#hook) definition of `Hook` respectively--or a union of them if multiple hooks share the same `<hook name>`. *(This is just an enforcement of the [data-passing specification](./function_tags.md#passing-data).)*

The `Hook` struct **MUST NOT** define any keys outside of the specification described above.

`hook.mcdoc` **MUST NOT** define any other named data types or include any other `dispatch` or `inject` statements.

A datapack **MAY** omit the file `hook.mcdoc` entirely if it does not define any [hooks](./function_tags.md#hooks).

#### Example

Pack `foo` defines the hooks:

* `#foo:hook/bar`
* `#foo:hook/baz`
* `#foo:hook/qux/baz`

it's `hook.mcdoc` is:

```mcdoc
dispatch minecraft:storage[foo:hook] to struct Hook {
    bar: super::function::bar::Hook,
    baz: (super::function::baz::Hook, super::function::qux::baz::Hook)
}
```

## `extern.mcdoc`

For external/misc. `dispatch` and `inject` statements.

All public `dispatch` and `inject` statements that are not otherwise described in this specification (e.g. dispatching `mcdoc:custom_data`) **MUST** be contained in `extern.mcdoc`.

`extern.mcdoc` **SHOULD NOT** define any named data types.

A datapack **MAY** omit the file `extern.mcdoc` entirely if it would otherwise be empty.

## `function/...`

For individual [developer function](./functions.md#developer-functions) documentation and input/output definitions.

For every [developer function](./functions.md#developer-functions), `<pack ID>:<path...>/<function name>`, a pack defines, there **MUST** exist a mcdoc file at `function/<path...>/<function name>.mcdoc` that:

* **MUST** define structs `In` and `Out` that define the structure of the function's input and output respectively. *(Dispatched to `<function name>` in `<pack ID>:in` and `<pack ID>:out` respectively in [`func.mcdoc`](#funcmcdoc).)*
* **MUST** define data type `Return`, declaring the possible return codes of the function. `Return` **MUST** either be an `enum(int)` or a `type` with value `int @ <min>..<max>`. If the function's return code is unbounded, the range specifier (`@ <min>..<max>`) **MUST** be omitted.
* **MUST NOT** contain any `dispatch` or `inject` statements.
* **SHOULD NOT** define any other named data types unless they are *exclusively* related to the represented function conceptually.
* **SHOULD** contain comment line(s) at the beginning of the file documenting/describing the function.

In comments, function inputs **SHOULD** be referenced by their path within `In` surrounded by `<` and `>` ("<`<input path>`>"); function outputs **SHOULD** be referenced by their path within `Out` surrounded by `>` and `<` (">`<output path>`<").

#### Example

A pack, `foo`, defines the developer function `foo:math/add_numbers`. Mcdoc file `function/math/add_numbers.mcdoc` contains:

```mcdoc
// Adds 2 ints, <a> and <b>.

struct In {
    /// First number to add.
    a: int,
    /// Second number to add.
    b: int,
}

struct Out {
    /// The result of the addition of <a> and <b>.
    result: int
}

/// The result of the addition of <a> and <b>; matches >result<.
type Return = int
```

> Input/output key documentation for such a simple function is somewhat pedantic/verbose but is included for the sake of example.

A datapack **MAY** omit the directory `function` entirely if it does not define any [developer function](./functions.md#developer-functions).

## `abstract/...`

For individual [abstract function](./function_tags.md#abstract-functions) documentation and input/output definitions. *(Very similar to [``function/...`](#function).)*

For every [abstract function](./function_tags.md#abstract-functions), `#<pack ID>:abstract/<path...>/<abstract function name>`, a pack defines, there **MUST** exist a mcdoc file at `abstract/<path...>/<abstract function name>.mcdoc` that:

* **MUST** define structs `In` and `Out` that define the structure of the function's input and output respectively. *(Dispatched to `<abstract function name>` in `<pack ID>:abstract/in` and `<pack ID>:abstract/out` respectively in [`abstract.mcdoc`](#abstractmcdoc).)*
* **MUST** define data type `Return`, declaring the possible return codes of the function. `Return` **MUST** either be an `enum(int)` or a `type` with value `int @ <min>..<max>`. If the function's return code is unbounded, the range specifier (`@ <min>..<max>`) **MUST** be omitted.
* **MUST NOT** contain any `dispatch` or `inject` statements.
* **SHOULD NOT** define any other named data types unless they are *exclusively* related to the represented function conceptually.
* **SHOULD** contain comment line(s) at the beginning of the file documenting/describing the intended function behavior.

In comments, function inputs **SHOULD** be referenced by their path within `In` surrounded by `<` and `>` ("<`<input path>`>"); function outputs **SHOULD** be referenced by their path within `Out` surrounded by `>` and `<` (">`<output path>`<").

#### Example

A pack, `foo`, defines the abstract function `#foo:abstract/do_something`. Mcdoc file `abstract/do_something.mcdoc` contains:

```mcdoc
// Does something with <a> and <b>.

struct In {
    a: int,
    b: any,
}

struct Out {
    /// The result of doing something to <a> and <b>.
    result: int
}

enum(int) Return = {
    Success = 1,
    Fail = 0,
}
```

A datapack **MAY** omit the directory `abstract` entirely if it does not define any [abstract functions](./function_tags.md#abstract-functions).

## `hook/...`

For individual [hook](./function_tags.md#hooks) documentation and pass-data definitions.

For every [hook](./function_tags.md#hooks), `#<pack ID>:hook/<path...>/<hook name>`, a pack defines, there **MUST** exist a mcdoc file at `hook/<path...>/<hook name>.mcdoc` that:

* **MUST** define struct `Hook` that defines the structure of the hook's pass-data. *(Dispatched to `<hook name>` in `<pack ID>:hook` in [`hook.mcdoc`](#hookmcdoc).)*
* **MUST NOT** contain any `dispatch` or `inject` statements.
* **SHOULD NOT** define any other named data types unless they are *exclusively* related to the represented hook conceptually.
* **SHOULD** contain comment line(s) at the beginning of the file documenting/describing the hook--in particular, exactly when it is called (and from what [entrypoint](../slimecore/manifest.md#entrypoints) "timing", if applicable).

In comments, hook pass-data **SHOULD** be referenced by it's path within `Hook` surrounded by `<` and `>` ("<`<pass-data path>`>").

#### Example

A pack, `foo`, defines the hook `#foo:hook/bar`. Mcdoc file `hook/bar.mcdoc` contains:

```mcdoc
// Called when "bar" happens AS and AT the player that did "bar".
// Entrypoint timing 'tick'.
// (Meaning this hook is called within the scope of the function schedule-loop initiated by foo's entrypoint 'tick'.)

struct Hook {
    some_data: int
    other_data: string
}
```
A datapack **MAY** omit the directory `hook` entirely if it does not define any [hooks](./function_tags.md#hooks).

## `_/...`

For all private mcdoc structure/definitions.

From *[Private](./private.md#mcdoc-structure)*:

> All structure defined by mcdoc files in the `_` directory and all subdirectories (`<datapack>/mcdoc/_/**`) [is private].

A datapack **MAY** define any mcdoc files within the `_` directory, or **MAY** omit it entirely.

Mcdoc files within `_` **MUST NOT** `dispatch` or `inject` to public data locations.

Mcdoc files outside of `_` **MUST NOT** reference any structure defined by mcdoc files inside of `_`.

> Generally, private mcdoc structure is used to dispatch `minecraft:storage[<pack ID>:_]`, though this is not required.