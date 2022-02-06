---
title: GreenSock
description: Enabling and accessing the powerful GreenSock animation API in your system or module.
published: true
date: 2022-02-06T08:13:05.793Z
tags: greensock
editor: markdown
dateCreated: 2022-02-06T08:13:05.793Z
---

## What Is GreenSock?
The [GreenSock Animation Platform](https://greensock.com/) ("gsap" for short) is a robust JavaScript toolset that provides a host of powerful, easy-to-use, high-performance features for animating just about any visual element your module or system adds to the Foundry experience.  Foundry not only exposes the core GreenSock API from within its native code, it also includes the complete suite of premium "Club GreenSock" features for your use. Powerful, fully-featured and easy to use, GreenSock is an excellent way to add visual flare to your creative endeavors.

## Learning the GreenSock API
A tutorial on GreenSock's features is beyond the scope of this article, but don't be discouraged: GreenSock's documentation is excellent, and is directed at the beginner. Start [here](https://greensock.com/get-started/) for an introduction and feature overview, then head over to the [GSAP](https://greensock.com/docs/v3/GSAP) chapter of the [GreenSock Docs](https://greensock.com/docs/v3) and poke around: there are plenty of videos, examples, CodePen tutorials and the like to get you started.

## Implementing GreenSock in Your Foundry Project
Enabling GreenSock in your Foundry project, then accessing it from within your scripts, isn't especially well-documented and is the main purpose of this guide.  Fortunately, the process isn't difficult --- follow these few quick steps, and you'll be off to the races:

### 1) Add a Reference in Your `system.json`/`module.json` File
Add `"greensock/esm/all.js"` as an entry in the `"esmodules"` property of your `system.json`/`module.json` file, for example:

```json
{
  "name": "mysystem",
  "title": "My System",
  ...
  "esmodules": [
    "scripts/mysystem.js",
    "greensock/esm/all.js"
  ],
  ...
}
```
### 2) Import `gsap` and Any Desired Plugins As Needed in your JavaScript Files
The `gsap` object serves as the access point for most of GreenSock's functionality, and is the default export from `greensock/esm/all.js`. In many cases, this will be all you need to accomplish your animation goals:
```javascript
import gsap from "/scripts/greensock/esm/all.js";

// The gsap object can handle most animations on its own (see the GreenSock documentation for details):
gsap.to("#turn-me-orange", {backgroundColor: "orange", duration: 2});
```
More specialized features are offered in the form of plugins (a full list of which can be found [here](https://greensock.com/docs/v3/Plugins)). Additional plugins should be imported by name alongside the default `gsap` object, and only as needed (to avoid unnecessary bloat). Finally, it's good practice to register any plugins you import with the `gsap` object --- this isn't always necessary, but it never hurts:

```javascript
import gsap, {
  MorphSVGPlugin,
  Physics2DPlugin,
  PixiPlugin,
  SplitText
} from "/scripts/greensock/esm/all.js";

gsap.registerPlugin(MorphSVGPlugin, Physics2DPlugin, PixiPlugin, SplitText);
```

> A namespace collision currently exists between Foundry's own 'Draggable' class, and the 'Draggable' plugin offered by GreenSock.  To use GreenSock's 'Draggable' plugin (and its many awesome features), you must rename it to something else when you import it (I suggest "Dragger"):
{.is-warning}

```javascript
import gsap, {Draggable as Dragger} from "/scripts/greensock/esm/all.js";
gsap.registerPlugin(Dragger);
```