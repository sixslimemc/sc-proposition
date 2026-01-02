# Scoreboards

## Objectives
### Public Objectives

Public scoreboard objectives **MUST NOT** have any [variable](#score-variables) scoreholders, only real-entity scoreholders.

*See the [public objective naming specification](./naming.md#scoreboard-objectives).*

### Private Objectives

Private scoreboard objectives (with the exception of the [private register](#private-register)) **SHOULD NOT** have any [variable](#score-variables) scoreholders, only real-entity scoreholders.

*See the [private objective naming specification](./naming.md#scoreboard-objectives).*

### Private Register

The **private register** is a special private scoreboard objective that's name matches a pack's pack ID prefixed by `_` (`_<pack ID>`).

Private registers **SHOULD** hold all of a pack's [variable](#score-variables) scoreholders.

Private registers **MUST NOT** have any real-entity scoreholders.

---

## Score Variables

**Score variables** (commonly referred to as "fake-players") are scoreholders that are not intended to represent an entity or be selectable by a selector.

Score variables **SHOULD** only be defined in [private registers](#private-register) and **MUST NOT** be defined in any [public objectives](#public-objectives).

> TENTATIVE: `#` and `$` prefix are already common within the current datapack community. Current rationale for `*` is that it is easy to spot and does not already have a common primary meaning; `#` is used to start comment lines and `$` is used to prefix macro arguments.

Score variable names **MUST** start with `*`, but otherwise have no other restrictions.