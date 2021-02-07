---
title: Create a formalized PlaylistSound embedded document type, instances of which belong to the playlist#sounds Collection.
description: 
published: true
date: 2021-02-07T17:36:06.015Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:36:06.015Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4450

## What changed

- `Playlist#sounds` is now a `DocumentCollection` containing `PlaylistSound` instances rather than an array of objects

- `Playlist#playSound` is deprecated in favor of `PlaylistSound#play`



## What you need to change

- [Unverified] ensure you expect the full DocumentCollection model
- [Unverified] stop using playSound

### Research Notes

- 