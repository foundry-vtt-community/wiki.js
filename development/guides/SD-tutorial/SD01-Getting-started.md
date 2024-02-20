---
title: 01. Getting Started
description: 
published: true
date: 2024-02-20T02:39:23.187Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:35:18.520Z
---

![Foundry v11](https://img.shields.io/badge/Foundry-v11-green)

When building a new system, you have several options to choose from. You can copy an existing system (depending on its license) like the Simple World-building system, you can use a system generator, or you could start from scratch and have total control.

## Option 1: Use the Boilerplate generator

First, clone the Boilerplate repository

```bash
git clone git@gitlab.com:asacolips-projects/foundry-mods/boilerplate.git
```

Second, install the dependencies for the generator. You'll need [node.js](https://nodejs.org/en) installed, ideally version 20 or higher. Once you have node, execute the following from inside the `boilerplate` directory.

```bash
npm install
```

Finally, run the generator. It will prompt you to name your system in several different ways, like `my-system` for your package name and `MySystem` for class names.

```bash
npm run generator
```

This will create a generated version of Boilerplate that handles all of the filename and text replacements for you. The generated version will be saved to `boilerplate/build/<your-system-name` and then you can move the `<your-system-name>` directory out of the `build` directory and into your `/Data/systems` directory of your Foundry userData folder.

## Option 2: Boilerplate with manual replacements


1. Download a zip version of the [Boilerplate System](https://gitlab.com/asacolips-projects/foundry-mods/boilerplate/-/archive/master/boilerplate-master.zip)
2. Extract it to the `/Data/systems` directory in your Foundry userData folder and rename it from `boilerplate-master` to your system's name (lowercase, machine-safe).
3. Search the directory for any files with the filename `boilerplate` and rename them to your system's machine-safe name, such as `mysystemname`.
4. Search the files for any occurrences of `Boilerplate` (case-sensitive) and replace those with a capitalized version of your system name that's still machine-safe, such as `MySystemName`. These are typically used for the classes in the Javascript files.
5. Search the files for any occurrences of `boilerplate` (case-sensitive) and replace those with a lowercase machine-safe version of your system name, such as `mysystemname`.
6. If the replace instructions from step 4 also included updating your description in system.json, you should edit that to be more readable now, such as "My System Name".


From here, you can skip to the next page or keep reading if you'd like to read about the other options for starting a new system.

## Other options

If you'd like to see other options for getting started with building a new system to start from, see the [Other options](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD012-Other-options) page.

---

* **Next**: [Stuff to be aware of](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD02-Stuff-to-be-aware-of)