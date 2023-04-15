---
title: 01.2. Other-options
description: 
published: true
date: 2023-04-15T11:32:25.873Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:35:23.660Z
---

In addition to using the Boilerplate System, you can also use Simple World-building or a project template such as the League's various templates.

## Starting with Simple World-building system
**URL**: https://gitlab.com/foundrynet/worldbuilding

Starting with Simple World-building system (SWS) is a good option if you want to build your systems based on the latest standards that Atropos has built into FoundryVTT. The system is one of the two official systems for FoundryVTT, so you can be sure that the code in there will be a great example of how to build things The Right Way™.

If you use SWS as your base, you should search the files for any instances of `worldbuilding` or `simple` and rename them to be more appropriate for your system's name.

Something to watch out for is that SWS uses dynamic attributes for both actors and items. If your system doesn't also use those, you'll want to remove them from your html template files and make sure they're also removed from your actor and item sheet Javascript.

## Starting with a boilerplate

Another option is to use a boilerplate system as your base. These systems aren't intended to be use directly but are instead lightweight systems that include some stuff to get you started while leaving most of the work up to you.

This is the option that I recommend for most new developers, and is also the route this tutorial will follow.


### Boilerplate System
**URL**: [https://gitlab.com/asacolips-projects/foundry-mods/boilerplate](https://gitlab.com/asacolips-projects/foundry-mods/boilerplate)

The Boilerplate System is a lightweight starter set I created that includes a small number of examples (such as character ability scores) and a few helper CSS classes for laying out your sheets without actually having to write new CSS. It also includes the Sass source files I used to the compile the CSS if you're comfortable with NPM and Gulp.

We'll use this system for the tutorial. If you're comfortable with using NPM, you can also use my Yeoman generator to generate this system: [https://www.npmjs.com/package/generator-foundry](https://www.npmjs.com/package/generator-foundry). Once you have the generator installed, you just need to run `yo foundry` in your systems directory and it will prompt you for your system name before generating the files for you.

> The **generator-foundry** package mentioned above has not yet been updated for Foundry v10. For now, it's advised to instead use the Boilerplate system or League TypeScript Template instead.
{.is-warning}


### League TypeScript Template
**URL**: https://github.com/League-of-Foundry-Developers/foundry-typescript-template

If you prefer to work with TypeScript, the League of Extraordinary FoundryVTT Developers maintains several templates for new projects, such as the TypeScript project above. It's worth noting that the template linked above is for a module rather than a system, so you'll need to adapt it if you're following along this tutorial.


## Starting from scratch

You can also start completely from scratch! This is the most advanced option since you need to already know what to build, but more information can be found at [https://foundryvtt.com/article/system-development/](https://foundryvtt.com/article/system-development/).

---

* **Prev**: [Getting an empty system together](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD01-Getting-started)
* **Next**: [Stuff to be aware of](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD02-Stuff-to-be-aware-of)