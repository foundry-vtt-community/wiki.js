---
title: Collection#get should return undefined on a failed lookup, rather than null, to remain compliant with the parent Map behavior.
description: 
published: true
date: 2021-02-07T17:47:51.837Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:47:51.837Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4504

## What changed

`Collection#get` now returns `undefined` instead of `null` for failed lookups

## What you need to change

- [Unverified] ensure you aren't expecting `null` for failed lookups

### Research Notes

- 