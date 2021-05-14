---
title: Add an asynchronous version of the dice and rolling APIs
description: 
published: true
date: 2021-04-21T16:50:34.265Z
tags: 0.8.1
editor: markdown
dateCreated: 2021-04-14T12:50:50.088Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4135

## What changed

Dice rolling now supports returning a promise.

## What you need to change

There is a deprecation path, nothing immediately will change.

* [Unverified] 

### Research Notes

* By 0.10.0 Dice rolling will no longer be synchronous