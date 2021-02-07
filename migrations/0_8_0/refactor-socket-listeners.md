---
title: Rename the socketListeners static method of several classes to _activateSocketListeners to more semantically describe the action that is taken as well as to denote that the method should not be called by external code.
description: 
published: true
date: 2021-02-07T17:17:07.722Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:17:07.722Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4408

## What changed

`socketListeners` -> `_activateSocketListeners`

## What you need to change

- [Unverified] update your overrides to the new name

### Research Notes

- 