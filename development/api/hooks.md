---
title: Hooks
description: API documentation for interacting with and creating Hooks
published: true
date: 2024-04-26T03:02:02.965Z
tags: development, api
editor: markdown
dateCreated: 2022-03-15T14:35:36.691Z
---

# Hooks

![Up to date as of v11](https://img.shields.io/static/v1?label=FoundryVTT&message=v11&color=informational)

Hooks are an important method by which the core software, systems, and even modules can provide interaction points for other developers.

*Official Documentation*
- [Hook Events](https://foundryvtt.com/api/modules/hookEvents.html)
- [Hooks](https://foundryvtt.com/api/classes/client.Hooks.html)

*Note:* Not all core hook events are documented by the hook events page, and any system- or module-specific hooks may or may not be documented on that specific package's repository.

**Legend**

```js
Hooks.on // `.` indicates static method or property
// The Hooks class doesn't use instance methods as it's never instantiated
```

## Overview

Hooks are how Foundry Core exposes certain public API events modules and systems to interact with. It is always recommended to register a [callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) for an existing hook event instead of [monkey patching](https://www.audero.it/blog/2016/12/05/monkey-patching-javascript/) a core method whenever possible.

---

## Key Concepts

Working with hooks requires keeping in mind the following limitations.

### Registration and Execution

There are two sides to the hook architecture: registering a callback and executing registered callbacks (aka triggering a hook).

These aspects are ignorant of eachother. It is not problematic to register a callback for an event which never fires, nor is it problematic to trigger a hook which has no callbacks registered.

### Returned Values

Hook callbacks ignore returned values except in cases where the event is triggered with `call`. If `call` is used, returning an explicit `false` will stop the hook event cycle and stop whatever upstream event is occuring (e.g. returning `false` in `preUpdate` will stop the update). 

### Synchronous in nature

Hooks do not `await` any registered callback that returns a promise before moving on. It is however advisable to use a Promise as a hook callback when the callback you register does not need to block the main process.

### Local only
Hooks callbacks only execute on the client triggering that hook. Any core hook that appears to fire on all clients is actually firing on each client individually in response to a socket broadcast from the server. Typically these are related to the [Document update cycle]().

When creating hooks for a package, it is recommended to rely on the api consumer creating its own socket implemention, rather than broadcasting a socket event which triggers a hook.

### Notes about `this`

A hook callback is executed in the context of the core `Hooks` class, not in the context the hook was triggered from.

It is expected that all of the data a hook callback will need should be provided in its arguments, rather than expecting `this` to reference useful data.

---

## API Interactions

### Registering a Hook callback

There are two ways to register a hook callback with slightly different usecases:

#### [`Hooks.on`](https://foundryvtt.com/api/classes/client.Hooks.html#on)

Used when the callback being registered should run every time the event is triggered.

```javascript=
function someFunction(hookArg1, hookArg2) {
  console.log('hookEvent callback', hookArg1, hookArg2);
}

Hooks.on('hookEvent', someFunction);
```

#### [`Hooks.once`](https://foundryvtt.com/api/classes/client.Hooks.html#once)
Used if the event might be triggered many times but the callback being registered should only run once.

This is a convience method to make manually calling `Hooks.off` unecessary for this specific use case.

```javascript=
function oneTimeFunction(hookArg1, hookArg2) {
  console.log('hookEvent callback that should run once', hookArg1, hookArg2);
}

Hooks.once('hookEvent', oneTimeFunction);
```

### Unregistering a Callback

#### [`Hooks.off`](https://foundryvtt.com/api/classes/client.Hooks.html#off)

Used when a particular use case calls for a Hook callback to be executed a specific number of times, or if some other control makes the callback unecessary.

Unregistering a hook callback can be done two ways:
1. When registering a hook, an ID is provided. Calling `Hooks.off` and providing this ID will unregister that callback.
2. Calling `Hooks.off` and providing a reference to the same function that was registered initially will unregister that callback.

```javascript=
function someFunction(hookArg1, hookArg2) {
  console.log('hookEvent callback that should run once', hookArg1, hookArg2);
}

const hookId = Hooks.on('hookEvent', someFunction);

// later...

Hooks.off('hookEvent', hookId);
// OR
Hooks.off('hookEvent', someFunction); // both ways work
```

### Executing callbacks

It is possible to leverage the Hook API for your own use cases, rather than simply registering callbacks for Core's existing hooks. Doing this is as simple as running `call` or `callAll` and providing a unique hook name. Any callbacks registered will fire at that point on the client machine which calls the hook. This is a great way for system developers to allow modules to extend system functionality.

Remember Hooks must be synchronous and cannot `await` their registered callbacks.

#### [`Hooks.call`](https://foundryvtt.com/api/classes/client.Hooks.html#call)

Calls the registered callbacks in order of registration, stopping when any of them explicitly returns `false`. This means not all registered callbacks might be called.

Useful for cases where a hook callback should be able to interrupt a process.

```javascript=
function someProcessWithHook(arg) {
  // you can pass any number of additional arguments after the event name
  const canProceed = Hooks.call('myCustomInterruptHook', arg);
  // You may want some kind of error message here more elaborate than the simple `return`
  if (!canProceed) return;
  
  // do something else
}
```


#### [`Hooks.callAll`](https://foundryvtt.com/apiclasses/client.Hooks.html#callAll)

Calls the registered callbacks in order of registration, ensuring that all registered callbacks are called.

Useful for cases where a hook callback should not be able to interrupt a process, for example to notify third party scripts that an event has happened and allow them to respond to event.

```javascript=
function someProcessWithHook(arg) {
  Hooks.callAll('myCustomHookEvent', arg);
  
  // do something else
}
```
---

## Specific Use Cases

Below are some common patterns working with specific hooks

### Render hooks

One extremely common use for hooks is the various `render` hooks, which are triggered by instances of the [Application](/en/development/api/application) class. Whenever an application is rendered, a hook fires for it and each of its parent classes, e.g. `renderActorSheet` then `renderDocumentSheet` then `renderFormApplication` then `renderApplication`. Each of these event calls has the same information, the difference is just being able to specify how far up the inheritance tree you want to operate.

All render hooks pass the same three arguments
- `app`: The sheet class instance
- `html`: A JQuery object of the application's rendered HTML
- `data`: The result of the `getData` operation that was fed into the application's handlebars template

A common usage pattern within these hooks is adding new inputs; by properly assigning the `name` property, you can have the application's native form handling do the work for you. Remember that you can't just assign arbitrary data to a data model, so you usually have to work with [flags](/en/development/api/flags) to define your additional data.

```js
Hooks.on("renderActorSheet", (app, html, data) => {
  // The value after `??` controls the "default" value for the input
	const myData = app.actor.getFlag("myModule", "myFlag", "myData") ?? "foobar";
  // The `value` sets what shows in the input, and `name` is important for the form submission
  const myInput = `<input type="text" value="${myData}" name="flags.myModule.myFlag">`;
  // the jquery work here may be kinda complicated
  html.find(".some .selector").after(myInput);
})
```

---

## Troubleshooting

Below are some common issues people run into when working with hooks.

### What hooks are firing when?

You can use `CONFIG.debug.hooks = true` in the console to set foundry to be verbose about when hooks are firing and what arguments they provide. It can be useful to have a simple macro to toggle this behavior:

```js
CONFIG.debug.hooks = !CONFIG.debug.hooks
console.warn("Set Hook Debugging to", CONFIG.debug.hooks)
```

If you already know the name of the hook, you can also use `Hooks.once("hookEventNameHere", console.log)` to cleanly send the next instance to console to access its properties.

Hooks that fire as part of Foundry's initialization process, such as `init`, are documented in the [Game](/en/development/api/game) article.

### Object Reference Troubles

Objects (this includes class instances as well as arrays) passed to hooks are *pass by reference* - any mutations made to them will also be present in the object instances used by the function that called them. If the arguments are re-assigned, that linkage breaks and changes will *not* be reflected. 

By contrast, primitives - strings, numbers, booleans - will NOT see any changes made to them reflected in the original function.

```js
Hooks.on("someEvent", (someObj) => {
  // this change will be present on the original instance
	someObj.someProp = true
  // this breaks the link and further changes will not be reflected
  someObj = { foo: "bar" } 
})
```