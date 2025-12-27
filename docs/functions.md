# Public Functions

## Definition
Public functions in the `-` directory of the `function` registry (`data/<pack id>/function/-/**`) are **user functions**. All other public functions are **developer functions**.

## User Functions

User functions **SHOULD** provide intended functionality when called directly in-game (via player and/or command blocks) and **SHOULD NOT** be called by functions.

### Inputs

Dedicated function inputs **MUST** be provided via macro arguements.

### Outputs

Aside from the return code/value, all user function output/work is, formally speaking, a side-effect.

> If a user function must have output, a common pattern is to set data in `<pack id>:data`.

## Developer Functions

Developer function **SHOULD** provide intended functionality when called from functions and **SHOULD NOT** be called directly in-game.

### Inputs

For every developer function (`<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`), dedicated function input(s) **MUST** be provided via storage data using the `<pack id>:in` storage location, with the storage path `<function name>.<input name>`.

Input data **MUST** be removed/cleared before the function ends; i.e. `<pack id>:in <function name>` **MUST** be empty after the function returns.

### Outputs

For every developer function (`<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`), dedicated function output(s) **MUST** be provided via storage data using the `<pack id>:out` storage location, with the storage path `<function name>.<output name>`.

### Mcdoc Documentation

> This section is particularly tentative, as [Mcdoc](https://spyglassmc.com/user/mcdoc/) is not a finished project.

For every developer function (`<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`), an mcdoc file **MUST** exist at filepath `<datapack>/mcdoc/function/<path...>/<function name>.mcdoc`.

Function mcdoc files **MUST** define structs named **In** and **Out**, defining the structure of the function's input(s) and output(s) respectively. Keys in these structs **SHOULD** be documented with documentation comments.

Function mcdoc files **MUST** contain comment(s) that describe the function at the very beginning of the file.

Function mcdoc files **MAY** define additional types/structs/etc., but **SHOULD NOT** unless they are exclusively related to that particular function's input/output.

Comments that reference inputs and/or outputs **SHOULD** refer to inputs by the path of the input in **In**, surrounded by `<` and `>`, and refer to outputs by the path of the output in **Out**, surrounded by `>` and `<`.

#### Example

For a function `foo:math/add_numbers`, the file `foo/mcdoc/function/math/add_numbers.mcdoc` exists with the following content.

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
```

> It is worth noting that writing documentation comments for every input/output for such a simple function is somewhat pedantic/verbose, but included for the sake of example. Practically, this function really only needs the description comment at the top of the file.

## Mcdoc Dispatch

If a datapack defines any developer functions, it **MUST** include an mcdoc file at filepath `<datapack>/mcdoc/func.mcdoc` that defines exactly 2 structs, **In** and **Out**, dispatching them to `minecraft:storage[<pack id>:in]` and `minecraft:storage[<pack id>:out]` respectively.

For every developer function (`<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`), a key with name `<function name>` must be defined in both the the **In** and **Out** structs, with type `super::function::<path...>::<function name>::In` or `super::function::<path...>::<function name>::Out` respectively.

If multiple functions have the same top-level name (`<function name>`), the `<function name>` key in these structs **MUST** be a union of all matching function **In**/**Out** types.

#### Example

A datapack defines 3 developer functions, `foo:thing_one/do`, `foo:thing_two/do` and `foo:get`. An mcdoc file at `foo/mcdoc/func.mcdoc` exist with the following content.

```mcdoc
dispatch minecraft:storage[foo:in] to struct In {
    do: (super::function::thing_one::do::In | super::function::thing_two::do::In),
    get: super::function::get::In,
}

dispatch minecraft:storage[foo:out] to struct Out {
    do: (super::function::thing_one::do::Out | super::function::thing_two::do::Out),
    get: super::function::get::Out,
}
```