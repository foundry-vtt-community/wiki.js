---
title: Adding Inputs
description: Module-added inputs to system and core sheets
published: true
date: 2025-03-19T04:04:29.132Z
tags: 
editor: markdown
dateCreated: 2025-03-18T16:04:29.278Z
---

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

### 2. Construct the input

Special case for select

### 3. Add input to the sheet

querySelector, insertAdjacentElement

## Final construction

Full example

## Buttons

Adding a button works largely the same, the main difference being that 