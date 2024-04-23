---
title: ApplicationV2
description: The Application class is responsible for rendering an HTMLElement into the Foundry Virtual Tabletop user interface.
published: true
date: 2024-04-23T05:01:33.143Z
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

### Use of ESModules

Unlike the original Application classes, AppV2 and its subclasses are accessed through nested javascript modules, e.g. `foundry.applications.api.ApplicationV2`. One common trick when dealing with these is the use of [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) to reduce line length and improve comprehensibility, e.g.

```js
// Similar syntax to importing, but note that 
// this is object destructuring rather than an actual import
const { ApplicationV2, HandlebarsApplicationMixin } = foundry.applications.api

class MyHandlebarsApp extends HandlebarsApplicationMixin(ApplicationV2) {}
```

### Which Class to extend

No FormApplication

## API Interactions

### HandlebarsApplicationMixin


## Specific Use Cases

### Non-Handlebars Rendering Frameworks

## Troubleshooting