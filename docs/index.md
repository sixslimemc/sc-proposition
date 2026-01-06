# Introduction

## What?

The SC proposition describes a specification/standard for Minecraft datapacks and their content.

## Why?

The cability and community of Minecraft datapack-ing is growing; The standards of datapack inter-operability are lagging far behind. While general "good practices" have been developed and shared informally, there is no community consensus or formal documentation of standards.

Additionally, datapack manifests (`pack.mcmeta`) do not support many fundamental features that software package manifests often have, such as dependencies, versioning, metadata, etc. Datapack loading is managed in an ad-hoc manner in-game via the `/datapack` command; this is very fragile and dependent on the user's knowledge of installed datapacks.

More often than not, these issues lead to many datapacks that are silently incompatible with eachother, general avoidance of using datapacks created by other people, and an incentive to make datapacks that "do it all" as opposed to modularity.

## How?

The SC proposition aims to fix as many of these issues as possible by clearly defining and documenting a set of rules that datapacks must follow.

These rules are designed to be:

* **Implementable with no third party tools.** Any developer writing a datapack "by hand" should be able to comply with these rules without unreasonable effort.
* **Unobtrusive.** Compliance with these rules should not unnecessarily conflict with a developer's desired development practices.
* **Effective.** Compliance with these rules should "just work".

The SC proposition requires all worlds to have a loader datapack ([SlimeCore](./slimecore/index.md)) installed; issues related to datapack manifests/loading would be impossible to address without this requirement.

---

## Get Started

It is recommended to look over the [Terminology](./terminology.md) page before reading any other pages.

The [Overview](./overview.md) page provides a brief but complete summary of this specification.

