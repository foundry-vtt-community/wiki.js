---
title: Change the default behavior for what is automatically populated as ChatMessageData#content when a Roll is included in a Chat Message.
description: 
published: true
date: 2021-04-21T15:09:37.520Z
tags: 0.8.1
editor: markdown
dateCreated: 2021-04-14T12:21:41.109Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4780

## What changed
Previously there were two inconsistent approaches:

When calling Roll#toMessage the message data content would end up being roll.total

When entering a dice equation in the chat log, or via a chat macro, the message data content would end up being roll.formula.

This standardizes on approach 1, and relies upon that assumption to decide whether or not to render the custom dice roll template based on the following logic:
```js
	let hasContent = data.content && (Number(data.content) !== this.roll.total);
	if ( !hasContent ) // ... replace the HTML with the rendered dice template
```

Otherwise custom message content will be retained and displayed.

## What you need to change

* [Unverified] Ensure this logic change hasn't affected your assumptions about what is sent to chat on a roll.

### Research Notes

* This shouldn't be an impact for the average caller - the signature isn't changing, just the behavior