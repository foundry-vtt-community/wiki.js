---
title: World Scripts
description: 
published: true
date: 2023-10-25T05:50:40.401Z
tags: 
editor: markdown
dateCreated: 2021-01-11T04:53:14.478Z
---

> This is known to be up to date as of Foundry Core 0.7.9
{.is-info}


World scripts, like modules and macros, are a way for users to define additional functionality for FoundryVTT.

## Comparison

### World Scripts vs Modules
World scripts and modules are very similar. Both allow for 3rd party javascript files to be loaded and executed by Foundry. Because of their similarities, learning resources designed for learning to make modules are often applicable to world scripts.

#### Distribution and Packaging
The primary difference between world scripts and modules is that modules are designed to be easy to package and distribute to other users. This makes it easy to share complex functionality and collections of related other assets such as html templates, css styles, images, compendia, and so on. However, this ability also requires some additional steps by the module author to create a module manifest, set up a build pipeline (if applicable), and handle hosting of the package.

World scripts, on the other hand, are not as portable and simple to distribute, but require less setup by the author. Thus, they are more suited (as the name implies) to world-specific functionality and configuration, or for tasks that are not worth the initial setup time of a module.

One major downside as a result of this difference is that if a user distributes a world script for others to use, and that world script has a bug, or conflicts with a core or system update, there is little recourse available to distribute the fix, and thus users will need to manually update or fix the issue.

#### Activation
Another difference between world scripts and modules is that modules can be enabled and disabled through the Foundry module management UI, while world scripts are always active.

### World Scripts vs Macros
World scripts and macros fill quite different niches in terms of 3rd party code. Their main similarity is that both are relatively easy to set up when compared to a module.

#### Structure
Macros take the form of entities within a Foundry world or compendium. They are written in the built-in Foundry macro editor. Since macros are entities, they get the concept of user permissions for "free", they can be manipulated programmatically like other Foundry entities, and can be stored in compendia.

World scripts, on the other hand, are javscript files which are written and edited in an external editor like Visual Studio Code. They are statically served to clients and thus can't be mutated in the same ways as macros.

#### Execution
World scripts are loaded and executed as soon as the Foundry page loads, while Macros are latent until explicitly executed by a user (in core Foundry, at least). This is a massive difference that makes some very important concepts feasible in world scripts that are not feasible with macros, such as usage of hooks and sockets.

This also adds an additional consideration that developers must take into account compared to authoring macros: world scripts are executed on every client. Developers must be careful to ensure that updates are only called from one client to avoid doing duplicate work, and avoid permissions issues from trying to update entities that the client does not have access to update.

### Summary
To summarize the difference between world scripts, modules, and macros:

1. World scripts and modules, unlike macros, are loaded and executed right away, and thus have access to features that are infeasible to use in macros.
2. World scripts are somewhere between modules and macros in terms of ease of development.

## Usage
In order to use a world script you must: (a) have a Javascript file you want to add to your world, and (b) edit the world's manifest to point to your Javscript code.

Your Javscript file will usually live within the world directory for the world in which it is used, but in reality, it could live anywhere in your Foundry userdata folder (see [Where Is My Data Stored?](https://foundryvtt.com/article/configuration/#where-user-data) on the Foundry KB for more information). This guide assumes that your Javscript is in the root of the world directory (i.e. `Data/worlds/my-world/my-script.js`).


### Adding a world script to your world manifest
To include your Javascript file in your world:
1. Navigate to your world's directory in your user data folder.
2. Open `world.json` in a text editor (Visual Studio Code is a good choice, but almost any editor will do).
3. In the `world.json` file, look for a line with the `esmodules` key. If there isn't one, add it after any of the existing keys, like this for example:
```json
{
  "title": "Evil Awakened",
  "esmodules": [],
  "version": "1.0.0",
  ```
4. Add the path to your Javascript file to the `esmodules` array. If your Javascript file is stored in the root of the world directory as suggested above, just write the filename in quotation marks. It will look like this when you're done: `"esmodules": [ "my-script.js" ],` (note the comma is required at the end of the line, unless you've added this as the very last key in the JSON file).
5. Save and close `world.json`.
6. If the world was already active, return to setup and launch it again. Whenever you change the `world.json`, you *must* re-launch the world for the changes to take effect.

## Examples

As mentioned above, world scripts are great for initial configuration tasks that would be a bit too simple for a whole module. This isn't intended to be a tutorial, but rather a set of examples of tasks that are well suited to world scripts.

### Basic use of Hooks
```js
// This example greets players with an on-screen notification once Foundry
// has finished its initial loading
Hooks.on("ready", () => ui.notifications.notify("Welcome to the World!"));
```

### Setting the default angle of a cone template
```js
// The default Foundry cone angle is 53.13 degrees.
// This world script will set the default angle for this world to 90 degrees.
Hooks.on("setup", () => CONFIG.MeasuredTemplate.defaults.angle = 90);
```

### Adding a new status effect icon
```js
// This world script will add a new status effect icon (a blue circle)
// that can be applied to tokens
Hooks.on("setup", () => {
  CONFIG.statusEffects.push({ id: "bluecircle", label: "Blue Circle", icon: "path/to/blue-circle.png" })
});
```

### Setting the default token config
```js
// This script will change the default token configuration for newly created or imported actors
//   (does not affect actors already in the world).
// It changes the following:
// - Show the token's name when an owner of the token hovers over it
// - Always show all resource bars for owners of the token
// - Sets the first bar to display the token's hit points (dnd5e)
Hooks.on("preCreateActor", (actorData, options, userId) => {
  actorData.token = actorData.token ?? {};
  mergeObject(actorData.token, {
    displayName: CONST.TOKEN_DISPLAY_MODES.OWNER_HOVER,
    displayBars: CONST.TOKEN_DISPLAY_MODES.OWNER,
    bar1: { attribute: "attributes.hp" },
    bar2: { attribute: null },
  });
});

```
### Adding custom Door Sounds (v11)
```js
/// assumes you have an "audio" folder in your Data folder
Hooks.once("init", function(){
    CONFIG.Wall.doorSounds.myCustom = {
       label: "My Custom Sound",
       open: "./audio/customOpenSound.ogg",
       close: "./audio/customCloseSound.ogg",
       lock: "./audio/customLockSound.ogg",
       unlock: "./audio/customUnlockSound.ogg",
       test: "./audio/customTestSound.ogg",
    }
 });
```

NOTE:
The dnd5e specific scripts that were previously listed here have been moved to the [dnd5e repository's wiki](https://github.com/foundryvtt/dnd5e/wiki/Modifying-Your-Game-with-Scripts#examples)