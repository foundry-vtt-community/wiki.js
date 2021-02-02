---
title: Library Modules
description: A quick list of known library modules which offer functionality for other modules to extend.
published: true
date: 2021-02-02T20:27:32.449Z
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

### Commonly reused packages
- Mathjs https://github.com/League-of-Foundry-Developers/mathjs-lib
- Codemirror https://github.com/League-of-Foundry-Developers/codemirror-lib
- Introjs https://github.com/League-of-Foundry-Developers/intro.js-lib
- Sortablejs https://github.com/League-of-Foundry-Developers/sortablejs-lib


### Foundry Specific Functionality
- Chat Commands https://foundryvtt.com/packages/_chatcommands/

