---
title: Hooks
description: API documentation for interacting with and creating Hooks
published: true
date: 2022-03-15T14:35:36.691Z
tags: development, api
editor: markdown
dateCreated: 2022-03-15T14:35:36.691Z
---

# Hooks

![Up to date as of v9](https://img.shields.io/static/v1?label=FoundryVTT&message=v9&color=informational)

## Overview

Hooks are how Foundry Core exposes certain public API events modules and systems to interact with. It is always recommended to register a [callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) for an existing hook event instead of [monkey patching](https://www.audero.it/blog/2016/12/05/monkey-patching-javascript/) a core method.

---

## Key Concepts

### Registration and Execution

There are two sides to the hook architecture: registering a callback and executing registered callbacks (aka triggering a hook).

These aspects are ignorant of eachother. It is not problematic to register a callback for an event which never fires, nor is it problematic to trigger a hook which has no callbacks registered.

### Returned Values

Hook callbacks ignore returned values except in cases where the event is triggered with `call`.

### Synchronous in nature

Hooks do not `await` any registered callback that returns a promise before moving on.

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

#### [`Hooks.on`](https://foundryvtt.com/api/Hooks.html#.on)

Used when the callback being registered should run every time the event is triggered.

```javascript=
function someFunction(hookArg1, hookArg2) {
  console.log('hookEvent callback', hookArg1, hookArg2);
}

Hooks.on('hookEvent', someFunction);
```

#### [`Hooks.once`](https://foundryvtt.com/api/Hooks.html#.once)
Used if the event might be triggered many times but the callback being registered should only run once.

This is a convience method to make manually calling `Hooks.off` unecessary for this specific use case.

```javascript=
function oneTimeFunction(hookArg1, hookArg2) {
  console.log('hookEvent callback that should run once', hookArg1, hookArg2);
}

Hooks.once('hookEvent', oneTimeFunction);
```

### Unregistering a Callback

#### [`Hooks.off`](https://foundryvtt.com/api/Hooks.html#.off)

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

It is possible to leverage the Hook API for your own use cases, rather than simply registering callbacks for Core's existing hooks. Doing this is as simple as running `call` or `callAll` and providing a unique hook name. Any callbacks registered will fire at that point on the client machine which calls the hook.

Remember Hooks must be synchronous and cannot `await` their registered callbacks.

#### [`Hooks.call`](https://foundryvtt.com/api/Hooks.html#.call)

Calls the registered callbacks in order of registration, stopping when any of them explicitly returns `false`. This means not all registered callbacks might be called.

Useful for cases where a hook callback should be able to interrupt a process.

```javascript=
function someProcessWithHook(arg) {
  const canProceed = Hooks.call('myCustomInterruptHook', arg);
  if (!canProceed) { return; }
  
  // do something else
}
```


#### [`Hooks.callAll`](https://foundryvtt.com/api/Hooks.html#.callAll)

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

> Stub
> This section is a stub, you can help by contributing to it.


---

## Troubleshooting

> Stub
> This section is a stub, you can help by contributing to it.
