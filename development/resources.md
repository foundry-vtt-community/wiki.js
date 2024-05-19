---
title: Development Resources
description: 
published: true
date: 2024-05-19T06:20:05.207Z
tags: development, resource, tutorial, template, macro
editor: markdown
dateCreated: 2020-09-18T21:54:56.070Z
---

![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

> Many old links have been removed from this page due to their content not being updated to modern Foundry.
{.is-warning}


# Official API
Atropos has a Developer API Documentation section on the official `foundryvtt.com` website [here](https://foundryvtt.com/api/), generated from the JSDoc comments within every local installation of Foundry. You m

You have two options for perusing the source code within `resources/app`:
- The subfolders `common` and `client` break the classes up into individual files. Classes inside `common` use [JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules), so for example the base `Document` class is bundled and available at `foundry.abstract.Document` in your client. By contrast, the classes within `client` are all thrown directly into the global scope
- Alternatively, `public/scripts/foundry.js` is the entire script as it's delivered to end users; note that it has some external imports for the esmodules content.

It is advisable to open this file up and search it in your IDE, as all of the JSDoc comments are present and you can easily see what is really going on in the code.

## Macros and Code examples

### [Community Macros](https://github.com/foundry-vtt-community/macros)
- Community contributed macros that can be downloaded as a module, a wide range of topics with pf1, pf2, and dnd5e system specific macros covered therein.

## Tutorials

### [Boilerplate System Development Tutorial](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD01-Getting-started)
- Wiki hosted guide to accompany and explain the Boilerplate system as well as a lot of core system development concepts.

## Templates and Starter Kits

There are a variety of community-created module and system starter kits out there. These range from very opinionated to barely opinionated.


### Basics for Modules

#### [League Basic JS Module Template](https://github.com/League-of-Foundry-Developers/FoundryVTT-Module-Template)
- Lightly Opinionated
- **Supports:** Javascript
- **Description:** A template repository for FoundryVTT module development that comes with versioned CI/CD.


### Generators

### System Development

#### [Boilerplate System](https://gitlab.com/asacolips-projects/foundry-mods/boilerplate)
- Lightly Opinionated
- **Supports:** Javascript, SCSS
- **Description:** This system is a boilerplate system that you can use as a starting point for building your own custom systems. It's similar to Simple World-building, but has examples of creating attributes in code rather than dynamically through the UI.


### Javascript for Modules

#### [TyphonJS's Demo Rollup Module](https://github.com/typhonjs-fvtt/demo-rollup-module)
- Very Opinionated
- Uses Rollup to bundle your module with examples of cleanly including node module libraries into final package
- **Supports:** Javascript, Sass / SCSS / PostCSS, minification / mangling w/ sourcemaps, and auto creates versions using Github Actions.
- **Description:** A starter project template demonstrating Rollup to bundle your module code regardless if you plan to include Node modules or not.


### Typescript for Modules

#### [League Basic TS Module Template](https://github.com/League-of-Foundry-Developers/foundry-typescript-template)
- Lightly Opinionated
- **Supports:** Typescript
- **Description:** A template for typescript projects that uses manifest+ and auto creates versions using Github Actions.


#### [TS Hot Module Replacement Template](https://github.com/Blackcloud010/FoundryVTT-Module-Template-Hotswap)
- Lightly Opinionated
- **Supports:** Typescript, Javascript, SCSS, ESLint
- **Contains:** Bundler and Hot Module Replacement through webpack.
- **Description:** A template for creating foundry modules using typescript. Project files are setup to be hot reloaded.


#### [DJ Addi's VSCode Debugging Enabled Template](https://gitlab.com/DJ.Addi/fvtt-project-template)
- Basically no Opinions
- Has VS Code Debugger integration out of the box
- **Description:** A bare bones template that plugs into the debugger features of Visual Studio Code.


#### [Dragon Flagon TS Automated Module Template](https://github.com/flamewave000/fvtt-module-template)
- Lightly Opinionated
- This module template has an opinionated folder structure, but otherwise nothing else.
- Fully Automated building, development watching, and bundling of the module.
- **Uses**: Gulp and TypeScript
- **Supports**: JavaScript, TypeScript, SourceMaps, Automated Bundling (.zip and manifest generation)
- **Description**: A TypeScript+Gulp template for a more automated FoundryVTT module development. In this system, you perform minimal changes to the module.json as it is populated automatically based on the package.json and the files present. Will allow you to run a series of Gulp watchers that will automatically build/update files to your dev environment whenever you save a file you've changed.


## Development Tools

### [foundry-vtt-types](https://github.com/kmoschcau/foundry-vtt-types)
- A community effort to cover all of Foundry’s API with TypeScript type definitions. At the moment, there are type definitions for versions `0.7.x`, `0.8.x`, and `v9`. Work on support for `v10` is ongoing.


### [TyhponJS's @eslint/foundry](https://www.npmjs.com/package/@typhonjs-fvtt/eslint-config-foundry.js)
- A plug and play shareable eslint configuration package containing all exported globals from `foundry.js` that when combined w/ the `no-shadow` rule prevents overwriting core Foundry VTT functionality. Useful for module / system development
- [Demo of it in action](https://github.com/typhonjs-fvtt/demo-rollup-module/blob/main/.eslintrc)


### [Quench - End to End testing within Foundry](https://github.com/Ethaks/FVTT-Quench)
- Allows devs to register end to end tests powered by Mocha (and also includes Chai), which are then run in-game in a native Foundry Application.


### [Development Mode](https://github.com/League-of-Foundry-Developers/foundryvtt-devMode)
- Provides an API for packages to register and read debug flags.
- Wraps CONFIG.debug in a client setting which preserves your choices.


### [1000Nettles' Foundry Magic L10n](https://github.com/1000nettles/foundry-magic-l10n)

- This CLI tool allows you, a module or system developer, to generate 10 different localization files in the FoundryVTT "language file" format. This utilizes AWS Translate - a "fluent and accurate machine translation" engine. It takes your base English translations, and converts them into language files for you to download and include in your own modules and systems.


### [NickEast's Foundry Project Creator Types](https://gitlab.com/foundry-projects/foundry-pc/foundry-pc-types) (now deprecated)
- As of 0.6.6 a good set of typescript type declarations for `foundry.js`. Support has dwindled during the `0.7.x` lineup but this is still a good starting point. With 0.8.x coming out, NickEast has stated:
> At this point I've moved to working with 0.8.x, which means my own type definitions are now pretty much "dead". I will start on my own "internal" definitions for 0.8.x, but I've yet to decide whether to start on a separate branch, or just make a clean slate... I'll keep everything open, but will focus on what I need instead of progressively increasing coverage.