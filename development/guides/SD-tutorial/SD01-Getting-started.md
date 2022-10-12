---
title: 01. Getting Started
description: 
published: true
date: 2022-10-12T21:37:04.879Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:35:18.520Z
---

When building a new system, you have several options to choose from. You can copy an existing system (depending on its license) like the Simple World-building system, you can use a system generator, or you could start from scratch and have total control.

## Option 1: Download

1. Download a zip version of the [Boilerplate System](https://gitlab.com/asacolips-projects/foundry-mods/boilerplate/-/archive/master/boilerplate-master.zip)
2. Extract it to the `/Data/systems` directory in your Foundry userData folder and rename it from `boilerplate-master` to your system's name (lowercase, machine-safe).
3. Search the directory for any files with the filename `boilerplate` and rename them to your system's machine-safe name, such as `mysystemname`.
4. Search the files for any occurrences of `Boilerplate` (case-sensitive) and replace those with a capitalized version of your system name that's still machine-safe, such as `MySystemName`. These are typically used for the classes in the Javascript files.
5. Search the files for any occurrences of `boilerplate` (case-sensitive) and replace those with a lowercase machine-safe version of your system name, such as `mysystemname`.
6. If the replace instructions from step 4 also included updating your description in system.json, you should edit that to be more readable now, such as "My System Name".

## Option 2:  Generate with npm

> The **generator-foundry** package has not yet been updated for Foundry v10. For now, it's advised to instead use the Boilerplate system or League TypeScript Template instead.
{.is-warning}


You'll need to be comfortable with your terminal/command line and have npm installed. If you are, run the following two commands to install the CLI generator version of Boilerplate (https://www.npmjs.com/package/generator-foundry):


```bash
npm install -g yo
npm install -g generator-foundry
```

Alternatively, you can install it locally to a directory with `npm install yo generator-foundry --save` and then execute commands with `npx` instead.


After that, you just need to open your `/Data/systems` directory in your Foundry userData folder in your terminal and run


```bash
yo foundry
```

to generate your system. The generator will ask you what your system name should be and will provide examples. In addition, it will prompt you to enable optional features such as Active Effects and Item macros, or examples such as ability score modifiers and spell items.


From here, you can skip to the next page or keep reading if you'd like to read about the other options for starting a new system.

## Other options

If you'd like to see other options for getting started with building a new system to start from, see the [Other options](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD012-Other-options) page.

---

* **Next**: [Stuff to be aware of](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD02-Stuff-to-be-aware-of)