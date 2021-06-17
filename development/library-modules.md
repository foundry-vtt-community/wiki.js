---
title: Library Modules
description: A quick list of known library modules which offer functionality for other modules to extend.
published: true
date: 2021-06-17T16:36:35.138Z
tags: 
editor: markdown
dateCreated: 2021-02-02T19:37:27.342Z
---

## What is a library module?

A library module is a module which itself does not offer any functionality for a user, rather provides an API which other modules can leverage.

## Conventions for Creating a Library Module

### Keep the API stable
Follow [semver](https://semver.org/) and avoid breaking the API if possible.

### `library`
Leverage the manifest JSON field `library` to inform Foundry Core that this module should be loaded first.

### Follow the [Best Practices](/en/development/guides/package-best-practices) for module APIs

The convention among Foundry VTT Development community (and the official recommendation from Atropos himself) is to expose module-specific APIs on the module's moduleData located at `game.modules.get('my-module-name')?.api`. Additionally, any module can call an arbitrary hook to inform other modules about events.

Together these two methods provide a bulletproof way for a module to expose an API to other modules.

#### Example

> #### Package Load Order
> It is a best practice to leverage a custom hook because packages fire hooks like `ready` and `init` in a specific order which you cannot control. A module might load before yours does, and if it relies on your module's api at ready, it will not have a good way to do so. 
{.is-warning}

##### cool-module
Cool module needs to set up some settings before it can reliably provide an API.
```js
class MyCoolClass {
  // ...
  
  static myStaticMethod(argument) {
    // does stuff you want other modules to have access to
  }
}


Hooks.on('init', () => {
	// my module needs to do something to set itself up (e.g. register settings)
  // ...
  
  // once set up, we create our API object
	game.modules.get('cool-module').api = {
    coolStaticMethod: MyCoolClass.myStaticMethod
  };
  
  // now that we've created our API, inform other modules we are ready
  // provide a reference to the module api as the hook arguments for good measure
  Hooks.callAll('coolModuleReady', game.modules.get('cool-module').api);
});
```

##### awesome-module
A different module can now reliably use this api like so.
```js
// if I need to do something as soon as the cool-module is ready
Hooks.on('coolModuleReady', (api) => {
  // do what I need with their api
});

// alternatively if I know that the API should be populated when I need it,
// I can defensively use the api on game.modules
game.modules.get('cool-module')?.api?.coolStaticMethod(someInput)
```



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
- SocketLib https://github.com/manuelVo/foundryvtt-socketlib - Provides commonly needed socket patterns in an abstracted form.




