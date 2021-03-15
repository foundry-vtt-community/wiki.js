---
title: Standardize the representation of the canvas global to have the same expected properties in cases where a canvas is used as well as in cases where no canvas is present.
description: 
published: true
date: 2021-02-07T17:05:38.378Z
tags: 
editor: undefined
dateCreated: 2021-02-07T17:05:34.935Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4359

## What changed

The `game.canvas` and `globalThis.canvas` will now always reference a singleton `Canvas` instance for every view. This means it is no longer necessary to condition references to the canvas using optional chaining. For example, canvas.ready is always defined (and may be false) where previously you would need to test `canvas?.ready`

This is not (strictly) a breaking change, since any prior references to canvas? will continue to work as normal, but I am flagging it using Breaking so that developers are aware of the change.

## What you need to change

- [Unverified] switch logic checking if canvas exists to instead check if `canvas?.ready`

### Research Notes

- 