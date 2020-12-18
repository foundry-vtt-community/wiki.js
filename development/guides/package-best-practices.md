---
title: Package Development Best Practices Checklist
description: A short checklist for module developers with best practices as discovered by the community.
published: true
date: 2020-12-18T16:03:18.524Z
tags: 
editor: markdown
dateCreated: 2020-11-12T14:02:50.522Z
---

# Releases and Updates
This section was last updated for Foundry Core 0.7.7.

For more details about how Foundry installs and updates packages, see the full article [Package Releases and Version History](/en/development/guides/releases-and-history).

## `version`
- Use a `string` for the version number instead of a `float` because for example `0.9` would be superior to `0.10` if using floats.
- Use [Semantic Versioning](https://semver.org/).
- Every change, even something that only changes the manifest and does not change the package contents, should increment something in the version number.

> You should never have two different versions of your package with the exact same version number.
{.is-danger}


## `manifest`
- A package's `manifest` URL should use a stable url that always points at the Latest manifest JSON.
- A package's `manifest` URL should be Raw JSON or a download link, not the github html view of the JSON.

> ### Bad Examples
> `https://github.com/ElfFriend-DnD/foundryvtt-compactBeyond5eSheet/blob/master/src/module.json`
> This is a link to the HTML view of the module.json
>
> `C://Users/elffriend/moduleDev/foundryvtt-compactBeyond5eSheet/src/module.json`
> This is a local file on your machine.
{.is-warning}


> ### Good Examples
> `https://github.com/ElfFriend-DnD/foundryvtt-compactBeyond5eSheet/releases/latest/download/module.json`
> Links to the latest release's `module.json` artifact. This link will never change even if the module's files move around.
>
> `https://raw.githubusercontent.com/ElfFriend-DnD/foundryvtt-compactBeyond5eSheet/master/src/module.json`
>  ⚠️ This url will break if the module's files are rearranged, but apart from that it is stable.
{.is-success}

## `download`
- The package `download` URL should point to a specific zip that matches the version.
- Package manifest should never download the "latest" zip (e.g. a zip of the current `master` branch) but rather each version's module.json `manifest` url would download that specific version.

# Localization

> There's a lot of nuance to localization that can't be summed up quickly. Take a look at the dedicated [Localization Best Practices](/en/development/guides/localization-best-practices) article for more in depth best practices.
{.is-info}

- Don't hardcode your strings, use localization right from the start. It is easier to localize from the start rather than going back through at the end.
- Make use of as many existing strings as possible.
- Keep your localization strings confined to your package's namespace.

# Files and Dependencies
- Paths to your files use the exact same case-sensitive directories as in the download zip. Windows, Linux, and OSX treat capitalization of directories differently.
- Do not use spaces in file names or directory names. All directories should be URL-compatible. We recommend using `kebabCase` where your spaces are replaced with `-`.
- Never do a relative import (esmodule) from one module to another (or to a system). If you need access to something, contact the developer and ask them to expose it instead.
- A dependency on another module should be resolved by `CONFIG` variables defined by that dependency or namespaced classes or Hooks.

# Code Practices
This section was last updated for Foundry Core 0.7.7.

- The [`libWrapper` library module](https://github.com/ruipin/fvtt-lib-wrapper) is an excellent dependency to aid in the "patching" of core functions.
- Foundry Core has a keyboard manager class accessible `game.keyboard`. It is recommended to leverage this when your module needs to know the state of the keyboard.
- Consider using [WeakMaps](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap) when you need to attach additional properties to core objects that don't have flags.

# Overwriting Foundry Core Behavior
- Code as defensively as possible when writing a module which aims to overwrite or replace Core functionality/logic.

From Atropos:
> The risk of this kind of failure will always be higher for modules which fundamentally exist for the purpose of overwriting core functions to behave differently in ways that are not allowed by the supported API. Not to pick on the "Perfect Vision" module - but it excavates most of the vision/lighting rendering of Foundry VTT and replaces it with different logic. A module like this is extremely vulnerable to all sorts of breaks in ways that I cannot possibly account for when I am making development decisions.
>
> If you're writing a module that carves out the guts of a core Foundry function and replaces it with entirely different logic - it would be prudent to have some way to abort or fall-back to the core logic in cases where your overridden logic fails

