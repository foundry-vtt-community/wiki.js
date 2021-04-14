---
title: Miscellaneous compatibility issues related to breaking changes in 0.8.0
description: 
published: true
date: 2021-04-14T12:38:41.621Z
tags: 
editor: markdown
dateCreated: 2021-04-14T12:38:41.621Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4577

## What changed

Blockquotes are responses from Atropos to various reports of inconsistencies.

---

Ok, so I discovered the following differences in 0.8.0:
PlaceablesLayer#layerOptions  attempts to give the sheetClass key in the returned data the value of CONFIG[this.documentName]. 0.7.x behavior was to assign this key the value of null.

> Needs further clarification: what is the issue here and what are the consequences of the change in practice?

---
Enum types, including but possibly not limited to SOURCE_TYPES, and ACTIVE_EFFECT_MODES, are no longer accessible in the global window scope, only through CONST. 0.7.x behavior appears to have allowed accessing these enums through the global window scope, in addition to through CONST.

> This was actually changed in 0.7.x where you were supposed to at that time migrate to using CONST, now it is simply required. exporting the CONST values to window was at the time, preserving backwards compatibility during 0.7.x

---
CONFIG.ActiveEffect.entityClass was renamed to CONFIG.ActiveEffect.documentClass, however no compatibility shim/getter exists for the entityClass property.

> Fixed with shim in 0.8.0

---
common/data.js::DocumentData#update appears to use the deprecated Document#_id property.

> This is correct. DocumentData still use _id for the internal database ids of all documents.

---
Document classes no longer have a toJSON method.

> Incorrect! Document#toJSON is defined and works as expected.

---
Compedium#create no longer exists.

> Known deprecation as part of the document redesign. Renamed to CompendiumCollection.createCompendium

---
FormApplication#getData uses this.object.toJSON() as the object key of the data being returned, which can fail if this.object doesn't have such a method. 0.7.x behavior was to call duplicate(this.object), which worked on most object types.

> Good catch, this behavior should only be done for the DocumentSheet class not for the base FormApplication. Will change it.

---
Backwards-compatibility breaking behavior in 0.8.0: Attempting to set a setting (via game.settings.set) in "init" or "setup" hooks throws the error, "Cannot read property 'id' of null", as the game's user entities have not yet been loaded.
Previous behavior was to not throw an error (probably fail silently). Since this is intended behavior, but also a likely source of confusion for module developers, a better error message and/or addition to the documentation would be an improvement.

> Added a more informative error message which will throw in this case.

---
Messages#flush and Messages#export use the deprecated object property, instead of the documentClass property.

> fixed

---
Ray#distance has a getter method, but no setter method. Attempts to set Ray#distance will succeed, however the underlying Ray#_distance property will remain unchanged, thus leading later accesses to Ray#distance to retrieve the unmodified Ray#_distance value.

> added a setter

---
MeasuredTemplate#direction is no longer normalized to be between 0 and 360 at the end of creation/pre-update/etc. events, leading to validation errors. 0.7.x behavior was to automatically normalize MeasuredTemplate#direction to be between 0 and 360.

> fixed by adding normalization cleaning to all ANGLE_FIELDS used throughout the data model.

---
Various properties previously set on the Compendium application have been removed/relocated to the Compendium#collection property, including but not limited to the metadata, and collection properties (that last one leading to a somewhat confusing .collection.collection access)

> This is due to the separation of the CompendiumCollection class which manages the data and the Compendium application definition which displays it. I'm happy to adjust the naming if there's a more sensible way to approach this. I will add a redirect for Compendium#metadata to CompendiumCollection#metadata, bot for the others I'm not sure what to change.

---
EntitySheetConfig #_updateObject uses the deprecated entity property.

> fixed


## What you need to change

* [Unverified] 

### Research Notes

* 