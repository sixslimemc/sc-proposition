# Public Functions

## Definition

Public functions are split into 2 categories, **user functions** and **developer functions**.

Public functions in the `-` directory of the `function` registry (`data/<pack id>/function/-/**`) are user functions. All other public functions are developer functions.

## User Functions

User functions **SHOULD** provide intended functionality when called directly in-game (via player and/or command blocks) and generally **SHOULD NOT** be called by other functions.

### Inputs

Dedicated user function inputs **MUST** be provided via macro arguements.

### Outputs

Aside from the return code/value, all user function output/work is, formally speaking, a side-effect.

> If a user function must have output, a common pattern is to set data in `<pack id>:data`.

## Developer Functions

Developer functions **SHOULD** provide intended functionality when called from functions and generally **SHOULD NOT** be called directly in-game.

### Inputs

For every developer function (`<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`), dedicated function input(s) **MUST** be provided via storage data using the `<pack id>:in` storage location, with the storage path `<function name>.<input name>`.

Input data **MUST** be removed/cleared before the function ends; i.e. `<pack id>:in <function name>` **MUST** be empty after the function returns.

### Outputs

For every developer function (`<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`), dedicated function output(s) **MUST** be provided via storage data using the `<pack id>:out` storage location, with the storage path `<function name>.<output name>`.

### Return Codes
Non-zero positive valued return codes for developer functions **SHOULD** represent success, while 0 or negative values **SHOULD** represent failure.

Developer functions **MAY** not have any meaningful return codes (e.g. a function cannot fail, a function is very simple, etc.), in which case they **SHOULD** return 1 in all cases.

### Example

The following is an example developer function, `foo:do_something`:

```
#> foo:do_something

# clear outputs
data remove storage foo:out do_something

# call main implementation body
function foo:_/impl/do_something/main

# clear inputs
data remove storage foo:in do_something

return 1
```

## Mcdoc Documentation

> TENTATIVE: [Mcdoc](https://spyglassmc.com/user/mcdoc/) is not a finished project.

For every public function (`<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`), an mcdoc file **MUST** exist at filepath `<datapack>/mcdoc/function/<path...>/<function name>.mcdoc`.

### User Functions

> TODO

### Developer Functions

#### In & Out
Developer function mcdoc files **MUST** define structs named **In** and **Out**, declaring the structure of the function's input(s) and output(s) respectively.

#### Return Code
Developer function mcdoc files **MUST** define **Return**, declaring the possible return codes of the function. Return **MUST** either be an `enum(int)` or a `type` with value `int @ <min>..<max>`. If the function's return code is unbounded, the range specifier (`@ <min>..<max>`) **SHOULD** be omitted.

#### Additional Definitions
Function mcdoc files **MAY** define additional types/structs/etc., however, these additional definitions **SHOULD** be *exclusively* related to that particular function's input/output.

#### Comments

Developer function mcdoc files **MUST** contain comment lines at the beginning of the file that describe/document the represented function.

All types **SHOULD** be properly documented/described with documentation comments.

Comments that reference inputs and/or outputs **SHOULD** refer to inputs by the path of the input in **In**, surrounded by `<` and `>`, and refer to outputs by the path of the output in **Out**, surrounded by `>` and `<`.

#### Example

The following is an example mcdoc file that would be located at `foo/mcdoc/function/math/add_numbers.mcdoc`, representing the function `foo:math/add_numbers`:

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

/// The result of the addition of <a> and <b>.
type Return = int
// It would also be acceptable to simply return 1 because the result is already stored in >result<.

```

> Input/output key documentation for such a simple function is somewhat pedantic/verbose but is included for the sake of example.

### Developer Function Dispatch

If a datapack defines any developer functions, it **MUST** include an mcdoc file at filepath `<datapack>/mcdoc/func.mcdoc` that defines exactly 2 structs, **In** and **Out**, dispatching them to `minecraft:storage[<pack id>:in]` and `minecraft:storage[<pack id>:out]` respectively.

For every developer function (`<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`), a key with name `<function name>` must be defined in both the the **In** and **Out** structs, with type `super::function::<path...>::<function name>::In` or `super::function::<path...>::<function name>::Out` respectively.

If multiple functions have the same top-level name (`<function name>`), the `<function name>` key in these structs **MUST** be a union of all matching function **In**/**Out** types.

#### Example

The following is an example mcdoc file that would be located at `foo/mcdoc/func.mcdoc`, where the pack `foo` has 3 developer functions, `foo:thing_one/do`, `foo:thing_two/do` and `foo:get`:

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