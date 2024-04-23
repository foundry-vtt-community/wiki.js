---
title: ApplicationV2
description: The Application class is responsible for rendering an HTMLElement into the Foundry Virtual Tabletop user interface.
published: true
date: 2024-04-23T17:22:26.845Z
tags: documentation
editor: markdown
dateCreated: 2024-04-18T15:30:54.955Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Applications are a core piece of Foundry's API that almost every developer will have to familiarize themselves with. The ApplicationV2 class and its subclasses were introduced in Foundry V12, with the long-term goal of transitioning all applications to the new framework. The original [Application](/en/development/api/application) class isn't going away, but it is no longer the subject of active development.


*Official Documentation*

- ApplicationV2
- DocumentSheetV2

**Legend**

```js
ApplicationV2.DEFAULT_OPTIONS // `.` indicates static method or property
Application#render // `#` indicates instance method or property
```

## Overview

ApplicationV2 is an entirely new set of classes that serve as a complete alternative to the original Application class. If you are building a new UI element in Foundry and otherwise only plan to support Version 12 and later, it is strongly recommended you use ApplicationV2 instead of the original Application class.

Code for ApplicationV2 and its related classes can be found at `yourFoundryInstallPath\resources\app\client-esm\applications`.

## Key Concepts

Here are the core things to know about ApplicationV2, including comparisons to the original Application class.

### Advantages of ApplicationV2

- Native light/dark mode support
- Better application window frames
- Architecture supports non-Handlebars rendering engines much more easily
- Support for partial re-rendering in Handlebars
- Better lifecycle events
- Improved a11y handling
- Overall simpler and cleaner to implement

### Use of ESModules

Unlike the original Application classes, AppV2 and its subclasses are accessed through nested javascript modules, e.g. `foundry.applications.api.ApplicationV2`. One common trick when dealing with these is the use of [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) to reduce line length and improve comprehensibility, e.g.

```js
// Similar syntax to importing, but note that 
// this is object destructuring rather than an actual import
const { ApplicationV2, HandlebarsApplicationMixin } = foundry.applications.api

class MyHandlebarsApp extends HandlebarsApplicationMixin(ApplicationV2) {}
```

### Which Class to Extend

Unlike the App V1, the base App V2 class handles forms natively, without reliance on a subclass. If you're not writing some kind of Document sheet, in which case you should use the appropriate subclass, everything is going to extend `ApplicationV2`. However, one important thing to know is that you need to use some form of rendering engine, whether that's the Foundry-provided `HandlebarsApplicationMixin` or one created by a community package. 

## API Interactions

### HandlebarsApplicationMixin

## Specific Use Cases

### Example ActorSheetV2 implementation

### Example non-Document ApplicationV2

### Non-Handlebars Rendering Frameworks

## Troubleshooting