---
title: Redesign the structure of ActorSheet#getData to provide more sensible references for the actor, its data, and any items or effects that the actor owns.
description: 
published: true
date: 2021-02-07T17:03:45.044Z
tags: 
editor: undefined
dateCreated: 2021-02-07T17:03:41.722Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4321

## What changed

ActorSheet#getData is being redesigned to look like so:

```js
  /** @override */
  getData(options) {
    const isOwner = this.document.isOwner;
    const isEditable = this.isEditable;
    const data = foundry.utils.deepClone(this.object);

    // Copy and sort Items
    const items = this.object.items.map(i => foundry.utils.deepClone(i.data));
    items.sort((a, b) => (a.sort || 0) - (b.sort || 0));
    data.items = items;

    // Copy Active Effects
    const effects = this.object.effects.map(e => foundry.utils.deepClone(e.data));
    data.effects = effects;

    // Return template data
    return {
      actor: this.object,
      cssClass: isEditable ? "editable" : "locked",
      data: data,
      effects: effects,
      items: items,
      limited: this.object.limited,
      options: this.options,
      owner: isOwner,
      title: this.title
    };
  }

```

## What you need to change

- [Unverified] verify your system or sheet module doesn't use the old bad references

### Research Notes

- 