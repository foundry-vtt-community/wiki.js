---
title: Active Effect
description: An embedded document that can be used to modify the attributes of other documents during prepareData
published: false
date: 2024-06-27T17:00:31.734Z
tags: documentation
editor: markdown
dateCreated: 2024-06-08T05:46:12.955Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Active Effects are a built-in way for users, systems, and modules to dynamically alter actor properties.

*Official documentation*

**Legend**
```js
ActiveEffect.fromStatusEffect // `.` indicates static method or property
ActiveEffect#duration // `#` indicates instance method or property
```

## Overview

The ActiveEffect document class provides extensive built-in functionality; it not only comes with its own core-defined sheet, but the actual effect application process is also handled by Foundry. Most systems only need to provide sheet buttons for basic CRUD operations.

## Key Concepts

### Schema

### Change Modes
___
## API Interactions

### CRUD Operations

Active effects inherit all of the ordinary Document and ClientDocument operations. A typical sheet listener looks like

```js
_createEffect(event) {
  aeCls = getDocumentClass("ActiveEffect");
  data = {
    name: aeCls.defaultName({parent: this.document}),
    img: "icons/svg/angel.svg",
  }
  aeCls.create(data, {parent: this.document});
}
```

### Actor#applyActiveEffects

### applyActiveEffect hook

### `type` and `system`
___
## Specific Use Cases

### Status Effects
___
## Troubleshooting