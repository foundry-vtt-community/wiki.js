---
title: Active Effects Primer
description: 
published: true
date: 2022-05-19T13:24:34.597Z
tags: 
editor: markdown
dateCreated: 2021-03-02T01:26:07.563Z
---

This page is a stub. If you know the answer to any of these questions, please help us out by filling it out!

> Information known to be up to date as of Foundry Core 0.7.9
{.is-info}

---

> - What is an Active Effect? Is it just a class that can be added an Item/Actor?

An active effect is a type of `EmbeddedEntity` that can be owned by `Actor`s or `Item`s. In code, it takes the form of the [`ActiveEffect`](https://foundryvtt.com/api/ActiveEffect.html) class.

---

> - How do we create Active Effects? Are macros needed?

Macros *can* create active effects, but they are not required to create them. Many systems implement a way to add a blank active effect to an actor or item through their respective sheets. Core foundry provides a sheet class, [`ActiveEffectConfig`](https://foundryvtt.com/api/ActiveEffectConfig.html), which can be used to configure the details of an active effect, however it is up to the system to invoke this sheet when appropriate for that system.

---

> - Can Active Effect exist independently of an Item/Actor?

No, active effects exist solely as "embedded" entities within actors or items.

> The `CONFIG.statusEffects` array contains a pre-set list of effects that can be applied to a token through the Token HUD UI. These status effects can contain `ActiveEffect` data, however these are not `ActiveEffect` instances, just base data from which an `ActiveEffect` can be created when the icon is applied to the token.
{.is-info}

---

> - Can Items/Actors be exported or imported along with their Active Effect (to create enchanted objects, for example).

Yes, when an actor or item is exported it will include any active effects on that actor or item.

---

> - How do Active Effects modify attributes? Do you just add them to an Item/Actor or is there more to do?

Active effects will modify the attributes of the actor they are owned by, *unless* the effect is indicated as `disabled`. Active effects owned by items don't actually modify anything, however they can be set up to transfer to the actor that owns the item. In this case, the active effect is copied to the owning actor where it can have its effect.

Technically, Active Effects *could* modify Item data, if you implement this in a system or module (in the same way that core implements applying Active Effects to Actors). This functionality is not provided by core, however, and currently there doesn't seem to be a module or system that implements it. 

---

> - How do I define which attribute(s) are modified by an Active Effect?

Via the UI, you can add a new change which requires an "Attribute Key", which denotes what of the owning actor's data should be modified by the active effect. The attribute key indicates the data path to the data member which should be modified, for instance "data.abilities.dex.value", which, in the dnd5e system, would modify the actor's dexterity score.

In code, these changes are stored in the `changes` array in the `ActiveEffect`'s data.

---

> - Where do I code the Active Effects and in what programming language?

For the most part, you can't "code" active effects, aside from creating, updating, or deleting an active effect on an actor or item. As with all Foundry module, system, and macro code, such code would be written in JavaScript, or additionally for modules and systems, a language that can compile to JavaScript.
