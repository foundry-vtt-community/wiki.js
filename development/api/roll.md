---
title: Roll
description: An interface and API for constructing and evaluating dice rolls. 
published: true
date: 2024-03-14T05:09:07.510Z
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

### Asynchronous Evaluation

Once a roll is evaluated, that instance is locked; if you need to reroll, you can use `Roll#clone` to create a new instance that can be modified, or perhaps a the helper method `Roll#reroll` which clones and then immediately evaluates.

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

## Modifying an existing

---
## Troubleshooting

### Asynchronicity

> Stub
> This section is a stub, you can help by contributing to it.

---