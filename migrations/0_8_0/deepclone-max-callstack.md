---
title: deepClone - Maximum call stack size exceeded
description: 
published: true
date: 2021-02-18T21:26:36.946Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-18T20:06:10.734Z
---

# Summary

> deepClone - Maximum call stack size exceeded
{.is-danger}


When rendering sheets, `deepClone` seems to be called cyclically forever, causing overflow errors.

## What changed
The `deepClone` utility function replaced `duplicate` in foundry.js `getData` calls; the call in the ActorSheet class was cloning the object one level too high. 


## What you need to change
Per Atropos' message here: https://discord.com/channels/170995199584108546/811676497965613117/812065020238102578

Replacing this in the ActorSheet class in foundry.js:

`const data = foundry.utils.deepClone(this.object);`

with this: 

`const data = foundry.utils.deepClone(this.object.data);`

should correct the cloning issue. 


## Research Notes

* Occurs on character sheet rendering (Skimble#8601), (Bithir#2454)
* Not explictly called, seems to be internal
* Atropos noted that the ActorSheet `getData` method was cloning `object`, rather than `object.data`.