# Public Functions

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



The following rules apply for every public function, `<datapack>/data/<pack id>/function/<path...>/<function name>.mcfunction`, a datapack defines.

Datapacks **MUST** include an mcdoc file with path `<datapack>/mcdoc/function/<path...>/<function name>.mcdoc`.
