---
title: Document pre-create hook events are now provided with the candidate Document instance instead of merely the raw data object provided for the creation.
description: 
published: true
date: 2021-02-07T17:28:42.634Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:28:42.634Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4435

## What changed
API Documentation detailing the new hookEvents: https://foundryvtt.com/api/alpha/hookEvents.html

Document pre-create hooks get the whole entity instance instead of just the data.

## What you need to change

- [Unverified] Any pre-create hook should expect the full instance and not just the data

### Research Notes

- 