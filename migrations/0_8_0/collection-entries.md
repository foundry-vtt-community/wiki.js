---
title: Rename Collection#entries to Collection#contents to avoid colliding/overriding the default Map#entries behavior with a differently behaved property
description: 
published: true
date: 2021-02-07T17:11:45.390Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:11:45.390Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4390

## What changed

`Collection#entries` -> `Collection#contents` to avoid messing around with `Map#entries`

## What you need to change

- [Unverified] expect a collection's `entries` to now be its `contents`

### Research Notes

- 