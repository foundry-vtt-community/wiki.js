---
title: Development Resources
description: 
published: true
date: 2021-03-01T01:31:21.826Z
tags: development, resource, tutorial, template, macro
editor: markdown
dateCreated: 2020-09-18T21:54:56.070Z
---

## Templates and Starter Kits

There are a variety of community-created module and system starter kits out there. These range from very opinionated to barely opinionated.


### [League Basic JS Module Template](https://github.com/League-of-Foundry-Developers/FoundryVTT-Module-Template)
- Lightly Opinionated
- **Supports:** Javascript
- **Description:** A template repository for FoundryVTT module development that comes with versioned CI/CD.


### [League Basic TS Module Template](https://github.com/League-of-Foundry-Developers/foundry-typescript-template)
- Lightly Opinionated
- **Supports:** Typescript
- **Description:** A template for typescript projects that uses manifest+ and auto creates versions using Github Actions.


### [NickEast's Foundry Project Creator](https://gitlab.com/foundry-projects/foundry-pc/create-foundry-project)
- Very Opinionated
- Bootstraps both Systems and Modules
- **Supports:** Typescript, SCSS, LESS, Javascript
- **Description:** The Foundry Project Creator is a tool that developers can use to create modules and systems for the Foundry Virtual Tabletop software. It is designed to provide a (partially opinionated) boilerplate project and a set of scripts to quickly get started.


### [DJ Addi's VSCode Debugging Enabled Template](https://gitlab.com/DJ.Addi/fvtt-project-template)
- Basically no Opinions
- Has VS Code Debugger integration out of the box
- **Description:** A bare bones template that plugs into the debugger features of Visual Studio Code.

### [TyphonJS's Demo Rollup Module](https://github.com/typhonjs-fvtt/demo-rollup-module)
- Very Opinionated
- Uses Rollup to bundle your module with examples of cleanly including node module libraries into final package
- **Supports:** Javascript, Sass / SCSS / PostCSS, minification / mangling w/ sourcemaps, and auto creates versions using Github Actions.
- **Description:** A starter project template demonstrating Rollup to bundle your module code regardless if you plan to include Node modules or not.

### [ghost's Foundry Factory](https://github.com/ghost91-/foundry-factory)
- Very opinionated (though less opinionated presets could easily be added)
- Bootstraps both modules and systems
- Theoretically supports different presets that can be chosen from. However, only one is provided at the moment, which is based on Gulp and Rollup.
- **Supports (depends on the preset)**: JavaScript, TypeScript, Less, SCSS, ESLint, Jest, sourcemaps
- **Description**: Foundry Factory is an interactive CLI tool that developers can use to bootstrap modules and systems for Foundry Virtual Tabletop. It allows developers to choose among different presets to initialize their projects (currently only one preset based on Gulp and Rollup is available).


## Development Tools

### [foundry-vtt-types](https://github.com/kmoschcau/foundry-vtt-types)
- A community effort to revamp and refactor NickEast's typescript types for `0.7.x`. This set of type definitions leverages more advanced typescript tooling.

### [NickEast's Foundry Project Creator Types](https://gitlab.com/foundry-projects/foundry-pc/foundry-pc-types)
- As of 0.6.6 a good set of typescript type declarations for `foundry.js`. Support has dwindled during the `0.7.x` lineup but this is still a good starting point.

### [TyhponJS's @eslint/foundry](https://www.npmjs.com/package/@typhonjs-fvtt/eslint-config-foundry.js)
- A plug and play shareable eslint configuration package containing all exported globals from `foundry.js` that when combined w/ the `no-shadow` rule prevents overwriting core Foundry VTT functionality. Useful for module / system development
- [Demo of it in action](https://github.com/typhonjs-fvtt/demo-rollup-module/blob/main/.eslintrc)

### [Quench - End to End testing within Foundry](https://github.com/schultzcole/FVTT-Quench)
- Allows devs to register end to end tests powered by Mocha (and also includes Chai), which are then run in-game in a native Foundry Application.


## Macros and Code examples

### [Community Macros](https://github.com/foundry-vtt-community/macros)
- Community contributed macros that can be downloaded as a module, a wide range of topics with pf1, pf2, and dnd5e system specific macros covered therein.


### [Vance's Code Examples](https://github.com/VanceCole/macros)
- Excellent resource covering a huge range of examples for how to interact with Foundry Core.


### [Kekilla's Macros](https://github.com/Kekilla0/Personal-Macros)
- Another well organized personal macro repository covering a wide range of useful applications.


## Tutorials

### [Spacemandev's FoundryVTT Development Video Series](https://www.youtube.com/watch?v=UDVH6UIFRos&list=PLLF8ndmyGEGyAWaZifYCzqMuRq2lsaENA)
- Content that covers topics ranging from Setting up a development environment to the deep dive videos: Macros 101 and 102.


### [Mougli's FoundryVTT System Development Video Series](https://www.youtube.com/watch?v=gcSN4AQcUzM&list=PLFV9z59nkHDccUbRXVt623UdloPTclIrz)
- Video Series for those looking to create their own Systems.

### [Svelte and Foundry](https://sunspots.eu/posts/foundry-svelte/)
- A quick guide by Sunspots detailing their strategy for using Svelte to make Foundry Modules.