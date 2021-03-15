---
title: Tidy up SidebarDirectory class definitions to reference "documents" rather than "entities"
description: 
published: true
date: 2021-02-07T17:46:30.609Z
tags: 
editor: undefined
dateCreated: 2021-02-07T17:46:27.195Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4503

## What changed

A lot of `SidebarDirectory` class definitions are renamed for consistency sake.

## What you need to change

- [Unverified] ensure you're not referencing an old template with a new name
- [Unverified] `SidebarDirectory#entity` -> `SidebarDirectory.documentName`
- [Unverified] `SidebarDirectory#entities` -> `SidebarDirectory#documents`

### Research Notes

- 