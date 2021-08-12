---
title: Introduction to PIXI in Foundry VTT
description: 
published: true
date: 2021-08-12T16:03:47.688Z
tags: guide, pixi, layer, canvas, filter
editor: markdown
dateCreated: 2020-09-23T07:01:33.581Z
---

> This is known to be accurate as of Foundry Core 0.7.x
{.is-info}

This guide is intended to be a crash course in how to draw things on the canvas in Foundry VTT using PIXI.

## Basic Concepts
Foundry uses the powerful PIXI graphics library, and most of the time you will manipulate elements on the canvas through this interface, although if needed you should be aware that PIXI will allow you to dive deep and modify native WebGL elements directly if required.

Foundry VTT draws WebGL graphics to an HTML5 Canvas element, which is drawn such that it covers the screen. The canvas element itself is part of the HTML DOM tree but almost everything within it (except UI elements such as HUD) are not and you cannot access them as HTML elements. You can access the HTML Canvas element at `$('#board')` if needed but it is rarely necessary.

## The Major PIXI Elements

The canvas contains an instance of [`PIXI.Application`](https://pixijs.download/dev/docs/PIXI.Application.html) 
```js
canvas.app
```

The Application in turn contains 3 primary objects you will use:

- The **Stage** which holds your various elements, this is the outermost Container, a type of element which allows you to group objects which we will cover in more detail later
```js
canvas.app.stage
```
- The **Renderer**, which exposes the methods by which you manipulate and draw the elements within the Stage
```js
canvas.app.renderer
```
- The **Ticker**, which tracks time and manages calling for frame updates allowing animation
```js
canvas.app.ticker
```
Each of these will be covered in more detail later. For now, you simply need to know that the Stage holds your stuff, the renderer lets you control how the stuff is combined and drawn, and the ticker lets you control when that happens.

## The Foundry VTT Layers

In Foundry VTT, almost all elements drawn to the canvas will be contained within a CanvasLayer, which in turn are contained within the Stage. CanvasLayers typically cover the entire area of the stage, which unlike the canvas, is not necessarily the entire browser window.

The Layers are children of canvas.app.stage, and can be located at `canvas.app.stage.children` however for convenience you can access them at `canvas.layers` as well.

Layers tend to be instances of (or extended from) [`CanvasLayer`](https://foundryvtt.com/api/CanvasLayer.html) or [`PlaceablesLayer`](https://foundryvtt.com/api/PlaceablesLayer.html). For now, just know that all layers are in fact just [`PIXI.Container`](https://pixijs.download/dev/docs/PIXI.Container.html) with extra properties added but otherwise behave exactly like the [`PIXI.Container`](https://pixijs.download/dev/docs/PIXI.Container.html) we will be using below.

As of Foundry VTT 0.6.x there are 13 default layers, although modules can add more. These are, in order from lowest to highest:

```
ControlsLayer
EffectsLayer
SoundsLayer
SightLayer
LightingLayer
TokenLayer
NotesLayer
WallsLayer
TemplateLayer
GridLayer
DrawingsLayer
TilesLayer
BackgroundLayer
```

Note that an object on a lower layer can never be rendered above an object on a higher layer, thus it is important to choose carefully what layer you wish to add objects to, and if you add your own Layer, you should consider the best position to insert it.

## Drawing to the Canvas

Ok, let's dive in and start actually drawing things to the canvas. For these simple examples, I recommend you follow along in a browser console and reload in between lessons. Later on we will get to building more complex creations and use a simple module instead, but for now the console is convenient.

To keep things simple and tidy, let's start with a blank scene and make it fairly small. Create a new Scene and make it 500x500px, with no background image, and enable the Global Illumination setting. Set your grid to square with grid size 100px.

Now, lets create a simple graphic. In this case we will draw a simple square and place it near the Stage origin (top left corner). In your console, type the following to create a graphic, and hit enter.

```js
const g = new PIXI.Graphics();
g.beginFill(0x0000DD);
g.drawRect(0, 0, 200, 200);
g.endFill();
```

You will probably notice that nothing has happened. That's normal, as we haven't told PIXI how to add this graphic to our scene yet. But first, let's cover what we have done. In the first line, we create a new instance of [`PIXI.Graphics`](https://pixijs.download/dev/docs/PIXI.Graphics.html) and save a reference to it as `g` so we can use it later.

Then we began drawing a fill, and told PIXI what color to draw. `0x0000DD` is a hexadecimal representation of a shade of blue.

Next, we told PIXI what kind of shape to draw and how large. In this case, we specified to draw a rectangle at position x: 0, y: 0, or in other words at the top left corner. In PIXI x & y almost always refer to the top left corner of the current container as 0,0. The 200's tell pixi to draw the square 200 units in width and height, respectively.

Lastly, we told PIXI we were done drawing our shape. We could have drawn more shapes to this Graphic, but for now we'll keep it simple and leave it at a single square.

Now we want our square to actually show up on the canvas. To do that, we'll have to tell PIXI what Container should hold our graphic. In this case, let's just draw it directly to the stage for simplicity, although in Foundry VTT you should usually draw things to a Layer instead.

```js
canvas.app.stage.addChild(g);
```

You should now have a blue square positioned to the top left corner of your stage. Let's move the square around a bit. Try typing something like the following and seeing what happens:

```js
g.x = 200;
g.y = 120;
```

What happens if you set the x and y to a negative value? Try it and find out, just remember that if you set these values too large your square may disappear off the screen!

Try making your square invisible, then bringing it back again like so:

```js
g.visible = false;
```
```js
g.visible = true;
```

You can also set the alpha, which is a fancy way of saying "make it see through". With alpha, a value of 1 means fully opaque, while 0 is fully see through. Try this:

```js
g.alpha = 0.3;
```
```js
g.alpha = 0.7;
```

Lastly, let's try setting the rotation.

```js
g.rotation = 45;
```

You probably were expecting your square to rotate to 45 degrees, but if you look carefully you'll notice it is not 45 degrees. This is because the rotation property in PIXI refers to Radians not degrees, and 45 radians is actually around 2578 degrees, oops!

Let's fix that by setting the `angle` property instead:

```js
g.angle = 45;
```

Now your square should be nicely rotated to 45 degrees. But you might have noticed that your square rotated from the top left corner. That's because when you draw a rectangle the pivot point of the graphic begins wherever you draw from, in this case we started at 0,0 and drew the square down and right (positive height and width), so our pivot is the top left corner until we say otherwise.

Lets make our square straight again, then we'll change the pivot point of the graphic and see how that affects rotation.

```js
g.angle = 0;
g.pivot.x = 100;
g.pivot.y = 100;
g.angle = 45;
```

Now your square should be rotating around it's center point instead of a corner, excellent.

## Containers
## Sprites
## Masks
## Filters

Here is a list of [all default available Filters](http://pixijs.download/release/docs/PIXI.filters.html) for PIXI.

Some things to note about filters:
- Filters can only be applied to `PIXI.Sprite` and `PIXI.Graphic` elements.
- Filters are applied last in the render order, thus they cannot be applied to Masks.

### Filters and References

The reference for a given filter is preserved after it is applied to a PIXI elment. This allows for easy on-the-fly changes to filters if you set them up like so:

```js
// assume `someChild` is an existing PIXI.Sprite

const blurFilter = new PIXI.BlurFilter();
blurFilter.blur = 10; // 10 strength blur

someChild.filters = [blurFilter];

// ...

/**
 * Update the blurFilter strength
 */
function updateBlurFilterStrength(strength) {
  blurFilter.blur = strength;
}
```

## Coordinates
- ### Translating coordinates from global to local
- ### Mouse position
## Renderer
## Ticker & Animation
## Collision
