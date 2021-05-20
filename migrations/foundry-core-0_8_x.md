---
title: Migration Summary for 0.8.x
description: 
published: true
date: 2021-05-19T20:43:09.377Z
tags: 
editor: markdown
dateCreated: 2021-05-01T03:24:28.830Z
---

## Documents

![documents.png](/documents.png)

> Most Likely Affects: Systems, Modules
{.is-info}

> Stub

### What changed between 0.7 and 0.8?

> Stub
> [Entity -> Document](https://foundryvtt.wiki/en/migrations/0_8_0/no-base-entity)
> All APIs standardized


## ActorSheet#getData

> Most Likely Affects: Systems
{.is-info}

Relevant Issue: [Redesign the structure of ActorSheet#getData to provide more sensible references for the actor, its data, and any items or effects that the actor owns.
](https://gitlab.com/foundrynet/foundryvtt/-/issues/4321)

ActorSheet's default `getData` method's return signature changed. If your system did not override this to create your own data for the template, your template might need adjusting.

**It is highly recommended by Foundry to create your own return data object that is returned from this method**

### What changed between 0.7 and 0.8?

In 0.7 `ActorSheet#getData()#actor` was a copy of `Actor#data`. In 0.8 it is a copy of the Actor document itself. This means the path to the value being changed is no longer the same as the path to the value being displayed.

Practically this is what might need to be changed in your handlebars templates:

#### :x: 0.7
```html
<input name="data.foo" value="{{data.foo}}" />
```

#### :heavy_check_mark: 0.8
```html
<input name="data.foo" value="{{data.data.foo}}" />
```

### ✅ 0.8 recommended
```js
getData(options) {
  let baseData = super.getData(options);
  let sheetData = {};
  sheetData.foo = baseData.actor.data.title;
  return sheetData;
}
```
```html
<input name="data.foo" value="{{foo}}" />
```

## Document CRUD Hooks and Methods

> Most Likely Affects: Systems, Modules
{.is-info}

### [New Hooks Documentation](https://foundryvtt.com/api/alpha/hookEvents.html)

As part of the transition to Documents as the primary base for all data structures in Foundry Core, the hooks related to ~~Entities~~ Documents have been standardized.

All Embedded Documents use the same hooks as their top-level counterparts:

- preCreate[documentName]
- create[documentName]
- preUpdate[documentName]
- update[documentName]
- preDelete[documentName]
- delete[documentName]

There are more detailed docs about what each Hook does and what arguments it has available in the official [hookEvents documentation](https://foundryvtt.com/api/alpha/hookEvents.html).

### Note for System Developers

> The exception to this recommendation is if your system does something which actively prevents `Document` creation in some circumstances. It is impossible to prevent `Document` creation from the `_preCreate` method, use the corresponding `preCreate` hook instead.
{.is-warning}

Unless you are attempting to prevent the creation of a `Document`, it is not recommended for Systems to leverage these hooks. Each of these hooks is fired by a corresponding method on the base Document type which you can overwrite for a more safe and consistent way to accomplish your system's logic without interference.


```js
class MySystemActor extends Actor {
  async _preCreate(data, options, user) {
    // do stuff to data, options, etc
  	await super._preCreate(data, options, user);
  }
  
  async _preUpdate(changed, options, user) {
    // do stuff to changed, options, etc
  	await super._preUpdate(changed, options, user);
  }
}
```

#### Changing data during these methods
When making a change to a document in one of these methods it is necessary to use the `update` method to persist the change to the database.
```js
class MySystemActor extends Actor {
 async _preCreate(data, options, user) {
   this.data.update({ someChange }); // note we are using `this` instead of the provided `data`
   // do stuff to data, options, etc
  	await super._preCreate(data, options, user);
 }
}
```


### What changed between 0.7 and 0.8?

All hooks related to embedded entities were removed. These embedded entities are treated as Documents same as their Parents are in 0.8. As a result all of the hooks around EmbeddedDocuments and top level Documents could be standardized.


## Tiles

> Most Likely Affects: Modules
{.is-info}

> Stub

### What changed between 0.7 and 0.8?

> Stub
> [TilesLayer was merged into BackgroundLayer/ForegroundLayer](https://foundryvtt.wiki/en/migrations/0_8_2/tiles-layers)


## Audio

> Most Likely Affects: Modules
{.is-info}

> Stub

### What changed between 0.7 and 0.8?

> Stub
> [Howler Removed](https://foundryvtt.wiki/en/migrations/0_8_2/tiles-layers)
> [New Sound API](https://foundryvtt.com/api/alpha/AudioHelper.html)

