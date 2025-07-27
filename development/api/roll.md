---
title: Roll
description: An interface and API for constructing and evaluating dice rolls. 
published: true
date: 2025-07-27T14:31:31.438Z
tags: documentation
editor: markdown
dateCreated: 2024-03-13T20:34:57.466Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

Rolls are how Foundry handles randomization and the game math involving them.

Official Documentation
- Knowledge Base
  - [Basic Dice](https://foundryvtt.com/article/dice/)
  - [Dice Modifiers](https://foundryvtt.com/article/dice-modifiers/)
  - [Advanced Dice](https://foundryvtt.com/article/dice-advanced/)
- [Roll](https://foundryvtt.com/api/classes/foundry.dice.Roll.html)
- [RollTerm](https://foundryvtt.com/api/classes/foundry.dice.terms.RollTerm.html)

**Legend**
```js
Roll.simulate() // `.` indicates static method or property
Roll#terms // `#` indicates instance method or property
```
  
## Overview

Rolls are a critical piece of Foundry's infrastructure and useful to every tabletop roleplaying game, but their actual usage is entirely dependent on the system or module.

Code for Roll and its related classes can be found in `yourFoundryInstallPath/resources/app/client/dice/` (`yourFoundryInstallPath/resources/app/client-esm/dice/` before v13).

---
## Key Concepts

Rolls work like normal JavaScript classes; code will frequently feature lines like `const roll = new Roll(formula, data)`, where the resulting `roll` variable is an instance of the Roll class. Also unlike other parts of Foundry, there aren't a ton of built-in static methods - most everything that matters is done as part of an instance of the class.

### Evaluation

Once a roll instance is constructed, you can call `Roll#evaluate` to produce a total for the roll. If you're planning to just send it straight to chat, `Roll#toMessage` will evaluate the roll then create the chat message.

**Asynchronicity.** Calling `Roll#evaluate` is an asynchronous operation, which means it must happen within an asynchronous function if you want to be able to use the `total`. The exception is if you pass `{maximize: true}` or `{minimize: true}`, in which case the roll terms can be evaluated synchronously because turns non-deterministic values (e.g. `1d6`) into deterministic ones (e.g. `6`).

**Evaluated Once.** Once a roll is evaluated, that instance is locked; if you need to reroll, you can use `Roll#clone` to create a new instance that can be modified, or perhaps a the helper method `Roll#reroll` which clones and then immediately evaluates.

### Roll Terms

The `Roll#terms` property actually determines how the roll is evaluated, and is constructed with `Roll.parse` on the formula and data. It's an array of `RollTerm` instances, which are handled in-order during evaluation to produce the final total.

The two most important subclasses of `RollTerm` are [`DiceTerm`](https://foundryvtt.com/api/classes/client.DiceTerm.html) and [`NumericTerm`](https://foundryvtt.com/api/classes/client.NumericTerm.html). DiceTerm is *non-deterministic*, which is to say that the result will change each time it is evaluated. NumericTerm, by contrast, _is_ deterministic and always produces the same results.

### Options

The `options` argument in the roll constructor baseline only uses a `flavor` argument, but any JSON serializable parameter (e.g. Arrays but not Sets) can be passed and will be saved to `this.options`.

The purpose of `options` is far larger; when a roll is added to a chat message, it is serialized as a JSON string and then reconstituted by field initialization. If you assign `this.foo = "bar"`, the serialization will *not* save that. However, if you assign `this.options.foo = "bar"`, that *will* be saved by the serialization and will be properly available to chat messages.

Examples of things you might want to save into options:
- Roll modifiers separate from the `formula` and `data`, like advantage/disadvantage in 5e
- Target values to turn flat rolls into success/fails.
- Other conditional modifier logic, like whether to reroll 1s.

---
## API Interactions

The `Roll` class has a large number of instance and static functions, the use of which may not be entirely obvious.

### [Roll#alter](https://foundryvtt.com/api/classes/foundry.dice.Roll.html#alter)

This simple instance method will multiply or add to each of the dice terms in a formula. This won't cover all possible ways you may want to modify a roll, but is an efficient one for many common use cases, such as adding or subtracting dice from a die pool.

### Roll Rendering

API Reference
- [toMessage](https://foundryvtt.com/api/classes/foundry.dice.Roll.html#tomessage)
- [CHAT_TEMPLATE](https://foundryvtt.com/api/classes/foundry.dice.Roll.html#chat_template)

Foundry has built-in support for sending rolls to chat; often times just calling `roll.toMessage()` is sufficient. The first argument, `messageData`, uses the following defaults and then is passed into `new ChatMessage`.

```js
{
  user: game.user.id,
  content: String(this.total),
  sound: CONFIG.sounds.dice
}
```

If this doesn't seem like much, that's correct - the majority of the formatting comes from `roll.render`, which references `Roll.CHAT_TEMPLATE` and `Roll.TOOLTIP_TEMPLATE` to build the actual message HTML. These static methods are the *actual* best place to alter roll HTML as a system through providing your own handlebars template to be filled in. 

The default object passed to the `CHAT_TEMPLATE` is as follows
```js
    const chatData = {
      formula: isPrivate ? "???" : this._formula,
      flavor: isPrivate ? null : flavor ?? this.options.flavor,
      user: game.user.id,
      tooltip: isPrivate ? "" : await this.getTooltip(),
      total: isPrivate ? "?" : Math.round(this.total * 100) / 100
    }
```

If you want to *change* that, you need to override `Roll#render`. Calling `super` won't be helpful in this instance as it's returning the raw HTML string; instead, you need to fully override the function. 

For further context, `Roll#render` is called by two functions; `ChatMessage#_renderRollHTML` and `RollTable#toMessage`. The reason roll rendering is complicated and dynamic is to respect roll privacy settings across different clients, rather than have a static chunk of HTML stored in the message `content`.

### CONFIG.Dice

**CONFIG.Dice.Rolls:** Both `Roll.create` and `Roll.defaultImplementation` refer to `CONFIG.Dice.Rolls[0]`, an array that by default is just the base `Roll` class. These methods are used in a number of places:
- Parsing the native chat command for `/r` and `/roll`
- Inline roll parsing (e.g. `[[/r 1d6]]`)
- `Combatant#getInitiativeRoll`
- The default class for `RollTable#roll`
- Internal evaluation handling for `MathTerm`, `ParentheticalTerm`, and `PoolTerm`

In combination with overriding `CHAT_TEMPLATE` and `TOOLTIP_TEMPLATE` you can do deep alterations of Foundry's default roll display across all possible invocations.

The remainder of the array is used as part of chat message serialization; if you use a roll subclass but *don't* register it with `CONFIG.Dice.Rolls.push` the rolls will fail to be properly reconstructed as part of message initialization.

### Roll.replaceFormulaData

API Reference

- [replaceFormulaData](https://foundryvtt.com/api/classes/foundry.dice.Roll.html#replaceformuladata)

The `replaceFormulaData` function, a static method of the Roll class, can help users visualize the effect of their roll formulas by providing the "translated" formula. 

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

#### Manual Synchronous "Rolling"
If you require synchronous randomization, for simple scenarios you can build your own.

Given a `formula` of the format `xdy+z`:

```js
const TWIST = new foundry.dice.MersenneTwister(Date.now());
const roll = new Roll(formula);
roll.evaluateSync({strict: false}); // calculate the constant portion
let total = Array(roll.dice[0].number).fill(roll.dice[0].faces).reduce((acc,f) => acc+Math.ceil(TWIST.random()*f), roll.total);
```

---