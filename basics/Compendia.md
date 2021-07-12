---
title: Compendia
description: 
published: true
date: 2021-07-12T14:19:26.246Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:22:26.311Z
---


# Overview
Compendium Packs exist to reduce the strain on worlds that have been running for a long period of time. This includes situations where a world has accrued a large number of actors, items or scenes that, while not active, should not be deleted. Compendium Packs should be used to help you keep your world organized and reduce clutter.

Compendiums can be add in several ways. They may come with a game system directly, providing Items, Actors, or rules, such as in the DnD5e system and the SRD content.  Compendium packs are also used by modules to store large amounts of content to be unpacked and added to an existing game world, such as part of a premium or exclusive content pack.

**Note:** Any compendiums provided by a game system or module should not be used to store any user created/edited entities. When the system or module that provided the compendium is update, they will be overwritten, resulting in data loss of any user added entities. These compendiums are normally locked by default and should remain that way. 

Each Compendium can only contain one type of document: Actors, Items, Journal Entries, Macro Commands, Playlists, Rollable Tables or Scenes. Data contained in compendium packs are not loaded until needed, reducing the amount of data that a particular user must load when first joining a game.

## Creating and Using Compendium Pack

A Compendium can be easily created in any World. First, navigate to the Compendiums Sidebar Tab, and then click the `Create Compendium` button. Enter the name of the Compendium (for example "Player Characters") and choose the type of document that will be contained in from the Document Type dropdown. After your Compendium has been created, clicking upon the Compendium name in the Sidebar will open a new window that will allow you to drag and drop from a Sidebar Tab into the Compendium.

Compendium Packs and their contents are automatically sorted alphabetically.

You can also export whole folders to compendiums by right clicking the folder and clicking the `Export to Compendium` option. This then gives you the ability to choose the destination pack (the compendium you want to export the folder's entities to), and if you want to overwrite existing entries with the same names. If the `Merge by name?` option is toggled off, then new copies of similar entities will be added. This is useful for storing multiple versions of the same document in a compendium.

## Locking Compendiums
You can toggle a lock on a compendium by navigating to the Compendium Packs tab, right clicking on name of the compendium and choosing `Toggle Edit Lock`. This can help prevent making any unwanted changes.  

**Entities in compendiums can now be edited directly!**
This means you no longer have to import an entity in order to make any changes to it.

# Linking Entities

Entities in compendiums can be linked just like the ones in your game world. They can be used in Journal Entries, Map notes, or Roll Tables. Simply drag and drop the entities from the compendium to the editable filed you want to link it to.
This will show up as the standard linking format of `@Entitytype(EntityId)`. In journal any field that requires saving after editing, such as journal entries the link will not show up until after you save. Fields like roll tables, there will be no link button, only the full formated id. 

# Sharing with players

You can allow your players to have access to compendiums by hiding the compendia. Navigate to the `Compendium Packs` tab on the sidebar, then right click on a compendium. Select `Toggle Visability`. An eye symbol with a line crossed out will appear next to those that you have hidden from your players. 

This can be usefull in that you may want to let your players have access to a compendium full of spells when they need to add them, but you may not want to them to have access to one where you keep all of your monsters. 

**NOTE:** Any compendium your players have access to, they will see all of the entities within.  

# Importing Compendium Packs

If you would like to import all of the content in a compendium pack you can do so by right-clicking the pack in the Compendium sidebar then selecting the `Import All Content` option. This will open a dialog allowing you to change the name of the folder which the compendium pack's content should be imported into. It will use the compendium's name if you choose not to edit the name, or if you leave the field blank. You can also import a single entity by opening the compendium folder, right clicking on the entity, and choosing `Import Entry`. 

Once content has been imported into a game world it becomes a localized part of that world, and any changes made to the documents will not be reflected in the compendium unless you also export the content back to the compendium, or create a new compendium with the changed documents.

# Tips and Tricks

## `Do I need a compendium?`

Storing unused Entities in a Compendium **greatly** reduces the time it takes to load your world. Even though each Actor, Item, or other document may be small, as their numbers start to rise into the hundreds the amount of data that gets transferred to each of your players when they join can cause your world to slow down over time. It is a best to practice good organization.

## Sharing compendia between worlds

You can use a tiny module to share content between worlds. This reddit post by reddit user u/solfolango describes the process:

[How to create a tiny module for shared content between worlds](https://www.reddit.com/r/FoundryVTT/comments/fvw3c7/how_to_create_a_tiny_module_for_shared_content/)