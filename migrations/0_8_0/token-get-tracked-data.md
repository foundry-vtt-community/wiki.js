---
title: Improve TokenConfig.getTrackedAttributes to differentiate between "base" attributes which can be edited and "derived" attributes which are deterministic. Prevent the editing derived tracked attributes through the Token HUD.
description: 
published: true
date: 2021-04-21T15:05:10.687Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T16:54:35.763Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4205

## What changed

Something will have changed to indicate an attribute is read-only and thus can't be edited by the HUD.

## What you need to change

- [Unverified]

### Research Notes

- 