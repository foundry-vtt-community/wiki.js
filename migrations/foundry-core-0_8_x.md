---
title: Migration Summary for 0.8.x
description: 
published: true
date: 2021-05-20T20:23:47.519Z
tags: 
editor: markdown
dateCreated: 2021-05-01T03:24:28.830Z
---

## Documents

![documents.png](/documents.png)

> Most Likely Affects: Systems, Modules
{.is-info}


Every instance of `Document` contains an instance `DocumentData` which represents the data actually persisted to the backend.

### Document
`Document` has standardized CRUD (Create, Read, Update, Delete) methods:
- `create`, `update`, and `delete` affect the data persisted to the backend.
- `data` is an instance of `DocumentData`


Some subclasses of `Document` (e.g. `Actor`) then implement methods like `prepareData` which mutate the Document instance's `data` without persisting those mutations to the backend.

### DocumentData

All Documents have a `DocumentData` instance which is designed such that it maintains a separation of the underlying source state and a derived/manipulated local copy. `DocumentData` has standardized ~~C~~RU~~D~~ methods:

- `update` does not not affect the data persisted to the backend. It is instead useful for cases where something on the front-end is temporarily out of sync with the database (e.g. animating a Token's position).
- A `Document`'s instance of `DocumentData` is mutable without affecting the corresponding database entry.


#### Validations

`DocumentData` is governed by a Schema, `DocumentData#validate` is run during initialization and updates which will purge any 'unexpected' data (i.e. arbitrary data not in the Schema) from itself.

As a result of the shift to Document architecture, it is no longer advisable for modules to pass arbitrary data alongside the document's defined schema during creation/update. This data should be added to a document's `flags` or passed through the `options` provided during the document creation or else it risks being purged by the `DocumentData`'s schema validation.

If you are a system this is unlikely to affect you, as your `template.json` informs the schema for your Documents. Modules however will have to be extra strict about ensuring their data is confined to `flags`.


### What changed between 0.7 and 0.8?

> Stub
> [Entity -> Document](https://foundryvtt.wiki/en/migrations/0_8_0/no-base-entity)
> All APIs standardized, Validation will purge non-schema data

In 0.7 many of the core data structures were based on `Entity`. There were some that were not though, and those that were had inconsistent APIs for CRUD (create, read, update, delete) operations.

In 0.8 **all** of the non-PIXI data structures in Foundry are `Document`s. The `Document` class has a base-level API for CRUD operations which keeps these consistent across all of its implementations.

This includes Embedded ~~Entities~~ Documents, which had a large API difference from their top-level counterparts in 0.7. In 0.8 embedded `Document`s are the same class as top-level `Document`s. The APIs to interact with them are identical, though parent Documents have some extra API for reacting to change in their embedded documents.

### Embedded ~~Entities~~ Documents

> Stub.
> Something about how embeddedEntities are interacted with has changed...

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

### Changing data during these methods and hooks
When making a change to a document in one of these methods/hooks it is necessary to use the `update` method on the document's `DocumentData` instance to persist the change to the database.

#### Hook example
```js
Hooks.on('preCreateActor', (document, data, options, userId) => {
  document.data.update({ someChange });
});
```

#### Method Example
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

