# Owned Objects

## Definition
Some in-world objects (items, entities) can be marked as **owned**, indicating that they **SHOULD NOT** be treated like default/standard objects, but can still be interacted with by any datapack. Generally, this indicates a "custom" object.

## Declaring Owned Objects

The following describes owned object declarations.

### Items
Items with `-:true` in their `minecraft:custom_data` component.

### Entities
Entities with the '`-`' entity tag, excluding players.

## Handling Owned Objects

The handling of owned objects is somewhat subjective, but in general, owned objects **SHOULD NOT** be treated as standard objects, but **SHOULD** still be recognized as objects of their type.

#### Examples

* A datapack that converts `stone_pickaxe`s to `copper_pickaxe`s **SHOULD NOT** convert owned `stone_pickaxe`s.
* A datapack that adds item transport pipes **SHOULD** transport owned items the same as regular items.
* A datapack that increases the health of zombies **SHOULD NOT** increase the health of owned zombies.
* A datapack that adds a weapon that knocks back mobs **SHOULD** knock back owned mobs.

It is important to note the difference between **private** and **owned**: **Private** objects **MUST NOT** be interacted with at all (other than by the defining datapack); **owned** objects **MAY** be interacted with, but should not be treated as *standard* objects.




