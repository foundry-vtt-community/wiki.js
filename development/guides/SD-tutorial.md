---
title: System Development Tutorial (Boilerplate)
description: 
published: true
date: 2024-02-20T02:58:52.399Z
tags: 
editor: markdown
dateCreated: 2020-12-20T21:45:49.612Z
---

Welcome to the system development tutorial for FoundryVTT! Our goal is to guide you through system development with little to no knowledge of Foundry or the languages it uses. At first we'll walk through the steps to create relatively simple systems that allow you to collect data for things like stats and attributes and calculate modifiers for them, but eventually we'll get into more advanced topics like making dice rolls from your sheet or letting items be converted into macros.

This tutorial is written to take advantage of the accompanying [Boilerplate](https://gitlab.com/asacolips-projects/foundry-mods/boilerplate) system, but if you'd rather start from scratch or use a different base system, the concepts should still be helpful!

If you have any questions, you can find me on the Foundry discord as @Asacolips#1867. And with that, let's move onto to the tutorial!

> **Foundry Compatibility Tags**
> Throughout this guide you'll see tags at the top of each page listing the most recent of Foundry the page was updated for. If the guide supports multiple versions of Foundry, such as both v11 and v12, multiple tags will be shown on the page. Currently, this guide has been updated for Foundry v11.
>
> ![Foundry v11 Compatible](https://img.shields.io/badge/Foundry-v11%20Compatible-blue)
>
> If the guide is out of date and has known incompatibilities with the current Foundry version, you'll see a red indicator like the following:
>
> ![Foundry v13 Incompatible](https://img.shields.io/badge/Foundry-v13%20Incompatible-red)
{.is-info}


-   [Getting an empty system together](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD01-Getting-started)
    -   Start with a boilerplate system
    -   Using SWS
    -   Using a CLI
-   [Stuff to be aware of](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD02-Stuff-to-be-aware-of)
    -   Scripts or ES6 modules?
    -   CSS preprocessors
    -   Handlebars
    -   What about 3rd party libraries?
    -   A Note About Localization
-   [system.json](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD03-systemjson)
-   [template.json](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD04-templatejson)
    -   Defining actor and item types
-   [Creating your main system javascript file](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD05-Creating-your-main-JS-file)
-   [Extending the Actor class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD06-Extending-the-Actor-class)
    -   Basic data vs. derived data
    -   Creating derived values with prepareData()
    -   \_prepareCharacterData()
-   [Extending the ActorSheet class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD07-Extending-the-ActorSheet-class)
    -   defaultOptions
    -   getData()
    -   activateListeners()
-   [Creating HTML templates for your actor sheets](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD08-Creating-HTML-templates-for-your-actor-sheets)
-   [Extending the Item class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD09-Extending-the-Item-class)
-   [Extending the ItemSheet class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD10-Extending-the-ItemSheet-class)
-   [Creating rollable buttons with event listeners](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD111-Creating-rollable-buttons-with-event-listeners)
-   [Localization](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD13-Localization)

## Further Reading

- [Localization Best Practices](/en/development/guides/localization/localization-best-practices)
- [Understanding FormApplication](/en/development/guides/understanding-form-applications)
- [Handling Data](/en/development/guides/handling-data)