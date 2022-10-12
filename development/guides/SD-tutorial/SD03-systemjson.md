---
title: 03. system.json
description: 
published: true
date: 2022-10-12T21:39:14.334Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:35:35.124Z
---

> **Not Updated for Foundry v10**
>
> This section of the system development tutorial has not yet been updated for Foundry v10+ versions. While the general concepts are still applicable, it's recommended that you review the equivalent section of the Boilerplate system used in the tutorial for differences (the system itself has been updated for v10).
> https://gitlab.com/asacolips-projects/foundry-mods/boilerplate/-/tree/master
{.is-warning}

Once you've created your system, you'll need to make some changes to system.json. Here's what it looks like for the Boilerplate System by default:

```json
{
  "name": "boilerplate",
  "title": "Boilerplate",
  "description": "The Boilerplate system for FoundryVTT!",
  "version": "1.1.0",
  "minimumCoreVersion": "0.8.3",
  "compatibleCoreVersion": "0.8.6",
  "author": "Asacolips",
  "esmodules": ["module/boilerplate.mjs"],
  "styles": ["css/boilerplate.css"],
  "scripts": [],
  "packs": [],
  "languages": [
    {
      "lang": "en",
      "name": "English",
      "path": "lang/en.json"
    }
  ],
  "gridDistance": 5,
  "gridUnits": "ft",
  "primaryTokenAttribute": "health",
  "secondaryTokenAttribute": "power",
  "url": "https://gitlab.com/asacolips-projects/foundry-mods/boilerplate",
  "manifest": "https://gitlab.com/asacolips-projects/foundry-mods/boilerplate/-/raw/master/system.json",
  "download": "https://gitlab.com/asacolips-projects/foundry-mods/boilerplate/-/archive/1.1.0/boilerplate-master.zip",
  "license": "LICENSE.txt"
}

```

For a full breakdown of each of those properties head to [https://foundryvtt.com/article/system-development/](https://foundryvtt.com/article/system-development/). But there a few important ones to take a look at in detail:

* **name**: Your system's machine-safe name, such as `mysystemname` or `dnd5e`.
* **version**: Your system's version. This should match the git tag you create for this release if you're using git, and see the **Managing Releases** section below for more details on how to best manage versions for your system.
* **minimumCoreVersion**: The minimum core version of Foundry required to install this system. If there are API changes in Foundry itself that you have to make significant updates for, you'll want to update this number.
* **compatibleCoreVersion**: The most recently tested version of Foundry for this system. If Foundry is newer than the version listed here, users will receive a warning when trying to install or use your system. You'll need to update this frequently.
* **esmodules**: An array of Javascript files to import as ES modules. If you need to add multiple files, you can do a comma separated list inside the `[]` brackets.
* **styles**: An array of CSS files to use for your system's styling.
* **scripts**: An array of Javascript files to include in your system. These files will not use the export/import syntax that ES modules use.
* **packs**: Compendium packs. Each pack will be an object where you specify the name, system, path, and entity type. The `dnd5e` system has several great examples of creating a compendium, and more info can be found at [https://foundryvtt.com/article/compendium/](https://foundryvtt.com/article/compendium/).
* **languages**: Any language definitions that you create will need to be listed here. Boilerplate System comes with an included `en.json` language file.
* **url**: The link to your system's homepage, such as the Github or Gitlab page for it.
* **manifest**: The raw link to this `system.json` file. Foundry uses this to find out information about your system for update and install purposes.
* **download**: The raw link to download a zip file of your system. This can be built via custom build tools, but Github and Gitlab both generate zip files from your master branch that you can use here.

> **Managing Releases**
> To properly manage releases, you need to do three things:
> (1) Use the link to the latest version of your system in the `manifest` property of system.json. This will allow updates to be found by Foundry.
> (2) Use the link to the **tagged** version of your system in the package listing on foundryvtt.com when making a new release. This will allow Foundry find and install specific versions (which is important for users who don't update immediately and stay on the previous stable version).
> (3) Use the link to the **tagged** version of your system in the `download` property of your system.json, which allow the manifest to find and download a specific version (and works in tandem with #2 above about using tagged versions on foundryvtt.com).
>
> The example **system.json** snippet above shows an example of how this structure would look. Review it closely when working on your own system.json file during releases.
{.is-warning}


---

* **Prev:** [Stuff to be aware of](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD02-Stuff-to-be-aware-of)
* **Next:** [template.json](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD04-templatejson)