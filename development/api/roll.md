---
title: Roll
description: An interface and API for constructing and evaluating dice rolls. 
published: true
date: 2024-04-03T16:35:00.712Z
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

The `Roll` class has a large number of instance and static functions, the use of which may not be entirely obvious.

### [Roll#alter](https://foundryvtt.com/api/classes/client.Roll.html#alter)

This simple instance method will multiply or add to each of the dice terms in a formula. This won't cover all possible ways you may want to modify a roll, but is an efficient one for many common use cases, such as adding or subtracting dice from a die pool.

### CONFIG.Dice

**CONFIG.Dice.Rolls:** Both `Roll.create` and `Roll.defaultImplementation` refer to `CONFIG.Dice.Rolls[0]`, an array that by default is just the default `Roll` class. These methods are used in a number of places:
- Parsing the native chat command for `/r` and `/roll`
- Inline roll parsing (e.g. `[[/r 1d6]]`)
- `Combatant#getInitiativeRoll`
- The default class for `RollTable#roll`
- Internal evaluation handling for `MathTerm`, `ParentheticalTerm`, and `PoolTerm`

---
## Specific Use Cases

Here are some more specific examples and implementations involving the Roll class.

### Handling User-Input Formulas

The Roll class isn't just useful for literal dice rolls - it's also useful as a well-scoped math engine to process user input. You can use `Roll.safeEval` to validate that a user-input formula evaluates to a clean result, including the use of external roll data. This is the type of structure that `dnd5e` uses for its custom AC formulas.

```js
// assuming you've defined `actor` and `formula` in some way
const data = actor.getRollData()
const updatedFormula = Roll.replaceFormulaData(formula, data, {missing: '0', warn: true})
let result = null
try {
  result = Roll.safeEval(updatedFormula)
}
catch {
	ui.notifications.warn("Bad formula!") // this should be more descriptive
  // if you do this in a preUpdate operation you could `return false` here
  // or you can do some kind of fallback `result = FALLBACK_FORMULA` kinda deal
}
```


### Modifying System Rolls as a Module

By default, rolls do not fire hooks - that must be handled by the system itself. One common way to do this is with a "preRoll" hook, along the lines of the following.
```js
const roll = new Roll(formula, data)
if (Hooks.call("system.preRoll", roll) === false) return;
```

This constructs the roll, then *creates* a hook that modules can respond to with their own `Hooks.on("system.preRoll", (roll) => {})` invocation. Systems can any number of additional arguments to the hook - keep in mind that objects passed this way are mutable, which may or may not be desirable.

To actually modify the roll as a module, the simplest option is to modify the `terms` property of the roll, then call `roll.resetFormula()`. Modifying the `terms` can either involve making specific changes (Keeping in mind that the subclass of the RollTerm matters - if you're changing a `1d6` to a `4` or visa versa, you have to swap between `DiceTerm` and `NumericTerm`. Another method is to to re-run `roll.terms = roll.constructor.parse(formula, data)`, if you want to totally rebuild things.

---
## Troubleshooting

These are some of the common foibles and stumbling blocks when dealing with rolls.

### Asynchronicity

Because `Roll#evaluate` is asynchronous, unless you're using the `maximize` or `minimize` options it cannot be used in strictly synchronous operations. Within Foundry, there's two common cases for this

- Data Preparation
- `pre` [document](/en/development/api/document) operation [hooks](/en/development/api/hooks). (Note that the `_pre` document *functions* available to systems *do* run asynchronously).

For the first, this is an intentional limitation - data preparation should be idempotent, which means re-running it shouldn't create new results. Randomization from a roll would violate this principal.

For the second, this is a major limitation of modules compared to systems; if you find yourself needing to perform an asynchronous `preCreate`, `preUpdate`, or `preDelete` operations, consider the two following two options
1. Refactor to use `create`, `update`, or `delete` operations - these fire on all clients, so you will need to ensure only one client performs any further operations and you avoid infinite loops.
2. Use [libWrapper](https://foundryvtt.com/packages/lib-wrapper) and wrap the relevant document class's operation.

---