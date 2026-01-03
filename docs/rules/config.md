# Pack Config

## Definition

A pack's **config** is all NBT data in the storage location `<pack ID>:config`. Config structure **MUST** be defined and documented via [mcdoc](TODO). Config is intended to provide a standard method of allowing users to configure pack behavior.

Packs **MUST** set default config (for all non-nullable keys) within the scope of their [load tag](../slimecore/special_tags.md#load-tag) when it is called for the *very first* time (installation).

After a pack's installation, it's config **SHOULD NOT** be changed anywhere except for in-game.

While config **MAY** be read anytime by any source, it **SHOULD** be assumed that config is only read directly and cached during [loading](../slimecore/rebuilding.md#loading). *(I.e. it should be assumed that changes to config will not affect pack behavior until the next `/reload`.)*
