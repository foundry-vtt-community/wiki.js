---
title: System Development Tutorial - Start to Finish
description: 
published: true
date: 2022-05-19T13:22:13.175Z
tags: 
editor: markdown
dateCreated: 2020-12-20T21:45:49.612Z
---

Welcome to my system development tutorial for FoundryVTT! My goal is to guide you through system development with little to no knowledge of Foundry or the languages it uses. At first we'll walk through the steps to create relatively simple systems that allow you to collect data for things like stats and attributes and calculate modifiers for them, but eventually we'll get into more advanced topics like making dice rolls from your sheet or letting items be converted into macros.

If you have any questions, you can find me on the Foundry discord as @Asacolips#1867. And with that, let's move onto to the tutorial!

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
-   Other things to do on actor sheets
    -   [Creating rollable buttons with event listeners](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD111-Creating-rollable-buttons-with-event-listeners)
    -   [Grouping items by type](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD113-Grouping-items-by-type)
    -   [Separating item types into tabs](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD114-Separating-item-types-into-tabs)
-   [Localization](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD13-Localization)
-   [Adding macrobar support for your items](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD16-Adding-macrobar-support-to-your-Items)

## Further Reading

- [Localization Best Practices](/en/development/guides/localization/localization-best-practices)
- [Understanding FormApplication](/en/development/guides/understanding-form-applications)
- [Handling Data](/en/development/guides/handling-data)

## Sections that are Work-in-Progress

In addition to the above, there are several more sections that will be added to this tutorial over time! Check back in every few weeks to see if any additions have been made. If there's a topic related to system development that you would like to see added to this list, shoot me a message on Discord!

-   Other things to do on actor sheets
    -   Creating rollable buttons with dialogs
    -   Creating item-like data structures
    -   Querying for entities with map and filter
    -   When in doubt, look at other systems
-   CONFIG and things to do with it
-   TabsV2
-   Flags
-   System Settings
-   Making things draggable in your sheet
-   Overriding core behaviors (like the combat tracker)
-   Development patterns to be aware of in the API