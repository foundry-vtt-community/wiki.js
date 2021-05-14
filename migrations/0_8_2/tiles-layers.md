---
title: Refactor the TilesLayer to combine it into the BackgroundLayer which contains Tile objects for a single vertical cross-section of the Scene alongside a background image which fills the Scene canvas.
description: 
published: true
date: 2021-05-01T03:05:06.974Z
tags: 
editor: markdown
dateCreated: 2021-05-01T03:04:47.807Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4856

## What changed

The TilesLayer has been merged with BackgroundLayer, and a new ForegroundLayer has been added with the same shape.

## What you need to change

Check that your logic around Tiles doesn't reference `canvas.tiles` and instead checks for Tiles within `canvas.background` and/or `canvas.foreground`.

* [Unverified] 

### Research Notes

*