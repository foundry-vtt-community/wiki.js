---
title: Journal
description: 
published: true
date: 2021-07-09T09:28:54.501Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:22:38.240Z
---

The journal is where you can take all sorts of notes and prepare your handouts. They are not limited to text and can both insert images directly inside the text with the rich text field, plus each entry can also be assigned a main image that can be displayed as a standalone handout, useful for showcasing that awesome artwork of your city! As with actors, scenes and items, you can organize them into folders to keep them organized.

# Linking Entities
One of Foundry's feature allows you to link entities in the various rich text editor in the platform. However, the synthax is often forgotten, so here it is: **`@EntityType[EntityId]`**. `{displayed text}`(spaces ok) can be added to the end to change the text of the link. There are currently five `EntityType` supported like such:
* Actors
* Items (Reminder that in Foundry, this applies to many different things, including spells, classes and feats!)
* JournalEntry (no space)
* Compendium
* Scene

example: `@Scene[GoXvkXb0l1aJLYjw]{The Market Square}` would link to scene with the scene_id of `GoXvkXb0l1aJLYjw`. `The Market Square` would be the displayed text of the link if you want it to be different than the name of the entity. 

## Troubleshooting
**You can now link directly from a compendium!** 

The link won't actually appear as a link in the **edit mode**, you have to save your work to get out of edit mode to be able to click the link. Once out of edit mode, it should display as a link with a little icon to the side of it. If it still appears as `@EntityType[NameOfEntity]`, something else breaks the link. 

Lastly, be sure that the part between the brackets [ ] corresponds exactly to the entry you want to link. Some **special characters** (spaces are fine) in the name of the entry might break the link as well, in which case you'll have to rename the entry you want to link before you do so.

# Journal Pins
Foundry VTT also allows the creation of map pins on the map. These are links to journal entries that can be placed directly on the map, useful for keeping track of landmarks and various locations you have a journal entry for. These pins also inherit the permission level of their journal entry and will be visible to any player with at least `Limited` access to the entry.


# Sharing
## Permissions
Similarly to other types of data in Foundry VTT, you can give different levels of permission to your players. As such, you can keep some campaign notes for yourself private, let them see but not edit your rules, and have ownership of an entry so they can take notes as they progress in your campaign.

## "Show Players" Option
This is a "one time show" option to open a handout for your players, which will appear on their screen automatically. This is a great way both to showcase artwork at a specific time or to simply show something you don't want your players to have permanent access to, since it doesn't rely on permissions. 

**There have been mentions of showing players an entry would change the permission level of the entry to limited automatically, but I couldn't reproduce.