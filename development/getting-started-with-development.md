---
title: Getting Started with Package Development
description: Some common hurdles facing new Package Developers
published: true
date: 2021-08-13T13:18:42.652Z
tags: settings
editor: markdown
dateCreated: 2021-02-05T16:13:36.470Z
---

> ### What is this?
> This is going to be our FAQ and "common hurdles to overcome" guide to help new package developers. It's going to be perpetually updating and changing.
> If you know of some questions or common sticking points for new package developers, please edit this to include those things.
> ### Have a question not answered here?
> The [League of Foundry VTT Developers](https://discord.gg/cudQBu8HKT) is a helpful development-focused discord community which aims to help veterans and new developers alike. Please drop by and ask us your questions, the more questions we get the more likely this document is going to be updated with their answers.
> ### Pseudocode
> None of the code within this document is guaranteed to work and should be tested before used in a world that you care about.
{.is-warning}

> This document is up to date as of 0.8.8
{.is-info}

# FAQs

## Is there a list of hooks?
*Main article: [Hooks Listening & Calling](/en/development/guides/Hooks_Listening_Calling)*

[There is some official documentation about hooks as of 0.8.](https://foundryvtt.com/api/hookEvents.html)

[There is also a community maintained hook typescript definition and documentation file.](https://github.com/League-of-Foundry-Developers/foundry-vtt-types/blob/foundry-0.8.x/src/foundry/foundry.js/hooks.d.ts) But there's an easier way to discover what hook you can use for a specific piece of functionality:

```js
CONFIG.debug.hooks = true;
```

With this enabled, you'll see every hook that fires as you interact with Foundry's UI and what arguments it is passed.

> Bonus Tip: Install [Developer Mode](https://www.foundryvtt-hub.com/package/_dev-mode/) and you can explore the hooks debug more easily.
{.is-info}

## How do I delete a value?

This is Foundry specific and will delete the `someKey` in the database entry for `someEntity`
```js
someEntity.update({
  -=someKey: null
})
```

## What does Foundry VTT use for templating?
*Main article: [Template Basics](/en/development/guides/Tabs-and-Templates/Template-Basics)*

The `.html` files in Foundry are actually [Handlebars](https://handlebarsjs.com/) files. In your own packages you can use either `.html` or `.hbs`.

### How do I use Handlebars Templates?

> Stub
> [`loadTemplates`](https://foundryvtt.com/api/global.html#loadTemplates)
> [`renderTemplate`](https://foundryvtt.com/api/global.html#renderTemplate)

### How do I debug Handlebars?

Handlebars comes built in with a `log` helper, which can be used exactly like `console.log` can be to log out what data a template sees. It might help to think of a space as a comma within Handlebars `{{}}` brackets.

This snippet will log an object with all of the data that the "current scope" has access to:
```hbs
{{log 'some arbitrary string' this}}
```

## How do I change or extend a core function's behavior?

> stub
> "monkeypatching" definition (maybe in appendix?)
> Use [libWrapper.](https://github.com/ruipin/fvtt-lib-wrapper)


The short and mildly helpful answer is "Use [LibWrapper](https://github.com/ruipin/fvtt-lib-wrapper)."

LibWrapper is a library module which lets you 'safely' [monkeypatch](#monkeypatch) other functions, including class methods. We say "safely" because it has some built in QOL things around conflicting patches and puts the power in the user's hands to prioritize one module over another when said conflicts arise (and they will arise).

To pull this off first you have to find where the method or function is in the global scope which libWrapper can see and affect. For dnd5e's entity class that ends up being `game.dnd5e.entities.Actor5e` (assigned [here](https://gitlab.com/foundrynet/dnd5e/-/blob/master/dnd5e.js#L49)). Once you know what prototype you need to modify, you'll end up with something that looks like this:

```js
    libWrapper.register('my-fvtt-module-name', 'game.dnd5e.entities.Actor5e.prototype._prepareSkills', function (wrapped, ...args) {
        console.log('game.dnd5e.entities.Actor5e.prototype._prepareSkills was called with', ...args);
        const actor = this.actor;  // you should have access to `this` in here as well
        // ... do things ...
        const result = wrapped(...args); // remember to call the original (wrapped) method
        // ... do things ...
        return result; // this return needs to have the same shape (signature) as the original, or things start breaking
    });
```

## How do I get started with sockets?

The easiest way to use Sockets in a module is via [Socketlib](https://github.com/manuelVo/foundryvtt-socketlib). This library provides a useful abstraction layer on top of the core socket implementation.

> [Stub](https://github.com/VanceCole/macros/blob/master/sockets.js)

## How do I make an API available to other modules?

*Main article: [Library Modules](/en/development/library-modules)*

The convention among Foundry VTT Development community (and the official recommendation from Atropos himself) is to expose module-specific APIs on the module's moduleData located at `game.modules.get('my-module-name')?.api`. Additionally, any module can call a custom hook to inform other modules about events.


## How do I handle keyboard events?

> Stub. There's a [KeyboardManager](https://foundryvtt.com/api/KeyboardManager.html) class that might be useful here.
> There's also some library modules which allow keybind registration.

## What is a flag and how do I use them?

*Main article: [Handling Data: Flags, Settings, and Files](/en/development/guides/handling-data)*

Flags are the safest way that modules can store arbitrary data on existing entities. If you are making a module which allows the user to set a data point which isn't supported normally in Foundry Core or a system's data structure, you should use a flag.


## How do I work with settings?

*Main article: [Handling Data: Flags, Settings, and Files](/en/development/guides/handling-data)*

Settings, like flags, are a way for modules to store and persist data. Settings are not tied to a specific entity however, unlike flags. Also unlike flags they are able to leverage the 'scope' field to keep a set of data specific to a user's localStorage (`scope: client`) or put that data in the database (`scope: world`).


## How do I get data based on a user-defined data path?

For system independence, it's often a good idea to allow data paths to be defined by a setting (e.g. if you want to reference an attribute modifier in a dialog, then you could hard-code `modifier = actor.attributes.str.mod`, but that path is 5e specific - so allowing a user to define the path to the attribute modifier makes it easy to tweak your module for a specific system).

This can be done with the [`getProperty(object, key)`](https://foundryvtt.com/api/module-helpers.html#.getProperty) function, e.g. `modifier = getProperty(actor, attributeKey)`.


# Common Hurdles and How to Overcome Them

## My new package doesn't show up in Foundry

By far the most common issue when you're brand new to this. Don't panic and go through this checklist

### 1. Your manifest json has all the required fields. 

* [Modules](https://foundryvtt.com/article/module-development/)
* [Systems](https://foundryvtt.com/article/system-development/)

### 2. Your manifest's `name` matches your directory exactly.

Foundry verifies that the package `name` and the package directory name match exactly, otherwise it deems them invalid.

```json
{
  "name": "my-module-name"
}
```

With this module.json in the following structure, your module will not appear as installed in Foundry.

```
/Data
└ /modules
  ├ /not-my-module
  └ /my-module-wrong-name
    ├ myScript.js
    └ module.json
```

Instead you need to either change your directory or your `name` so they match.

### 3. Your manifest is in the root of your package directory.

Similar to 2, but affects people who have a build step.

```
/Data
└ /modules
  ├ /not-my-module
  └ /my-module-name
  	└ /dist
    	├ myScript.js
    	└ module.json
```

In this example, the manifest at `modules/my-module-name/dist/module.json` will not be found by Foundry and thus the module will not appear as installed.


## Storing Module Data
*Main article: [Handling Data: Flags, Settings, and Files](/en/development/guides/handling-data)*

There's basically three options for modules for how to store data: Flags, Settings, and Files.

# Module Development Tutorial

If you'd like a tutorial that walks through each step one at a time sequentially, this community made tutorial touches on a lot of the basics:

> ### [Foundry VTT Module Making for Beginners](https://hackmd.io/@akrigline/ByHFgUZ6u)
> This covers everything from "I have no files" to "I have a module which interacts with Flags, Settings, FormApplication, CSS, Localization, Hooks, and more." 




# Appendix 1: Fun Javascript Terminology

## Mutate

Basically means "changing" a piece of data.

```js

let foo = 'bar';

someFunction() {
  // does things...
  foo = 'bat';
  
  console.log({foo}); // 'bat'
}

someOtherFunction() {
  if (foo === 'bar') {
  	// do things...
  }
}
```

In the example above, `someFunction` mutates `foo`. Depending on the order in which it is called (before or after `someOtherFunction`) this might cause `someOtherFunction` to behave unexpectedly.

This isn't necessarily a bad pattern but it can save you some headaches if you avoid mutating things as much as possible. Instead try to make duplicates of your data when you want to change it.

## Object-oriented Programming (OOP)

OOP is a coding paradigm that is widely used in the Foundry Development community as it is what Foundry itself is built on. In essence, it values splitting functionality and data mangement into ES2020 `Class`es as opposed to a lot of independent functions and variables.

> #### Further reading
> - [MDN basics article](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS) 
> - [Dev.to comparison vs Functional Programming](https://dev.to/bhaveshdaswani93/oop-vs-fp-with-javascript-39jf)

## Promise

> [stub](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

## Monkeypatch

> stub [monkeypatch](https://davidwalsh.name/monkey-patching)
