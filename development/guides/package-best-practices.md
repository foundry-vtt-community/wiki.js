---
title: Package Development Best Practices Checklist
description: A short checklist for module developers with best practices as discovered by the community.
published: true
date: 2021-08-24T13:40:44.872Z
tags: localization, development, guide, manifest, code, files, paths
editor: markdown
dateCreated: 2020-11-12T14:02:50.522Z
---


This article was last updated for Foundry VTT 0.8.7.

## Releases and Updates

For more details about how Foundry VTT installs and updates packages, see the full article [Package Releases and Version History](/en/development/guides/releases-and-history).

### `version`
- Use a `string` for the version number instead of a `float` because for example `0.9` would be superior to `0.10` if using floats.
- Use [Semantic Versioning](https://semver.org/).
- Every change, even something that only changes the manifest and does not change the package contents, should increment something in the version number.

> You should never have two different versions of your package with the exact same version number.
{.is-danger}

### `manifest`
- A package's `manifest` URL should use a stable url that always points at the Latest manifest JSON.
- A package's `manifest` URL should be Raw JSON or a download link, not the github html view of the JSON.

> ### Bad Examples
> `https://github.com/ElfFriend-DnD/foundryvtt-compactBeyond5eSheet/blob/master/src/module.json`
> This is a link to the HTML view of the module.json
>
> `C:\Users\elffriend\moduleDev\foundryvtt-compactBeyond5eSheet\src\module.json`
> This is a local file on your machine.
{.is-warning}

> ### Good Examples
> `https://github.com/ElfFriend-DnD/foundryvtt-compactBeyond5eSheet/releases/latest/download/module.json`
> Links to the latest release's `module.json` artifact. This link will never change even if the module's files move around.
>
> `https://raw.githubusercontent.com/ElfFriend-DnD/foundryvtt-compactBeyond5eSheet/master/src/module.json`
>  ⚠️ This url will break if the module's files are rearranged, but apart from that it is stable.
{.is-success}

### `download`
- The package `download` URL should point to a specific zip that matches the version.
- Package manifest should never download the "latest" zip (e.g. a zip of the current `master` branch) but rather each version's module.json `manifest` url would download that specific version.

## Localization

> There's a lot of nuance to localization that can't be summed up quickly. Take a look at the dedicated [Localization Best Practices](/en/development/guides/localization/localization-best-practices) article for more in depth best practices.
{.is-info}

- Don't hardcode your strings, use localization **right from the start**. It is easier to localize from the start rather than going back through at the end.
- Make use of as many existing strings as possible.
- Keep your localization strings confined to your package's namespace.

## Files
- Paths to your files use the exact same case-sensitive directories as in the package download zip. Windows, Linux, and OSX treat capitalization of directories differently.
- Do not use spaces in file names or directory names. All directories should be URL-compatible. We recommend using `kebab-case` where your spaces are replaced with `-`.
- Never do a relative import (esmodule) from one module to another (or to a system). If you need access to something, contact the developer and ask them to expose it instead.
- Remember that other people host foundry differently, including on setups which change the root directory.

## Module APIs and Dependencies
- Expose module-specific APIs on the module's moduleData located at `game.modules.get('my-module-name')?.api`.
- Leverage custom hooks to provide a reliable way for other packages to react to events caused by your module.
- Depend on every module in the dependency tree for your package, Foundry does not handle dependency-trees with 2+ levels during install or activation.

## Code Practices
This section was last updated for Foundry VTT 0.7.7.

- The [`libWrapper` library module](https://github.com/ruipin/fvtt-lib-wrapper) is an excellent dependency to aid in the "patching" of core functions.
- Foundry VTT core has a keyboard manager class accessible `game.keyboard`. It is recommended to leverage this when your module needs to know the state of the keyboard.

## Overwriting Foundry VTT Core Behavior
- Code as defensively as possible when writing a module which aims to overwrite or replace Core functionality/logic. If something causes an error in your logic, it would be best to implement a fallback to the core behavior you are replacing.

From Atropos:
> The risk of this kind of failure will always be higher for modules which fundamentally exist for the purpose of overwriting core functions to behave differently in ways that are not allowed by the supported API. Not to pick on the "Perfect Vision" module - but it excavates most of the vision/lighting rendering of Foundry VTT and replaces it with different logic. A module like this is extremely vulnerable to all sorts of breaks in ways that I cannot possibly account for when I am making development decisions.
>
> If you're writing a module that carves out the guts of a core Foundry VTT function and replaces it with entirely different logic - it would be prudent to have some way to abort or fall-back to the core logic in cases where your overridden logic fails

## UI Practices

### Sidebar Tab Action Buttons
When adding buttons to the Heading of a Sidebar Tab, append your buttons to the `.actions-buttons` div and not to the `.directory-header` div.

```js
Hooks.on('renderJournalDirectory', (app, html, data) => {
    const actionButtons = html.find('.action-buttons');
    const myButton = '<button>My Button</button>';
    actionsButton.append(myButton);
});
```

By default, this will make all buttons in the header squish down to hide their text before wrapping, if you wish to avoid that behavior, add some CSS to `.actions-buttons button` which forces their `min-width` to be `max-content`, for bonus compatibility with some systems, also add `white-space: nowrap`.

```css
.actions-buttons button {
    min-width: max-content;
    white-space: nowrap;
}
```

