# Introduction

## What?

The SC proposition describes a specification/framework for Minecraft datapacks and their content.

## Why?

The capability and community of Minecraft datapack-ing is growing; The standards of datapack inter-operability are lagging far behind. While general good practices have been developed and shared informally, there is no community consensus or formal documentation of standards.

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

## Prerequisites

General intermediate-level knowledge of Minecraft commands and datapacks is assumed. Familiarity with SNBT data manipulation via commands, function execution scope/order, and datapack structure are likely required for full understanding of the specification.

Basic knowledge of [mcdoc](https://spyglassmc.com/user/mcdoc) (primarily type definitions and dispatching) is required for compliance with rules described in the [Mcdoc](./rules/mcdoc.md) page.

## Get Started

It is recommended to read the [Terminology](./terminology.md) page before reading further pages.

A first-time reader may have the "smoothest" experience reading the pages of the SC proposition as they appear on the left sidebar, and it designed as such; however, this is completely optional.
