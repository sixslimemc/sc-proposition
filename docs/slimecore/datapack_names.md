# Datapack Naming

## Standard Format

All datapack names (name of folder in world's `datapack` directory) **SHOULD** conform to one of the following formats:

---

#### Authored and Versioned

`<author ID>.<pack ID>.<major ver>.<minor ver>.<patch ver>`

*Example: `sixslime.foo.1.2.3`*

#### Authored and Unversioned

`<author ID>.<pack ID>`

*Example: `sixslime.foo`*

#### Unauthored and Versioned

`<pack ID>.<major ver>.<minor ver>.<patch ver>`

*Example: `foo.1.2.3`*

#### Unauthored and Unversioned

`<pack ID>`

*Example: `foo`*

---

Fully released datapacks **SHOULD** match the [Authored and Versioned](#authored-and-versioned) format; all other formats are intended for datapacks that are still in development stages.

## Non-Standard Names

For every datapack with a name that does not match [standard format](#standard-format), NBT path `datapack_path_overrides.<pack ID>` in storage location `slimecore:config` **MUST** contain their datapack path as a string.

For example, if a datapack has the name `Non Standard foo` and it's [manifest](./manifest.md) declares the pack ID `foo`, then `datapack_path_overrides.foo` in `slimecore:config` would contain the string `file/Non Standard foo`.

SlimeCore will fail to [rebuild](./rebuilding.md#rebuilding) if non-standard datapack names are not specified this way. *(This only applies to datapacks that follow the SC spec.)*

