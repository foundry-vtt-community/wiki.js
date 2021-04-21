---
title: Library Modules
description: A quick list of known library modules which offer functionality for other modules to extend.
published: true
date: 2021-04-21T15:03:48.133Z
tags: 
editor: markdown
dateCreated: 2021-02-02T19:37:27.342Z
---

## What is a library module?

A library module is a module which itself does not offer any functionality for a user, rather provides an API which other modules can leverage.

## Conventions for Creating a Library Module

### Keep the API stable
Follow [semver](https://semver.org/) and avoid breaking the API if possible.

### `name` should start with an underscore
This is the best we can do for pushing these library modules to the top of the initialization stack.

### Manifest+ `library`
Leverage the Manifest+ field [`library`](https://foundryvtt.wiki/en/development/manifest-plus#library).

## Known Library Modules

### Javascript Library packages
Rather than including these js libs in your own module's files, you can make use of these commonly re-used packages.

- Mathjs https://github.com/League-of-Foundry-Developers/mathjs-lib
- Codemirror https://github.com/League-of-Foundry-Developers/codemirror-lib
- Introjs https://github.com/League-of-Foundry-Developers/intro.js-lib
- Sortablejs https://github.com/League-of-Foundry-Developers/sortablejs-lib


### Foundry Specific Functionality

More than simply wrapping an existing js lib, these are made specifically with foundry's use cases in mind.

- libWrapper https://github.com/ruipin/fvtt-lib-wrapper - Useful for overwriting methods you don't control in a way which allows for conflict resolution.
- Chat Commands https://foundryvtt.com/packages/_chatcommands/ - Allows modules to define their own chat commands.
- Color Settings https://github.com/ardittristan/VTTColorSettings - Adds color picker as settings option and form option.
- Find the Path https://github.com/dwonderley/lib-find-the-path/ - Provides a library that performs system-agnostic path planning calculations for two-dimensional, square grids.
- DF's Hotkeys https://foundryvtt.com/packages/lib-df-hotkeys/ - It allows modules to register their own Keyboard Shortcuts and gives way for users to then customize those hotkey bindings.




