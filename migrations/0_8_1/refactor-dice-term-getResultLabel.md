---
title: Refactor DiceTerm.getResultLabel to be an instance method DiceTerm#getResultLabel allowing for more nuanced formatting of the labeled output.
description: 
published: true
date: 2021-04-21T15:10:05.238Z
tags: 0.8.1
editor: markdown
dateCreated: 2021-04-14T12:47:13.132Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4610

## What changed

`DiceTerm.getResultLabel(result)` is now an instance method `DiceTerm#getResultLabel(result)`

## What you need to change

* [Unverified] Stop using this as a static method and instead use the instance method.

### Research Notes

* 