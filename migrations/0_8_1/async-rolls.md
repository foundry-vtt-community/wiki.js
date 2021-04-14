---
title: RollTable#roll refactored to async to now return a Promise in support of asynchronous dice rolling infrastructure.
description: 
published: true
date: 2021-04-14T12:25:13.673Z
tags: 0.8.1
editor: markdown
dateCreated: 2021-04-14T12:25:13.673Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4778

## What changed

RollTable#roll now returns a promise as a part of the Migration to async rolls.

## What you need to change

* [Unverified] Ensure any instances of `RollTable#roll` handle receiving a promise instead of the previous value.

### Research Notes

* Other methods for Rolltables (e.g. `draw` and `drawMany` are already async and thus were not broken), this will have a smaller affect than it seems at first glance.