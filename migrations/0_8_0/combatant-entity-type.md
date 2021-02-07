---
title: Create a Combatant embedded entity type which internalizes helper methods for manipulating a combatant entry within the Combat tracker.
description: 
published: true
date: 2021-02-07T16:45:20.716Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T16:45:20.716Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4090

## What changed

Combatant is an embedded entity type which internalizes helper methods like the old `Combat#_prepareCombatant`.

## What you need to change

- [Unverified] A host of deprecated methods should be swapped out.

### Research Notes

- This should make things simpler to handle with regard to the combat tracker.