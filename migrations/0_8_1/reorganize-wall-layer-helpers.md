---
title: Reorganize the helper methods used to compute sound and vision source polygons into the WallsLayer as a generalized computePolygon helper which both the sight and sounds layer use (with different arguments).
description: 
published: true
date: 2021-04-21T16:49:59.330Z
tags: 0.8.1
editor: markdown
dateCreated: 2021-04-14T12:27:36.968Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4767

## What changed

[`WallsLayer#computePolygon`](https://foundryvtt.com/api/alpha/WallsLayer.html#computePolygon) now exists and takes over the roles that previous helper methods did.

## What you need to change

* [Unverified] Convert any code relying on now-removed helper methods to instead use `computePolygon`

### Research Notes

