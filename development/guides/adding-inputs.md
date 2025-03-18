---
title: Adding Inputs
description: Module-added inputs to system and core sheets
published: true
date: 2025-03-18T16:04:29.278Z
tags: 
editor: markdown
dateCreated: 2025-03-18T16:04:29.278Z
---

One extremely common desire for modules is to add a new input to an existing sheet, such as a system-provided character sheet or any of the core-provided sheets. Fortunately, if properly constructed, modules can easily use the core document submission and rendering process to manage their data.



## API Reference

This article makes use of the following concepts
- [Hooks](/en/development/api/hooks)
- [Flags](/en/development/api/flags)

## Walkthrough

The total amount of code necessary

### 1. Identify the render hook

CONFIG.debug.hooks

### 2. Construct the input

Special case for select

### 3. Add input to the sheet

querySelector, insertAdjacentElement

## Final construction

Full example