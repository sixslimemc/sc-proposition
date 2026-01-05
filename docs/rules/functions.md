# Public Functions

## Definition

Public functions are split into 2 categories, **user functions** and **developer functions**.

Public functions in the `-` directory of the `function` registry (`data/<pack ID>/function/-/...`) are user functions while all others are developer functions.

## User Functions

User functions **SHOULD** provide intended functionality when called directly in-game (via player and/or command block) and generally **SHOULD NOT** be called by other functions.

### Inputs & Outputs

User function dedicated inputs **MUST** be provided via macro arguements.

User functions do not have dedicated outputs; all outputs are, formally speaking, a side-effect.

> If a user function must have output, a common pattern is to set data in `<pack ID>:data`.

### Return Codes
Meaningful information/output **MAY** be conveyed through a user function's return code.

## Developer Functions

Developer functions **SHOULD** provide intended functionality when called from functions and generally **SHOULD NOT** be called directly in-game.

### Inputs

Given a developer function, `<pack ID>:<path...>/<function name>`, dedicated function input(s) **MUST** be specified through a struct at path `<function name>` (i.e. `<function name>.<input name>`) in the NBT storage location `<pack ID>:in`.

Input data **MUST** be removed/cleared before the function ends within the function's scope; i.e. `<pack ID>:in <function name>` **MUST** be empty after the function returns.

For functions calling developer functions, it **MUST** be garunteed that no external functions are called between the time input data is set and the developer function is called.

### Outputs

Given a developer function, `<pack ID>:<path...>/<function name>`, dedicated function output(s) **MUST** be specified through a struct at path `<function name>` (i.e. `<function name>.<output name>`) in the NBT storage location `<pack ID>:out`.

### Return Codes
Non-zero positive valued return codes for developer functions **SHOULD** represent a form of success, while 0 or negative values **SHOULD** represent a form of failure.

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
