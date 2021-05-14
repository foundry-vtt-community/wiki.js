---
title: Refector Collection#getName to return undefined if no match is found, to be consistent with Map#get and Array#find
description: 
published: true
date: 2021-05-01T03:08:23.326Z
tags: 
editor: markdown
dateCreated: 2021-05-01T03:08:20.185Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4814

## What changed

Collection#getName will return `undefined` if no match is found instead of `null`. 

## What you need to change

Probably nothing.

* [Unverified] 

### Research Notes

*