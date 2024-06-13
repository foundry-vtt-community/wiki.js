---
title: DialogV2
description: A lightweight Application that renders a dialog containing a form with arbitrary content, and some buttons.
published: true
date: 2024-06-13T07:25:16.746Z
tags: documentation, docs
editor: markdown
dateCreated: 2024-06-12T23:19:13.654Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

The DialogV2 class, introduced in v12 alongside [AppV2](/en/development/api/applicationv2), is a responsive and modern way to present basic choices to users.

Official documentation
- [DialogV2](https://foundryvtt.com/api/classes/foundry.applications.api.DialogV2.html)
- [DialogV2Configuration](https://foundryvtt.com/api/interfaces/foundry.DialogV2Configuration.html)
- [DialogV2Button](https://foundryvtt.com/api/interfaces/foundry.DialogV2Button.html)

**Legend**
```js
DialogV2.confirm // `.` indicates static method or property
DialogV2#render // `#` indicates instance method or property
```

## Overview

The DialogV2 class is a quick and easy way to render a simple application window with a handful of buttons. It is built upon AppV2, so it inherits the dynamic styling for both light and dark mode.

Use DialogV2 if:
- You need the user to make a simple choice
- You don't expect to re-render the application based on user actions
- The application represents a pause in a workflow that should prevent other actions

It's better to extend ApplicationV2 yourself, e.g. with `HandlebarsApplicationMixin`, if:
- You expect to handle complex form data
- You want your application to have tabs
- There isn't a clear "I'm done here" state in your process

## Key Concepts

Any usage of DialogV2 should keep the following in mind.

### Options

DialogV2 inherits all the options from [ApplicationConfiguration](https://foundryvtt.com/api/interfaces/foundry.applications.types.ApplicationConfiguration.html), the most notable of these is the `classes` array which can be helpful for specifying the styling of your dialogs. In addition, the options from [DialogV2Configuration](https://foundryvtt.com/api/interfaces/foundry.DialogV2Configuration.html) are all important to implement; `buttons` and `content` especially are where you usually do the most work to configure the dialog. The only automatic [localization](/en/development/api/localization) for these properties is the `label` of any button; use template strings and calls to `game.i18n.localize` for defining your `content`.

## API Interactions

There are a few basic ways of invoking DialogV2 that can simplify your code.

## Specific Use Cases

## Troubleshooting