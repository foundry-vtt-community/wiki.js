---
title: Migrate to Node.js 14.x which is now REQUIRED in order to utilize server-side ESModules and V8s.
description: 
published: true
date: 2021-02-07T16:38:44.453Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T16:38:40.746Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/3545

## What changed

Node version 14 is now required for serverside esmodules and v8s.

## What you need to change

* Ensure your hosting uses Node 14

### Research Notes

* If you use the bundled application this will not affect you.
* This should be simple enough to do with docker.