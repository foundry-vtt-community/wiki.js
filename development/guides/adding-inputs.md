---
title: Adding Inputs
description: Module-added inputs to system and core sheets
published: true
date: 2025-03-19T04:33:15.530Z
tags: 
editor: markdown
dateCreated: 2025-03-18T16:04:29.278Z
---

![Up to date as of v13](https://img.shields.io/static/v1?label=FoundryVTT&message=v13&color=informational)

One extremely common desire for modules is to add a new input to an existing sheet, such as a system-provided character sheet or any of the core-provided sheets. Fortunately, if properly constructed, modules can easily use the core document submission and rendering process to manage their data.

This guide assumes you already have a module with a script attached; everything covered here is working within those js files. This process can also be useful to systems wishing to add functionality to core-provided sheets; just use your system ID whenever the code would refer to the module ID.

## API Reference

This article makes use of the following concepts
- [Hooks](/en/development/api/hooks)
- [Flags](/en/development/api/flags)

## Walkthrough

The total amount of code necessary for adding an input is fairly minimal, but it involves integrating multiple distinct parts of the foundry API.

### 1. Identify the render hook

Foundry fires a "render hook" whenever an application is opened or refreshed. You can see these (and other) hooks in the console by toggling CONFIG.debug.hooks. The following script macro makes it easy to turn this behavior on and off.

```js
CONFIG.debug.hooks = !CONFIG.debug.hooks

console.warn("Hook debugging set to", CONFIG.debug.hooks)
```

A typical render hook will look something like `renderCharacterSheet`, using the name of the class that's firing. Within foundry, a render operation will fire the hook not only for the class, but all of its parent classes as well. This may or may not be helpful, depending on your target; a system may have two sheets that are fairly similar and share a common parent class, in which case you may want to use the render hook for *that* class instead of the individual ones per sheet.

Once you've identified the render hook, you can test it in your script file by adding something similar to the following and refreshing the page then re-opening the application; you should see the yellow message in the console when you do.

```js
Hooks.on("renderCharacterSheet", console.warn)
```

This callback will print some number of parameters, in this order
1. The application instance
2. A reference to the contained HTML
3. The context used by the app to render the HTML
4. The options passed to rendering

The next thing to identify is whether you are working with AppV1 or AppV2. AppV2 was introduced in v12, but until v13 was barely used by core. As of v13, all core apps have been converted, but many systems may still rely on AppV1. There's a few ways to check; the most relevant one is that the *second* argument in the callback will be a jQuery instance in AppV1, and a vanilla dom element in AppV2. Once you've done this, you can progress your hook script based on what you're dealing with.

```js
// AppV1
Hooks.on("renderCharacterSheet", (app, [html], context) => {
  // The [html] is "dereferencing" the jQuery back to vanilla DOM
  // This allows the rest of the tutorial to treat the html variable as vanilla dom
  // regardless of if this is AppV1 or AppV2
  console.log(app, html, context);
});
// AppV2
Hooks.on("renderCharacterSheet", (app, html, context, options) => {
  console.log(app, html, context, options);
});
```



### 2. Construct the input

Special case for select

### 3. Add input to the sheet

querySelector, insertAdjacentElement

## Final construction

Full example

## Buttons

Adding a button works largely the same, the main difference being that 