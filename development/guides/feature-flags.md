---
title: Using Feature Flags
description: 
published: true
date: 2021-03-02T14:45:02.524Z
tags: guide
editor: markdown
dateCreated: 2021-02-07T23:30:36.770Z
---

> # Feature Flags removed in Foundry Core 0.8.0
> This page should be considered archived.
{.is-warning}

# Using Feature Flags
[Feature Flags](https://gitlab.com/foundrynet/foundryvtt/-/issues/3959) allow a package to granularly check if certain chunks of code should be executed based on if the functionality is present.

A common usecase for this is when a new Foundry release is available, and the developers want to release a version of their package which can target both the new features and the older release.

In the past, this would have required coarsely checking against the Foundry core version.

## An simple example

A developer is writing a package that has some new functionality taking advantage of Active Effects, but still have a good portion of userbase that is staying on older versions of Foundry.

The developer can check that AE is present like so:

```js
if (window.FEATURES.ACTIVE_EFFECTS) {
  // Do some cool AE stuff"
}
```

## Checking Feature version

Eventually, all things change, and Active Effects rolls out a new change. Feature Flags contain a simple int version indicating what major version they are on.

```js
if (window.FEATURES.ACTIVE_EFFECTS == 2) {
  // Do some cool AE stuff"
}
else if (window.FEATURES.ACTIVE_EFFECTS == 1) {
  // Do some cool older AE stuff"
}
else {
  // No AE available, probably ask the user to update Foundry
}
```


## Extending Features

Core ships with the following Features:

```
 ACTIVE_EFFECTS
 ACTORS
 AUDIO_VIDEO
 CHAT
 COMPENDIUM
 DRAWINGS
 ENTITIES
 ITEMS
 JOURNAL
 LIGHTING
 MACROS
 NOTES
 ROLL_TABLES
 SETTINGS
 SOUND
 TEMPLATES
 TILES
 TOKENS
 WALLS
```

A package can add to this list, or adjust what's already there - for instance, a System may keep their actor sheet versioned, and that System may have not bitten the bullet on their AE support, meaning that even if Foundry supports it, a Module couldn't use them on that System - they wouldn't apply. The system can add and disable the Features at runtime:

```js
Hooks.once('ready', function() {
  window.FEATURES.SYSTEM_CHARACTER_SHEET = 2;
  window.FEATURES.ACTIVE_EFFECTS = 0;
}
```