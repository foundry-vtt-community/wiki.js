---
title: Canvas
description: The visual game surface in Foundry Virtual Tabletop is managed by a WebGL-powered canvas which uses the PixiJS library.
published: false
date: 2024-04-20T00:07:40.091Z
tags: documentation
editor: markdown
dateCreated: 2024-04-20T00:07:40.091Z
---


## Overview

---
## Key Concepts

---
## API Interactions

---
## Specific Use Cases

---
## Troubleshooting

### Debugging Vision

```js
CONFIG.debug.polygons

// in type choose one of the values in WALL_RESTRICTION_TYPES: ["light", "sight", "sound", "move"]
ClockwiseSweepPolygon.create(point, {debug: true, type: CONST.WALL_RESTRICTION_TYPES})
```