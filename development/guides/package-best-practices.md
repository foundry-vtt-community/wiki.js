---
title: Package Development Best Practices Checklist
description: A short checklist for module developers with best practices as discovered by the community.
published: true
date: 2020-11-19T16:46:01.321Z
tags: 
editor: markdown
dateCreated: 2020-11-12T14:02:50.522Z
---

# Releases and Updates

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

- Don't hardcode your strings, use localization right from the start. It is easier to localize from the start.
- Make use of as many existing strings as possible.
- Keep your localization strings confined to your package's namespace.

# Files and Dependencies
- Paths to your files use the exact same case-sensitive directories as in the download zip. Windows, Linux, and OSX treat capitalization of directories differently.
- Do not use spaces in file names or directory names.
- Never do a relative import (esmodule) from one module to another (or to a system). If you need access to something, contact the developer and ask them to expose it instead.
- A dependency on another module should be resolved by `CONFIG` variables defined by that dependency or namespaced classes or Hooks.
