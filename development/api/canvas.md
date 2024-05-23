---
title: Canvas
description: The visual game surface in Foundry Virtual Tabletop is managed by a WebGL-powered canvas which uses the PixiJS library.
published: true
date: 2024-05-23T06:51:48.096Z
tags: documentation
editor: markdown
dateCreated: 2024-04-20T00:07:40.091Z
---

![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

Foundry's Canvas is the primary method by which Foundry fulfills its function as a *Virtual Tabletop*, providing a space to render maps full of rich details and complex interactions.

*Official Documentation*

- [PixiJS](https://pixijs.com/)
- [Canvas](https://foundryvtt.com/api/classes/client.Canvas.html)

**Legend**

```js
Canvas.getRenderTexture // `.` indicates static method or property
Canvas#ready // `#` indicates instance method or property
```

## Overview

The most performance-intensive aspect of Foundry Virtual Tabletop, the game canvas forms the third pillar of development, alongside [Documents](/en/development/api/document) and [Applications](/en/development/api/applicationv2). The canvas uses PixiJS to build on both the database management of documents as well as the UI controls of applications; understanding the canvas will frequently require deeper knowledge of how each of those three contribute to the overall rendering. 

Fortunately, while the Canvas is very complex, most developers do not need to worry about it; the core software team spends significant amounts of time each major release refining and upgrading the canvas capabilities. The downside of this is that the canvas API is not very stable, and so has major breaking un-deprecated changes every version. The top of this page has a version marker; even more so than the other pages in the wiki's API documentation, if you are developing for a different core version than what this documents it is unlikely the information here will be accurate.

Code for the Canvas class and its related classes can be found at `yourFoundryInstallPath\resources\app\client\pixi`.

---
## Key Concepts

This is the most essential information to understanding the canvas

### Groups and Layers

> Stub
> This section is a stub, you can help by contributing to it.

---
## API Interactions

The canvas has many points of interaction for systems and modules that wish to modify its behavior.

### CONFIG

> Stub
> This section is a stub, you can help by contributing to it.

---
## Specific Use Cases

> Stub
> This section is a stub, you can help by contributing to it.

---
## Troubleshooting

Here are some tips and tricks to help with common pitfalls while developing for the canvas.

### Debugging Vision

```js
CONFIG.debug.polygons

// in type choose one of the values in WALL_RESTRICTION_TYPES: ["light", "sight", "sound", "move"]
ClockwiseSweepPolygon.create(point, {debug: true, type: CONST.WALL_RESTRICTION_TYPES})
```