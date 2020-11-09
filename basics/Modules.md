---
title: Modules
description: 
published: true
date: 2020-11-09T19:39:52.047Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:22:50.901Z
---

Modules enhance or replace functionality within Foundry VTT. Most modules consist of a `module.json` file that describes the module and contains some metadata about it, and a `<module name>.js` file that contains the module code. Some modules also include `css` and `html` files.

# Finding Modules
Browsing the official module list is the easiest way to find Moudules. Some unlisted modules can be found via the [Unlisted Modules](/en/community/community-unlisted-modules) wiki page.

# Installing Modules

## Via Manifest URL
1. Open the Foundry setup page in a web browser
2. Click on the Add-on Modules tab
3. Click the Install Module button
4. Either browse the official listing or Paste the manifest URL to the module being installed in the field at the bottom of the dialog
5. Click Install 

## Manually
1. Placing the module files under `Data\modules\<module name>` inside your Foundry VTT user data folder. 
2. Restart the Foundry VTT server
3. Log back into Foundry VTT as the GameMaster
4. Open the Game Settings tab
5. Click the Manage Modules button
6. Find the newly installed module and place a checkmark next to the name, then click Update Modules at the bottom of the list to activate it


# Developing Modules
There is an [Introduction to Module Development](https://foundryvtt.com/article/module-development/) page in the official Foundry Knowledge Base which is a good place to start.

Modules are developed using the Foundry VTT API which is documented on the official [Foundry VTT site](https://foundryvtt.com/api/).

