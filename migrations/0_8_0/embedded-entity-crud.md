---
title: EmbeddedEntity create, update, and delete hooks are now passed the EmbeddedEntity document instance instead of only the raw object data involved in the operation.
description: 
published: true
date: 2021-04-21T15:05:50.594Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:26:27.916Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4434

## What changed

API Documentation detailing the new hookEvents: https://foundryvtt.com/api/alpha/hookEvents.html

CRUD hooks for EmbeddedEntities are now passed the whole instance instead of just json data.

## What you need to change

- [Unverified] Any hook related to embeddedEntity should expect to get the whole class instead of just the data

### Research Notes

- 