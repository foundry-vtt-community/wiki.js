---
title: Improve the handling of vision and lighting rendering to make use of the canvas buffer region while obscuring it from Token vision (unless the controlled Token is within that buffer)
description: 
published: true
date: 2021-05-01T03:02:24.675Z
tags: 
editor: markdown
dateCreated: 2021-05-01T03:02:20.849Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4885

## What changed

SightLayer container has some changes which affects how it is rendered.

```
 * The container structure of this layer is as follows:
 * sight: SightLayer              The SightLayer itself
 *   msk: PIXI.Graphics           A masking rectangle that restricts exploration to the scene background
 *   unexplored: PIXI.Graphics    An unexplored background that spans the entire scene canvas
 *   explored: PIXI.Container     The exploration container
 *      revealed: PIXI.Container  The container of areas which have been previously revealed
 *        saved: PIXI.Sprite      The saved FOW exploration texture from the database
 *        pending: PIXI.Container A container of pending exploration polygons that have not yet been saved
 *      current: PIXI.Container   The current vision container
 *        fov: PIXI.Graphics      The current filed-of-view polygon
 *        los: PIXI.Graphics      The current line-of-sight polygon
 *      msk: PIXI.Graphics        The masking rectangle that limits exploration to the Scene background

```

## What you need to change

Verify that your code doesn't rely on something that was moved.

* [Unverified] 

### Research Notes

* By 0.10.0 Dice rolling will no longer be synchronous