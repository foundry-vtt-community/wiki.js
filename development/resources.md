---
title: Development Resources
description: 
published: true
date: 2025-07-28T16:57:32.559Z
tags: development, resource, tutorial, template, macro
editor: markdown
dateCreated: 2020-09-18T21:54:56.070Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

> Many old links have been removed from this page due to their content not being updated to modern Foundry.
{.is-warning}


## Official API
Foundry has a Developer API Documentation section on the official `foundryvtt.com` website [here](https://foundryvtt.com/api/), generated from the JSDoc comments within every local installation of Foundry.

You have two options for perusing the source code within `resources/app`:
- The subfolders `common` and `client` break the classes up into individual files. Classes inside `common` use [JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules), so for example the base `Document` class is bundled and available at `foundry.abstract.Document` in your client.
Since v13, that is also true for `client` classes. Before v13, `client` classes were all thrown directly into the global scope (with a `client-esm` folder containing ones that were already converted to the modular approach).
- Alternatively, `public/scripts/foundry.js` is the entire script as it's delivered to end users; note that it has some external imports for the esmodules content.

It is advisable to open this file up and search it in your IDE, as all of the JSDoc comments are present and you can easily see what is really going on in the code.

## [Knowledge Base](https://foundryvtt.com/kb/)
- [Introduction to Development](https://foundryvtt.com/article/intro-development/)
- [Introduction to Module Development](https://foundryvtt.com/article/module-development/)
- [Introduction to System Development](https://foundryvtt.com/article/system-development/)
- [Introduction to System Data Models](https://foundryvtt.com/article/system-data-models/)


## Official Discord Server
The official [Discord](https://discord.gg/foundryvtt) is a great place to get up-to-date information and support. If you are looking for help, you might want to ask in the following channels:
- `#macro-polo` for help with macros;
- `#module-development` for help with developing your own module;
- `#system-development` for help with developing your own system.

## Tutorials

### [Boilerplate System Development Tutorial](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD01-Getting-started)
Wiki hosted guide to accompany and explain the Boilerplate system as well as a lot of core system development concepts.
[Boilerplate](https://gitlab.com/asacolips-projects/foundry-mods/boilerplate) is a lightly opinionated boilerplate system that can be used as a starting point to build a custom system.

> The main Boilerplate branch and parts of the Boilerplate tutorial could be considered slightly outdated at the time of writing. Consider using the `v12` branch of the Boilerplate system, and [Data Models](/en/development/api/DataModel) instead of `template.json`.
{.is-warning}

## Development Tools

### [foundry-vtt-types](https://github.com/League-of-Foundry-Developers/foundry-vtt-types)
- A community effort to cover all of Foundry’s API with TypeScript type definitions.

### [Quench - End to End testing within Foundry](https://github.com/Ethaks/FVTT-Quench)
- Allows devs to register end to end tests powered by Mocha (and also includes Chai), which are then run in-game in a native Foundry Application.
