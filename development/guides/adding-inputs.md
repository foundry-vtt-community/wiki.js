---
title: Adding Inputs
description: Module-added inputs to system and core sheets
published: true
date: 2025-03-20T06:26:19.922Z
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

The rest of the tutorial will cover what goes *inside* this hook callback, in place of the simple `console.log` presented above..

### 2. Construct the input

API Reference: [foundry.applications.fields](https://foundryvtt.com/api/modules/foundry.applications.fields.html)

The next step is to actually build the input. While there are a few ways to do this, this guide will focus on using Foundry's built-in helpers, found under the `foundry.applications.fields` namespace.

The most critical part of any of these functions is passing in the `name` and `value`. This is where flags come in; our input's value will be stored in the flags of the document.

```js
// This guide is written with the assumption that the target app is actually
// a subclass of DocumentSheet or DocumentSheetV2; if `doc` here ends up undefined,
// you're in a more complicated situation and should ask #module-development
// on the main Foundry discord for help
const doc = app.document;

// This is your package ID
const scope = "my-module";

// This can be whatever you want
const key = "myFLag";

// The current value. The ?? operator is nullish coalescing 
// and fills in the default value if the getFlag return is undefined or null
const defaultValue = "Foobar";
const value = doc.getFlag(scope, key) ?? defaultValue;
// The name of the input we're constructing
const name = `flags.${scope}.${key}`;

const input = foundry.applications.fields.createTextInput({ name, value });
```

The `name` and `value` properties are the only *required* fields for every input type; the API docs linked above provide info about all of the other optional properties, such as the `min` and `max` available to number inputs.

**Select Inputs.** One set of properties does deserve a callout; `createSelectInput` requires an `options` property which is an array of objects with at least a `label` and `value` property. Commonly, you'll have reference to some external object that is a key: value pair; fortunately, `Object.entries` and `array.map` can help transform this key:value pair into an array of the desired shape.

```js
const record = { foo: 1, bar: 2 };

const options = Object.entries(record).map(([value, label]) => ({value, label}));
```

**FormGroup.** Another common desire is to not only have the input, but have some kind of label. In most situations, the createFormGroup function is the answer. This takes an input (like the ones constructed above), as well as a label and optional hint properties. It also takes an array of strings as classes - you can use this for your own styling or just pass `classes: ["stacked"]` to access alternative stylings provided by core.

```js
const input = foundry.applications.fields.createTextInput({ name, value });

const group = foundry.applications.fields.createFormGroup({ 
  input, label: "MYMODULE.Foo.bar", localize: true 
});
```

### 3. Add input to the sheet

API Reference
- [querySelector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [insertAdjacentElement](https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentElement)

The final bit is actually adding the input to our sheet using native js functions. You'll need to use the js browser inspector to find the actual part of the sheet you want to add, then construct a selector for it.

```js
const target = html.querySelector(".some .selector .path");

// Good way to check if you found the right thing.
console.log(target);

target.insertAdjacentElement("afterend", group);
```

There's a lot of different options here, based on how your target application is structured and where exactly you want to put your new input.

#### AppV2 parts

One pitfall with AppV2 is its new feature for "partial re-renders". The initial hook may get called without resetting the HTML you added your input to, meaning that your added input would multiply. The fix is to monitor the fourth argument, `options` - the following code snippet could go near the top of your render hook.

```js
if (options.parts && !options.parts.includes("my-target-part")) return;
```

Figuring out the parts and which one your input is going into will likely require consulting the source code of the Application class as well as the templates its using.

## Final construction

The following example is taken from [Complete Card Management](https://github.com/MetaMorphic-Digital/complete-card-management/). It extracts the callback into a function, which provides two benefits.
1. It's easier to add type annotations and other comments
2. The function can be extracted to another file. For simplicity the export/import logic is excluded from the example.

```js
// Used throughout the module
const MODULE_ID = "complete-card-management";

/**
 * Add Scene pile selection
 * @param {SceneConfig} app
 * @param {HTMLElement} html
 * @param {Record<string, unknown>} context
 * @param {Record<string, unknown>} options
 */
function renderSceneConfig(app, html, context, options) {
  /** @type {Scene} */
  const scene = app.document;

  // You can build options out of anything iterable
  // In this case, it's a list of card piles
  const selectOptions = game.cards.reduce((arr, doc) => {
    if (!doc.visible || (doc.type !== "pile") || !doc.canUserModify(game.user, "update")) return arr;
    arr.push({value: doc.id, label: doc.name});
    return arr;
  }, []);

  const input = foundry.applications.fields.createSelectInput({
    name: `flags.${MODULE_ID}.canvasPile`,
    value: scene.getFlag(MODULE_ID, "canvasPile"),
    options: selectOptions,
    blank: ""
  });

  const group = foundry.applications.fields.createFormGroup({
    input,
    label: "CCM.SceneConfig.CanvasPileLabel",
    hint: "CCM.SceneConfig.CanvasPileHint",
    localize: true
  });

  const basicOptions = html.querySelector(".tab[data-group=\"ambience\"][data-tab=\"basic\"]");

  // append is another helpful function for mutating the DOM
  basicOptions.append(group);

  // Mutating the DOM can result in an app overflowing rather than growing.
  // A call to setPosition will cure this.
  app.setPosition();
}

Hooks.on("renderSceneConfig", renderSceneConfig)
```

## Buttons

Adding a button works largely the same, the main difference being that you then need to grab the constructed button element and add a click listener with whatever callback you wanted to use. It may be easier to write an HTML string or construct one with `renderTemplate` than try to go through `document.createElement("button")`; if so, the [`insertAdjacentHTML`](https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentHTML) function will be helpful. 

Once you've added the button to the DOM, you can use `querySelector("button").addEventListener("click", console.log)` to add a click function. Make sure to adjust the selector path and the callback.