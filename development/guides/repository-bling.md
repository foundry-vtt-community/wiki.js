---
title: Repository Bling
description: A quick guide to tricking out your repository readme.
published: true
date: 2021-02-06T02:17:25.208Z
tags: 
editor: markdown
dateCreated: 2021-02-06T02:17:25.208Z
---

Everyone knows that the most important thing about a git repository is how much cool bling it has at the top of its readme.

![babel-bling.png](/development/guides/repo-bling/babel-bling.png)
Look at all those shields, that must be a cool project. Man I wish my projects were as cool as that one.

These little pills are called shields because by and large they come from [shields.io](https://shields.io/), which is a neat little site with an awfully hard to use interface.

In short, there's a lot of magic urls out there that make shields.io generate these neat little graphics. Here are some useful ones for Foundry package projects.

### Latest Release Download Count

Assuming your repository is on GitHub and you're following the best practices outlined in [this guide about releases](https://foundryvtt.wiki/en/development/guides/releases-and-history), you should be able to get statistics about your latest release's artifact downloads. Specifically we want the one for `module.zip` or whatever your download artifact is called, we do not want the manifest included in this number as it's downloaded every time someone checks for an update.

```
![Latest Release Download Count](https://img.shields.io/github/downloads/YOUR_USERNAME/YOUR_REPO/latest/module.zip)
```
[Source: "GitHub release (latest by SemVer and asset)"](https://shields.io/category/downloads)

Example:
![downloads-at-latest.png](/development/guides/repo-bling/downloads-at-latest.png)

You can customize the colors and such at the source if desired.

### Forge Installs

The Forge has an API which lets you know what percentage of users have added a specific module to their games. Simply replace `[YOUR_PACKAGE_NAME]` in the string below with the `name` field from your manifest and you're off to the races.

```
![Forge Installs](https://img.shields.io/badge/dynamic/json?label=Forge%20Installs&query=package.installs&suffix=%25&url=https%3A%2F%2Fforge-vtt.com%2Fapi%2Fbazaar%2Fpackage%2F[YOUR_PACKAGE_NAME]&colorB=4aa94a)
```
Example:
![forge-installs.png](/development/guides/repo-bling/forge-installs.png)

The colors have been pre-set to more or less match the Forge's branding colors.