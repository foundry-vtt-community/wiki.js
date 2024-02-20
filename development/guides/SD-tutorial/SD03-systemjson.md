---
title: 03. system.json
description: 
published: true
date: 2024-02-20T02:49:31.348Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:35:35.124Z
---

![Foundry v11 Compatible](https://img.shields.io/badge/Foundry-v11%20Compatible-blue)

Once you've created your system, you'll need to make some changes to system.json. Here's what it looks like for the Boilerplate System by default:

```json
{
  "id": "boilerplate",
  "title": "Boilerplate",
  "description": "The Boilerplate system for FoundryVTT!",
  "authors": [
    {
      "name": "Asacolips",
      "discord": "asacolips"
    }
  ],
  "url": "Replace this with a link to your public repository",
  "license": "LICENSE.txt",
  "readme": "README.md",
  "bugs": "Replace this with a link to file issues or tickets",
  "changelog": "CHANGELOG.md",
  "media": [
    {
      "type": "setup",
      "url": "systems/boilerplate/assets/anvil-impact.png",
      "thumbnail": "systems/boilerplate/assets/anvil-impact.png"
    }
  ],
  "version": "1.3.0",
  "compatibility": {
    "minimum": 11,
    "verified": "11.315"
  },
  "esmodules": ["module/boilerplate.mjs"],
  "styles": ["css/boilerplate.css"],
  "languages": [
    {
      "lang": "en",
      "name": "English",
      "path": "lang/en.json"
    }
  ],
  "packs": [],
  "packFolders": [],
  "socket": false,
  "manifest": "Replace this with a link to the /latest release system.json so people can receive updates after they've installed the system",
  "download": "This must link to a .zip file of the current version",
  "background": "systems/boilerplate/assets/anvil-impact.png",
  "gridDistance": 5,
  "gridUnits": "ft",
  "primaryTokenAttribute": "health",
  "secondaryTokenAttribute": "power"
}

```

For a full breakdown of each of those properties head to [https://foundryvtt.com/article/system-development/](https://foundryvtt.com/article/system-development/). But there a few important ones to take a look at in detail:

* **id**: Your system's machine-safe name, such as `mysystemname` or `dnd5e`. In previous versions of Foundry, this was called `name`.
* **url**: The link to your system's homepage, such as the Github or Gitlab page for it.
* **license**: URL to your system's license information, if any. It's recommended that you provide some sort of license (such as GPL, MIT, or general copyright notices) to remove ambiguity in case others want to contribute to your code or take over maintenance in the future. For example, the Boilerplate system used in this tutorial is using the MIT license.
* **version**: Your system's version. This should match the git tag you create for this release if you're using git, and see the **Managing Releases** section below for more details on how to best manage versions for your system.
* **compatibility**: An object with your system's compatibility requirements. The properties on this object are:
	* **minimum**: The minimum core version of Foundry required to install this system. If there are API changes in Foundry itself that you have to make significant updates for, you'll want to update this number. This can be a general version like "10" or a specific version, like "10.288".
	* **verified**: The most recently tested version of Foundry for this system. If Foundry is newer than the version listed here, users will receive a warning when trying to install or use your system. You'll need to update this frequently, but it can be done through foundryvtt.com's package manager so that you don't have to create new releases. It's recommended to put specific versions here, like "10.288".
	* **maximum**: The maximum compatible version of Foundry this should be allowed to run in. For example, if you set this to "10", it can't be run in Foundry v11 at all. If you'd like to allow users to attempt to run this in future versions of Foundry, you can leave this field out.
* **esmodules**: An array of Javascript files to import as ES modules. If you need to add multiple files, you can do a comma separated list inside the `[]` brackets.
* **styles**: An array of CSS files to use for your system's styling.
* **scripts**: An array of Javascript files to include in your system. These files will not use the export/import syntax that ES modules use.
* **languages**: Any language definitions that you create will need to be listed here. Boilerplate System comes with an included `en.json` language file.
* **packs**: Compendium packs. Each pack will be an object where you specify the name, system, path, and entity type. The `dnd5e` system has several great examples of creating a compendium, and more info can be found at [https://foundryvtt.com/article/compendium/](https://foundryvtt.com/article/compendium/).
* **packfolders**: An array of folders to generate on the first load of a world for your provided compendia. Updates to this will NOT affect existing worlds. You can reset a world's compendium folders to default by running `game.settings.set("core", "compendiumConfiguration", {})` in the console.
* **manifest**: The raw link to this `system.json` file. Foundry uses this to find out information about your system for update and install purposes.
* **download**: The raw link to download a zip file of your system. This can be built via custom build tools, but Github and Gitlab both generate zip files from your master branch that you can use here.

> **Managing Releases**
> To properly manage releases, you need to do three things:
> (1) Use the link to the latest version (such as "main" or "master") of your system in the `manifest` property of system.json. This will allow updates to be found by Foundry. If you have multiple release tracks such as `v9` and `v10` for versions compatible with the respective Foundry versions, use that in the manifest URL.
> (2) Use the link to the **tagged** version (such as "1.0.0") of your system in the package listing on foundryvtt.com when making a new release. This will allow Foundry find and install specific versions (which is important for users who don't update immediately and stay on the previous stable version).
> (3) Use the link to the **tagged** version (such as "1.0.0") of your system in the `download` property of your system.json, which allow the manifest to find and download a specific version (and works in tandem with #2 above about using tagged versions on foundryvtt.com).
>
> For example, when using Gitlab, your URLs for the manifest and download properties of your system.json file might look like the following if your development branch was called **main** and your release version was **1.0.0**:
>
> `"manifest": "https://gitlab.com/asacolips-projects/foundry-mods/boilerplate/-/raw/main/system.json",`
> `"download": "https://gitlab.com/asacolips-projects/foundry-mods/boilerplate/-/archive/1.0.0/boilerplate-1.0.0.zip",`
> &nbsp;
{.is-info}


---

* **Prev:** [Stuff to be aware of](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD02-Stuff-to-be-aware-of)
* **Next:** [template.json](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD04-templatejson)