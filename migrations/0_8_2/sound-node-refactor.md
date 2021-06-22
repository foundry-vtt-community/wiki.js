---
title: Refactor the SoundNode class into the Sound class which controls playback and the AudioContainer class which interfaces with the underlying Web Audio API for different node types.
description: 
published: true
date: 2021-05-01T03:07:01.587Z
tags: 
editor: markdown
dateCreated: 2021-05-01T03:06:58.343Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4135

## What changed

The `SoundNode` class (introduced in 0.8.1) is renamed to `Sound`.
The `AudioContainer` class has been added.
The actual underlying source node is no longer `SoundNode#node`, but is now `Sound#container#sourceNode`. `Sound#node` exists as a convenient reference to this which will not break (aside from the rename of `SoundNode` -> `Sound`).


## What you need to change

Change your SoundNode references to Sound

* [Unverified] 

### Research Notes

*