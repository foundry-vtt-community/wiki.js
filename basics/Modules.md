---
title: Modules
description: 
published: true
date: 2025-07-26T22:03:07.374Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:22:50.901Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

Modules enhance or replace functionality within Foundry VTT. The simplest modules consist of a `module.json` file that describes the module and contains some metadata about it, and a `<module name>.js` file that contains the module code; but a module can include other types of resources depending on what it needs to do, like additional `js` files (usually imported as ES [modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)), `css` files for styling, or `html` and `hbs` application templates.

## Finding Modules
Browsing the official module list is the easiest way to find Modules. The website has a dedicated [search page](https://foundryvtt.com/packages/modules), but you can also directly look for available modules in Foundry's setup screen, by going to the Add-on Modules tab and clicking the Install Module button.

## Installing Modules

### Via the Module browser
1. Open the Foundry setup page in a web browser
2. Click on the Add-on Modules tab
3. Click the Install Module button
4. Find the module you want to install and click its Install button

### Via Manifest URL
1. Open the Foundry setup page in a web browser
2. Click on the Add-on Modules tab
3. Click the Install Module button
4. Paste the manifest URL for the module being installed in the field at the bottom of the dialog
5. Click Install 

### Manually
1. Place the module files under `Data\modules\<module name>` inside your Foundry VTT user data folder
2. Restart the Foundry VTT server

Regardless of the installation method, a module will also need to be activated in a world to be used. Only a GM user can activate or deactivate modules.
1. Log back into Foundry VTT as a Gamemaster
2. Open the Game Settings tab
3. Click the Manage Modules button
4. Find the newly installed module and place a checkmark next to the name, then click Save Module Settings at the bottom of the list to activate it


## Developing Modules
There is an [Introduction to Module Development](https://foundryvtt.com/article/module-development/) page in the official Foundry Knowledge Base which is a good place to start.

Modules are developed using the Foundry VTT API which is documented on the official [Foundry VTT site](https://foundryvtt.com/api/).

