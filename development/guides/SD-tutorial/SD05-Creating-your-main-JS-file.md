---
title: 05. Creating your main JS file
description: 
published: true
date: 2024-02-20T02:50:01.731Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:35:47.008Z
---

![Foundry v11 Compatible](https://img.shields.io/badge/Foundry-v11%20Compatible-blue)

Let's take a look at the Boilerplate System's `/module/boilerplate.mjs` file. We'll look at each section of it to see what's happening:

## Import your classes

```js
// Import document classes.
import { BoilerplateActor } from "./documents/actor.mjs";
import { BoilerplateItem } from "./documents/item.mjs";
// Import sheet classes.
import { BoilerplateActorSheet } from "./sheets/actor-sheet.mjs";
import { BoilerplateItemSheet } from "./sheets/item-sheet.mjs";
// Import helper/utility classes and constants.
import { preloadHandlebarsTemplates } from "./helpers/templates.mjs";
import { BOILERPLATE } from "./helpers/config.mjs";
```

In this first section, we've imported code from 6 different ES modules, one each for the Actor and Item document classes, and one each for the ActorSheet and ItemSheet sheet classes, one for Handlebars templates, and one for constants that are used later. For this system, those classes have `Boilerplate` as a prefix for them, but you should use a name more appropriate to your system when creating your own. 

> **Directory Structure**
> We'll review the recommended directory structure in detail later, but at a high level, the following structure is assumed in this tutorial for our ES modules:
> `/module/documents` - Classes for actor and item documents
> `/module/helpers` - Utility classes and helper methods
> `/module/sheets` - Classes for actor and item sheets
{.is-info}


Importing doesn't necessarily do anything at this point, but we do have the classes available in our file so that we can now let Foundry know how to use them.

## The 'init' hook

Foundry has a very robust hooks system that lets you hook into different kinds of events, such as `init`, `ready`, or other hooks for chat messages, scene render, etc. In this case, we'll be using the `init` hook to initialize the important overrides in our system. In the example below, everything should be considered essential for your system's init hook.

This example includes comments behind `//` that explain more about what's actually happening.

```js
/* -------------------------------------------- */
/*  Init Hook                                   */
/* -------------------------------------------- */

Hooks.once('init', function () {
  // Add utility classes to the global game object so that they're more easily
  // accessible in global contexts.
  game.boilerplate = {
    BoilerplateActor,
    BoilerplateItem,
    rollItemMacro,
  };

  // Add custom constants for configuration.
  CONFIG.BOILERPLATE = BOILERPLATE;

  /**
   * Set an initiative formula for the system
   * @type {String}
   */
  CONFIG.Combat.initiative = {
    formula: '1d20 + @abilities.dex.mod',
    decimals: 2,
  };

  // Define custom Document classes
  CONFIG.Actor.documentClass = BoilerplateActor;
  CONFIG.Item.documentClass = BoilerplateItem;

  // Active Effects are never copied to the Actor,
  // but will still apply to the Actor from within the Item
  // if the transfer property on the Active Effect is true.
  CONFIG.ActiveEffect.legacyTransferral = false;

  // Register sheet application classes
  Actors.unregisterSheet('core', ActorSheet);
  Actors.registerSheet('boilerplate', BoilerplateActorSheet, {
    makeDefault: true,
    label: 'BOILERPLATE.SheetLabels.Actor',
  });
  Items.unregisterSheet('core', ItemSheet);
  Items.registerSheet('boilerplate', BoilerplateItemSheet, {
    makeDefault: true,
    label: 'BOILERPLATE.SheetLabels.Item',
  });

  // Preload Handlebars templates.
  return preloadHandlebarsTemplates();
});
```

We're doing a few things in here:

1. **Line 8:** We're creating a `boilerplate` object on the global `game` object that Foundry itself provides so that we more easily have access to some of the classes defined in our ES modules. These are useful for things like debugging in your browser's console or letting modules interact with your system's classes.
2. **Line 15:** If you define constants for your system, such as in a `config.mjs` ES module, you can add them to the global `CONFIG` object like in this example.
3. **Line 21:** There are several ways to define how initiative is rolled, but the simplest is to override the `CONFIG.Combat.initiative` object like in this example.
4. **Line 27-28:** Because your system will define its own actor and item document classes, you need to override their CONFIG setting to utilize them.
5. **Line 33:** This will be covered later in a discussion of Active Effects.
6. **Line 36-45:** Foundry defines a general ActorSheet and ItemSheet class by default, so we need to unregister those sheets and instead register our own sheet classes. You can register as many sheets as you like, and modules can also register their own sheets.
7. **Line 48:** We'll go into more detail about this in a later step of the tutorial, but if you use Handlebars partials, this how you can preload them so that you can call them in templates.

## Handlebars Helpers

Foundry ships with a [LOT of core helpers](https://foundryvtt.com/api/classes/client.HandlebarsHelpers.html) on top of what Handlebars natively provides, but sometimes you need your own.

```js
/* -------------------------------------------- */
/*  Handlebars Helpers                          */
/* -------------------------------------------- */

// If you need to add Handlebars helpers, here is a useful example:
Handlebars.registerHelper('toLowerCase', function (str) {
  return str.toLowerCase();
});
```

This is a fairly simple example - you can call it with `{{toLowerCase "The Quick Brown Fox"}}` and the finished HTML will output `the quick brown fox`. As with other helpers you can provide variables as arguments in place of static strings and/or combine it with other helpers such as `{{toLowerCase (concat foo bar)}}` to return the value of `concat(foo, bar).toLowerCase()`.

## The 'ready' hook

Much like the `init` hook, Foundry also provides a `ready` hook that happens after systems and modules have finished initializing. It's useful for steps that need to happen later in the process.

```js
/* -------------------------------------------- */
/*  Ready Hook                                  */
/* -------------------------------------------- */

Hooks.once("ready", function() {
  // Include steps that need to happen after Foundry has fully loaded here.
});
```

## Making it your own

As with previous examples, this sample code using the Boilerplate System. You should rename your classes such as `MySystemNameActor` instead of `BoilerplateActor`, and you'll want to update the `registerSheet()` lines to replace `boilerplate` with `mysystemname`, using your system's machine name.

Now let's take a look at the extended Actor class.

---

* **Prev:** [template.json](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD04-templatejson)
* **Next:** [Extending the Actor class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD06-Extending-the-Actor-class)