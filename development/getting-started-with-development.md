---
title: Getting Started with Package Development
description: Some common hurdles facing new Package Developers
published: true
date: 2021-02-05T16:42:49.414Z
tags: 
editor: undefined
dateCreated: 2021-02-05T16:13:36.470Z
---

> ### What is this?
> This is going to be our FAQ and "common hurdles to overcome" guide to help new package developers. It's going to be perpetually updating and changing.
> If you know of some questions or common sticking points for new package developers, please edit this to include those things.
> ### Not answer a question you have?
> The [League of Foundry VTT Developers](https://discord.gg/cudQBu8HKT) is a helpful development-focused discord community which aims to help veterans and new developers alike. Please drop by and ask us your questions, the more questions we get the more likely this document is going to be updated with their answers.
{.is-warning}

## FAQs

### Is there a list of hooks?

[There is a community maintained hook typescript definition and documentation file.](https://github.com/League-of-Foundry-Developers/foundry-vtt-types/blob/foundry-0.7.9/types/core/hooks.d.ts) But there's an easier way to discover what hook you can use for a specific piece of functionality:

```js
CONFIG.debug.hooks = true;
```

With this enabled, you'll see every hook that fires as you interact with Foundry's UI and what arguments it is passed.

## Common Hurdles and How to Overcome Them