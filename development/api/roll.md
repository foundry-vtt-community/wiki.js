---
title: Roll
description: An interface and API for constructing and evaluating dice rolls. 
published: true
date: 2024-04-02T16:42:39.584Z
tags: documentation
editor: markdown
dateCreated: 2024-03-13T20:34:57.466Z
---

![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

Rolls are how Foundry handles randomization and the game math involving them.

Official Documentation
- [Roll](https://foundryvtt.com/api/classes/client.Roll.html)
- [RollTerm](https://foundryvtt.com/api/classes/client.RollTerm.html)

**Legend**
```js
Roll.simulate() // `.` indicates static method or property
Roll#terms // `#` indicates instance method or property
```
  
## Overview

Rolls are a critical piece of Foundry's infrastructure and useful to every tabletop roleplaying game, but their actual usage is entirely dependent on the system or module.

---
## Key Concepts

Rolls work like normal javascript classes; code will frequently feature lines like `const roll = new Roll(formula, data)`, where the resulting `roll` variable is an instance of the Roll class. Also unlike other parts of Foundry, there aren't a ton of built-in static methods - most everything that matters is done as part of an instance of the class.

### Evaluation

Once a roll instance is constructed, you can call `Roll#evaluate` to produce a total for the roll. If you're planning to just send it straight to chat, `Roll#toMessage` will evaluate the roll then create the chat message.

**Asynchronicity.** Calling `Roll#evaluate` is an asynchronous operation, which means it must happen within an asynchronous function if you want to be able to use the `total`. The exception is if you pass `{maximize: true}` or `{minimize: true}`, in which case the roll terms can be evaluated synchronously because turns non-deterministic values (e.g. `1d6`) into deterministic ones (e.g. `6`).

**Evaluated Once.** Once a roll is evaluated, that instance is locked; if you need to reroll, you can use `Roll#clone` to create a new instance that can be modified, or perhaps a the helper method `Roll#reroll` which clones and then immediately evaluates.

### Roll Terms

The two most important subclasses of `RollTerm` are `DiceTerm` and `NumericTerm`. DiceTerm is *non-deterministic*, which is to say that the result will change each time it is evaluated. NumericTerm, by contrast, _is_ deterministic.

---
## API Interactions
> Stub
> This section is a stub, you can help by contributing to it.
---
## Specific Use Cases
> Stub
> This section is a stub, you can help by contributing to it.

### Modifying an existing Roll

> Stub
> This section is a stub, you can help by contributing to it.

---
## Troubleshooting

### Asynchronicity

Because `Roll#evaluate` is asynchronous, unless you're using the `maximize` or `minimize` options it cannot be used in strictly synchronous operations. Within Foundry, there's two common cases for this

- Data Preparation
- `pre` [document](/en/development/api/document) operation [hooks](/en/development/api/hooks). (Note that the `_pre` document *functions* available to systems *do* run asynchronously).

For the first, this is an intentional limitation - data preparation should be idempotent, which means re-running it shouldn't create new results. Randomization from a roll would violate this principal.

For the second, this is a major limitation of modules compared to systems; if you find yourself needing to perform an asynchronous `preCreate`, `preUpdate`, or `preDelete` operations, consider the two following two options
1. Refactor to use `create`, `update`, or `delete` operations - these fire on all clients, so you will need to ensure only one client performs any further operations and you avoid infinite loops.
2. Use [libWrapper](https://foundryvtt.com/packages/lib-wrapper) and wrap the relevant document class's operation.

---