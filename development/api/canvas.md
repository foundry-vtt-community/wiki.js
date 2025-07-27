---
title: Canvas
description: The visual game surface in Foundry Virtual Tabletop is managed by a WebGL-powered canvas which uses the PixiJS library.
published: true
date: 2025-07-27T15:46:29.249Z
tags: documentation
editor: markdown
dateCreated: 2024-04-20T00:07:40.091Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

Foundry's Canvas is the primary method by which Foundry fulfills its function as a *Virtual Tabletop*, providing a space to render maps full of rich details and complex interactions.

*Official Documentation*

- [PixiJS](https://pixijs.com/)
- [Canvas](https://foundryvtt.com/api/classes/foundry.canvas.Canvas.html)

**Legend**

```js
Canvas.getRenderTexture // `.` indicates static method or property
Canvas#ready // `#` indicates instance method or property
```

## Overview

The most performance-intensive aspect of Foundry Virtual Tabletop, the game canvas forms the third pillar of development, alongside [Documents](/en/development/api/document) and [Applications](/en/development/api/applicationv2). The canvas uses PixiJS to build on both the database management of documents as well as the UI controls of applications; understanding the canvas will frequently require deeper knowledge of how each of those three contribute to the overall rendering. 

Fortunately, while the Canvas is very complex, most developers do not need to worry about it; the core software team spends significant amounts of time each major release refining and upgrading the canvas capabilities. The downside of this is that the canvas API is not very stable, and so has major breaking un-deprecated changes every version. The top of this page has a version marker; even more so than the other pages in the wiki's API documentation, if you are developing for a different core version than what this documents it is unlikely the information here will be accurate.

Code for the Canvas class and its related classes can be found at `yourFoundryInstallPath/resources/app/client/canvas`. Before v13, it was found at `yourFoundryInstallPath/resources/app/client/pixi` as well as `yourFoundryInstallPath/resources/app/client-esm/canvas`.

---
## Key Concepts

This is the most essential information to understanding the canvas

### Groups and Layers

The canvas is made up of a number of groups which define the drawing process. The top most is the `"stage"`, which encompasses all of Foundry's core-defined groups. They are drawn in the following order with the designated hierarchy.

- hidden
- rendered
  - environment
    - primary
    - effects
  - visibility
  - interface
- overlay

Almost all canvas layers are drawn as part of the `interface` group, with the exception being the `weather` layer which is drawn as part of the `primary` group. These layers largely represent the various canvas documents - `tiles`, `notes`, `lighting`, etc., with the additional ones being `controls`, `grid`, and `weather`. As each layer is drawn, it draws any objects contained in it, such as the associated placeable object for the embedded documents in the viewed scene.

---
## API Interactions

The canvas has many points of interaction for systems and modules that wish to modify its behavior.

### CONFIG

Systems and modules have many ways to interact with the canvas by modifying the globally available `CONFIG` object. You almost always modify this inside of an `init` hook.

- Canvas document classes, such as Token and Tiles, have `objectClass`, `layerClass`, and `hudClass` properties to replace and extend the relevant classes like you might for `documentClass`. (e.g. `CONFIG.Token.objectClass = MyToken`)
  - Many of these objects have additional properties such as `CONFIG.Wall.doorSounds`
- `CONFIG.Canvas` has many configurable properties, including the `groups` and `layers` that are used to define new groups and layers or alter the existing ones

### Scene Control Buttons

API Reference

- `getSceneControlButtons`
	- [v12](https://foundryvtt.com/api/v12/functions/hookEvents.getSceneControlButtons.html)
  - [v13](https://foundryvtt.com/api/functions/hookEvents.getSceneControlButtons.html)
- `SceneControl`
	- [v12](https://foundryvtt.com/api/v12/interfaces/client.SceneControl.html)
	- [v13](https://foundryvtt.com/api/interfaces/foundry.SceneControl.html)
- `SceneControlTool`
	- [v12](https://foundryvtt.com/api/v12/interfaces/client.SceneControlTool.html)
  - [v13](https://foundryvtt.com/api/interfaces/foundry.SceneControlTool.html)

The controls on the left side of the screen can be modified in the `getSceneControlButtons` hook. The only argument it passes, `controls`, is the current set of scene controls.
Before v13, this was an array of `SceneControl` objects. To add new ones, you would use `controls.push` to add new elements, or [splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) if you want to add your controls to a spot besides the bottom of the array.
Starting in v13, `controls` is instead an object, keyed by the control's name. A new one can be added directly to the object (`controls.myControl = { name: "myControl", title: "My Control", ...}`).

- The `title` property of the scene control as well as individual tools are automatically localized. 
- The `icon` property is expected to be a valid [FontAwesome](https://fontawesome.com/search?o=r&m=free) class, e.g. `"fa-solid fa-expand"`. 
- The signature for the `onClick` callback is `(toggled: boolean) => void` if the `toggle: true`, otherwise it is just `() => void`

---
## Specific Use Cases

Here are some tips and tricks when working with the canvas.

### Retrieving Grid Spaces

#### Square Grids
```js
// Grabbing the hovered grid's space
const p = canvas.mousePosition; // { x, y }
const { x, y } = canvas.grid.getTopLeftPoint({x: p.x, y: p.y });
```

#### Hex Grids
```js
// Grabbing the hovered grid's space
const p = canvas.mousePosition; // { x, y }
const coords = canvas.grid.getCenterPoint({ x: p.x, y: p.y }); // { x, y }
const cube = canvas.grid.getCube(coords); // { q, r, s }
const offset = canvas.grid.getOffset(cube); // { q, r, s }
const { x, y } = canvas.grid.getTopLeftPoint(offset);
```

### Highlighting a Grid Space

```js
// Registering the Highlight 
const highlightId = "foo";
canvas.interface.grid.addHighlightLayer(highlightId);

const CONFIG = {
  x, y, // The grid space's coordinates
  color: 0xff0000, // The fill color
  border: null, // The border color
  alpha: 0.25, // The highlight's opacity
  shape: null // A PIXI.Polygon, unused unless highlighting a gridless canvas
};
// Positioning the Highlight
canvas.interface.grid.highlightPosition(highlightId, CONFIG);

// Hiding the Highlight
canvas.interface.grid.clearHighlightLayer(highlightId);

// Destroying the Highlight
canvas.interface.grid.destroyHighlightLayer(highlightId);
```

---
## Troubleshooting

Here are some tips and tricks to help with common pitfalls while developing for the canvas.

### Debugging Vision

```js
CONFIG.debug.polygons

// in type choose one of the values in WALL_RESTRICTION_TYPES: ["light", "sight", "sound", "move"]
ClockwiseSweepPolygon.create(point, {debug: true, type: CONST.WALL_RESTRICTION_TYPES})
```