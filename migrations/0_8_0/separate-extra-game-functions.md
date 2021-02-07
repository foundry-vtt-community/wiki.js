---
title: Separate functions and UI that are used exclusively outside the /game endpoint into a separate JS file which does not unnecessarily expose those to regular users.
description: 
published: true
date: 2021-02-07T16:49:30.225Z
tags: 
editor: markdown
dateCreated: 2021-02-07T16:49:30.225Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4115

## What changed

Applications and Classes/Functions not needed within /game are not exposed to regular users. E.g. `SetupConfiguration`

## What you need to change

- [Unverified] Verify that stuff still works for all user types instead of just GMs

### Research Notes

- This will only show itself if you log in as whatever permission doesn't get this.