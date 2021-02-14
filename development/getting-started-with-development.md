---
title: Getting Started with Package Development
description: Some common hurdles facing new Package Developers
published: true
date: 2021-02-14T00:50:17.280Z
tags: 
editor: markdown
dateCreated: 2021-02-05T16:13:36.470Z
---

> ### What is this?
> This is going to be our FAQ and "common hurdles to overcome" guide to help new package developers. It's going to be perpetually updating and changing.
> If you know of some questions or common sticking points for new package developers, please edit this to include those things.
> ### Have a question not answered here?
> The [League of Foundry VTT Developers](https://discord.gg/cudQBu8HKT) is a helpful development-focused discord community which aims to help veterans and new developers alike. Please drop by and ask us your questions, the more questions we get the more likely this document is going to be updated with their answers.
{.is-warning}

## FAQs

### Is there a list of hooks?

[There is a community maintained hook typescript definition and documentation file.](https://github.com/League-of-Foundry-Developers/foundry-vtt-types/blob/foundry-0.7.9/types/core/hooks.d.ts) But there's an easier way to discover what hook you can use for a specific piece of functionality:

```js
CONFIG.debug.hooks = true;
```

With this enabled, you'll see every hook that fires as you interact with Foundry's UI and what arguments it is passed.

### What is a flag and how do I use them?

> [stub](https://github.com/VanceCole/macros/blob/master/flags.js)

### How do I work with settings?

> [stub](https://github.com/VanceCole/macros/blob/master/settings.js)

## Common Hurdles and How to Overcome Them


### Storing Module Data

> - Flags
> - Settings
> - Pros and Cons of the two

## Hello World Module Walkthrough

> ### [Stub](https://www.reddit.com/r/restofthefuckingowl/)
> https://foundryvtt.com/article/module-development/
> 1. Create your directory
> 1.a Symlink from a /dist directory
> 2. Common gotchyas with manifest.json and file structure
> 3. Include a script file
> 6. Include localization
> 4. Include a template
> 5. Include CSS
> 6. ???
> 7. Profit