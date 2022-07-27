---
title: Setting up-Worlds
description: 
published: true
date: 2022-05-19T13:18:38.483Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:26:22.936Z
---

This is the World configuration screen, used to create and update worlds, as you will see it as a new Gamemaster.
![An empty World Selection screen, before any worlds have been made](https://raw.githubusercontent.com/foundry-vtt-community/wiki/master/images/Getting%20Started/Setting%20Up%20Worlds/My_First_Screen.jpg)
Click **Create World**.
 
 
![The world creation screen, with numbered steps for completion](https://raw.githubusercontent.com/evilcj925/Foundry-wiki/main/images/new-game-setup-menu.PNG)


When creating a new world, you must fill in some mandatory details. 
1. The world name is a human-readable string that may contain special characters, and this will appear in the _Game Worlds_ screen.
2. The file path to your world defines the subdirectory name within the `public/worlds` folder where your world will be saved. **⚠️ Warning ⚠️** The file path for your world will be used in web URLs, so it is best to adhere to common web standards in choosing this name. Avoid spaces or special characters, instead using hyphens or underscores to separate multiple terms.
3. You must choose the Game System that your world will use. Foundry VTT comes with some common game systems built-in, but more game systems are available through our excellent community of mod developers. To install a new game system, simply download the system definition and extract it within the `public/systems folder`. For a new system to be recognized by the software you must restart the application. **⚠️ Warning ⚠️** A world’s game system cannot be changed once the world is created, since any data created in that world will use the data model and schema for that system. Game Systems can be found here: https://foundryvtt.com/packages/systems
4. _Optional:_ Background image to be shown on world log-in. This will be seen by everyone who attemps to log in to your game world. 
5. _Optional:_ Date of next scheduled session. This can be left blank, and is intended for informantional use only.  
6. _Optional:_ Will appear as text on the World Log-in screen. Game description, rules, helpful info you want to pass to your players. Can be left blank as well.  
7. Click **Create World** to save your work and return to the _Game Worlds_ screen
