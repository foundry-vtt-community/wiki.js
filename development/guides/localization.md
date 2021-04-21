---
title: Localization
description: Localization guides.
published: true
date: 2021-04-21T15:03:40.601Z
tags: untranslatable
editor: markdown
dateCreated: 2021-01-14T08:29:55.485Z
---

Helpful articles, best practices, tips, and tricks for translating core Foundry VTT and its packages.

## Currently Untranslatable Elements
Foundry Virtual Tabletop is not yet fully localizable. Parts of the user interface still lack translation support and some elements have been confirmed by the developers to not have translation support for the foreseeable future.

As of Foundry VTT 0.7.9, the following elements of the UI have been confirmed to not support localization in the near future:
- Error notification messages shown when attempting to create new entities with an empty name. See [GitLab issue #4472](https://gitlab.com/foundrynet/foundryvtt/-/issues/4472). For example the notification message:
> Required field name not present in JournalEntry.

Message by [Andrew](https://gitlab.com/aaclayton):

> These error messages are provided by server-side database validation and forwarded to the client so that there is some record of why a certain operation failed. Unfortunately localizing these sorts of error messages is not going to be a feasible undertaking (for any time in the forseeable future). This one is going to have to fall out of scope.