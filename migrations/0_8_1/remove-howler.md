---
title: Deprecate use of the Howler.js library (immediately) in favor of a move to using native and more modern Web Audio APIs.
description: 
published: true
date: 2021-04-21T16:50:43.056Z
tags: 
editor: markdown
dateCreated: 2021-04-14T13:00:53.923Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/3679

## What changed

Howler has been removed from the codebase.

- scripts/howler.min.js is no longer included in Foundry views
- All references to Howler, Howl, etc.. have been replaced with internal structure in `AudioHelper` (the `game#audio` singleton) and the `SoundNode` class which provides an interface for `AudioBufferSourceNode` and other APIs


## What you need to change

* [Unverified] Migrate your sound-related code to the new APIs

### Research Notes

* 