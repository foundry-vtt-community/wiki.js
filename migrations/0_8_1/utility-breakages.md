---
title: Some moved utility functions which cause breaking changes were not documented in the 0.8.0 change-notes.
description: 
published: true
date: 2021-04-21T16:50:20.750Z
tags: 0.8.1
editor: markdown
dateCreated: 2021-04-14T12:42:17.955Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4668

## What changed
The following utility functions appear to have been moved:



From
To

_handleMouseWheelInputChange
foundry.js, Unexported

clampNumber
Math.clamped?

window.normalizeDegrees
Math.normalizeDegrees

window.normalizeRadians
Math.normalizeRadians

window.roundDecimals
Math.roundDecimals

window.toDegrees
Math.toDegrees

window.toRadians
Math.toRadians

## What you need to change

* [Unverified] You'll need to update your code that expects these functions

### Research Notes

* These were broken in 0.8.0 but documentation is being added about it now.