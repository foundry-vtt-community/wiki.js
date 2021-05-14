---
title: Move calls to Document#prepareData outside of Document#_onUpdate to avoid cases where a system developer overrides this event handler and forgets to re-prepare Document data.
description: 
published: true
date: 2021-04-21T16:45:05.350Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:37:38.736Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4458

## What changed

`Document#prepareData` is no longer called inside `Document#_onUpdate`

## What you need to change

- [Unverified] you can stop calling `prepareData` inside an overriden `_onUpdate`

### Research Notes

- 