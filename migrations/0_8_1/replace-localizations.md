---
title: Replace localizations which previously encoded HTML "Are You Sure" messages as two separate localization strings.
description: 
published: true
date: 2021-04-21T16:50:04.773Z
tags: 
editor: markdown
dateCreated: 2021-04-14T12:30:05.261Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4750

## What changed

Localization Strings were split up:

AreYouSure
CHAT.FlushWarning
COMPENDIUM.DeleteConfirm (renamed to COMPENDIUM.DeleteWarning)
COMPENDIUM.DeleteHint (renamed to COMPENDIUM.DeleteEntryWarning)
MACRO.DeleteConfirm (renamed to MACRO.DeleteWarning)
SIDEBAR.DeleteConfirm (renamed to SIDEBAR.DeleteWarning)
FOLDER.DeleteConfirm (renamed to FOLDER.DeleteWarning)
FOLDER.RemoveConfirm (renamed to FOLDER.RemoveWarning)


## What you need to change

* [Unverified] Update any of your code relying on these localization strings to instead use the new ones.

### Research Notes
