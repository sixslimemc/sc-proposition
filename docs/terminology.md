# Terminology

**Access** (verb) - To reference or use in *any way*. 

**Content** (noun) - A general term for anything that can be introduced or changed by a datapack.

**Define** (verb) - To introduce something that does not exist otherwise--usually in the context of structured data or resources.

**Namespace** (noun) - A datapack [namespace](https://minecraft.wiki/w/Identifier#Namespaces). I.e. refers to the name of a 1st level subdirectory of the `data` directory in a datapack (`<datapack name>/data/<namespace>`).

**Owned** (adjective) - *See [Owned Objects](./rules/owned.md).*

**Pack** (noun) - Somewhat interchangeable term for "datapack"--tends to be used in the context of datapack content, while "datapack" tends to be used in the context of datapack files/directories.

**Pack ID** (noun) - *See [Pack ID](./slimecore/manifest.md#pack_id-and-author_id)*. Interchangeable with "primary namespace".

**Primary Namespace** (noun) - *See [Primary Namespace](./rules/primary_namespace.md).*

**Private** (adjective) - *See [Private Content](./rules/private.md).*

**Public** (adjective) - *See [Private Content](./rules/private.md).*

**Registry** (noun) - A 2nd level subdirectory of the `data` directory in a datapack (`<datapack name>/data/<namespace>/<registry>`).

**Resource** (noun) - A file within a registry or registry tag.

**Scope** (noun) - All possible commands that could possibly be executed as a direct result (i.e. would be limited by [max_command_sequence_length](https://minecraft.wiki/w/Game_rule)) of a function's/function tag's execution. 

---

The use of uppercase bolded words such as **MUST**, **SHOULD**, **MAY**, etc. are in adherence to [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119).

Code-block text surrounded by angle brackets (`<` and `>`) conveys that it should be replaced by what is inside the angle brackets conceptually. For example, in the context of a pack with pack ID `foo`, `<pack ID>` means `foo`.