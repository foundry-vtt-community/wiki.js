---
title: Collection#find should have the same return type as Array#find
description: 
published: true
date: 2021-04-21T15:09:51.056Z
tags: 0.8.1
editor: markdown
dateCreated: 2021-04-14T12:35:14.411Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4699

## What changed

Collection#find now returns `undefined` instead of `null | undefined`

## What you need to change

* [Unverified] You no longer need to check if you are using Collection#find vs Array#find to predict the failure to find an entry, both return `undefined`

### Research Notes

* By and large this will have negligible impact I suspect.