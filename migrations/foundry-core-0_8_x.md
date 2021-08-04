---
title: Migration Summary for 0.8.x
description: 
published: true
date: 2021-08-04T02:34:57.953Z
tags: 
editor: markdown
dateCreated: 2021-05-01T03:24:28.830Z
---

## Documents

![documents.png](/documents.png)

> Most Likely Affects: Systems, Modules
{.is-info}


Every instance of `Document` contains an instance `DocumentData` which represents the data actually persisted to the backend. [System Diagram](https://gitlab.com/foundrynet/foundryvtt/-/issues/4373) which might help explain the architecture.

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

When creating an embeddedDocument, `createEmbeddedDocuments` expects the data of the Document instead of a Document instance.

> When passing existing `Document` data to create and update functions, it is recommended to use the new `Document#toObject()` method instead of the `DocumentData` instance, in order to avoid inconsistent behavior. Read more about `Document#toObject()` in this [GitLab issue](https://gitlab.com/foundrynet/foundryvtt/-/issues/4862).
{.is-warning}

```js
const item = await somePack.getDocument('foo');

await actor.createEmbeddedDocuments("Item", [item.toObject()]);
```


---

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

### Changing data during `pre<Action>` methods and hooks
The purpose of `pre<Action>` events like the `Document#_preCreate` method or the `preCreate<Document>` hook is to modify the outgoing data that will be dispatched to the server as part of the operation.

### For Creation Operations
For creation operations, the data that will be sent to the server is the source data for the pending `Document` which exists locally, but has not yet been saved. As part of the creation workflow (and after `preCreate` functions are called) the document data is sent via `Document#toObject`. This means, in order to modify a pending document prior to it's creation, you need to modify the source data of the Document itself using `Document#data#update()`.

Example Usage, `Document#_preCreate`
```js
async _preCreate(createData, options, user) {
  this.data.update({name: "Some other name"});
}
```

Example Usage, `preCreate` Hook
```js
Hooks.on("preCreateActor", (document, createData, options, userId) => {
  document.data.update({name: "Some other name"});
});
```

#### Adding Items to an Actor during preCreate

Use `Actor#_preCreate` to add the items to the parent's embedded document collection.

```js
async _preCreate(data, options, user) {
  await super._preCreate(data, options, user);
  const item = new CONFIG.Item.documentClass({name: 'Foo', type: 'feat'});
  const items = this.items.map(i => i.toObject());
  items.push(item.toObject());
  this.data.update({ items });
}
```

### For Update Operations

For update operations, the data that will be sent to the server is the differential change data that will alter the existing source data for the `Document`. The update workflow sends this changed data directly to the server, so in order to modify the contents of an update operation you need to modify this change data object directly.

Example Usage, `Document#_preUpdate`
```js
async _preUpdate(updateData, options, user) {
  updateData.name = "Some new name";
}
```

Example Usage, `preUpdate` Hook	
```js
Hooks.on("preUpdateActor", (document, updateData, options, userId) => {
  updateData.name = "Some new name";
});
```


### What changed between 0.7 and 0.8?

All hooks related to embedded entities were removed. These embedded entities are treated as Documents same as their Parents are in 0.8. As a result all of the hooks around EmbeddedDocuments and top level Documents could be standardized.

---

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

#### ✅ 0.8 recommended
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

---

## Tiles & Canvas Layers

> Most Likely Affects: Modules
{.is-info}

Foundry 0.8 introduces Overhead Tiles and images to the canvas layer. There is an [official document](https://foundryvtt.com/article/canvas-layers/) with some details about this.

### What changed between 0.7 and 0.8?

> ##### Relevant Issue
> [Refactor the TilesLayer to combine it into the BackgroundLayer which contains Tile objects for a single vertical cross-section of the Scene alongside a background image which fills the Scene canvas.
](https://gitlab.com/foundrynet/foundryvtt/-/issues/4856)

<div style="display: flex">
<div style="width: 50%; margin-right: 0.5em;">
  
In 0.7 the canvas layers looked like this:
```
// order top to bottom
   ControlsLayer
   EffectsLayer
🠗  SoundsLayer
   SightLayer
   LightingLayer
   TokenLayer
   NotesLayer
🠗  WallsLayer
🠕  TemplateLayer
   GridLayer
   DrawingsLayer
-  TilesLayer
   BackgroundLayer
```
</div>
<div style="width: 50%; margin-left: 0.5em;">

In 0.8 they were changed to look like this:
```
// order top to bottom
   ControlsLayer
   EffectsLayer
   SightLayer
   LightingLayer
   SoundsLayer
+  ForegroundLayer
   TokenLayer
   NotesLayer
   TemplateLayer
   WallsLayer
   GridLayer
   DrawingsLayer
+- BackgroundLayer
```
</div>
</div>

The `TilesLayer` was merged with the `BackgroundLayer` in a new type of Canvas Layer: [`MapLayer`](https://foundryvtt.com/api/alpha/MapLayer.html), which contains 1 background image and an arbitrary number of tiles. There are two `MapLayer`s in 0.8 canvases: `BackgroundLayer` and `ForegroundLayer`.

---

## Canvas Placeables

### PlaceableObject document change

> ##### Relevant Issue
> [Split the responsibility of the current PlaceableObject class into core data management, permission control, and configuration which extends EmbeddedEntity vs the Canvas object representation and UX which extends PlaceableObject](https://gitlab.com/foundrynet/foundryvtt/-/issues/4468)

Canvas Placeables are all Documents embedded in a Scene Document. They leverage the standardized Document API to control the data management of the placeable.

#### Token

> Stub. Code sample desired.

The biggest change is to Tokens, which now have a TokenDocument. The recommended way to create a token from an Actor is no longer `Token.fromActor` but instead passing the result from `Actor#getTokenData` to `TokenDocument`'s constructor.

### PerceptionManager

> ##### Relevant Issue
> [Introduce the PerceptionManager class which centralizes scheduling and execution of refresh workflows for lighting, sight, sound, and overhead tiles.
](https://gitlab.com/foundrynet/foundryvtt/-/issues/4035)

The recommended way to refresh Sight, Sound, and Lighting in 0.8 is now through the [`PerceptionManager`](https://foundryvtt.com/api/alpha/PerceptionManager.html). This centralizes scheduling and is thus a more performant way to manually update perception related things.

#### :x: 0.7
```js
canvas.sight.refresh();
```

#### ✅ 0.8 recommended
```js
canvas.perception.refresh();
```

---

## Audio

> Most Likely Affects: Modules
{.is-info}

> Stub

### What changed between 0.7 and 0.8?

> Stub
> [Howler Removed](https://foundryvtt.wiki/en/migrations/0_8_2/tiles-layers)
> [New Sound API](https://foundryvtt.com/api/alpha/AudioHelper.html)

---

## Settings

> Most likely affects: Modules
{.is-info}

### New Setting Types

Two often-asked-for setting types have been [added in 0.8](https://gitlab.com/foundrynet/foundryvtt/-/issues/2888):
- DirectoryPicker
- FilePicker

#### Example Registrations
```js
game.settings.register("core", "chooseFile", {
  name: "Choose A File",
  hint: "You can use a file-picker to choose a certain file",
  scope: "client",
  config: true,
  type: String,
  filePicker: true // This is the important part
});

game.settings.register("core", "chooseFolder", {
  name: "Choose Folder",
  hint: "You can also choose a certain folder",
  scope: "client",
  config: true,
  type: String,
  filePicker: true // This is the important part
});
```


### Reloading onChange

A change in the way 0.8 processes `onChange` callbacks in settings as they are saved has made it unadvisable to use `window.location.reload()` directly. Doing so will trigger the refresh and sometimes cause a race condition that results in some settings not being saved. It is instead recommended to create a debounced reload callback and use that in each of your settings.

#### :x: 0.7
```js
game.settings.register("myModule", "mySetting", {
  ...,
  onChange: window.location.reload
}
                       
game.settings.register("myModule", "myOtherSetting", {
  ...,
  onChange: window.location.reload
}
```

#### ✅ 0.8 recommended
```js
const debouncedReload = foundry.utils.debounce(() => {
  window.location.reload();
}, 100);

game.settings.register("myModule", "mySetting", {
  ...,
  onChange: debouncedReload
}
                       
game.settings.register("myModule", "myOtherSetting", {
  ...,
  onChange: debouncedReload
}
```

## Icons
There were some Icon file path changes between 0.7 and 0.8. This list is formatted like `old/file/path -> new/file/path`.

<details>
<summary>List of Icon File diffs:</summary>

```
public/icons/commodities/flowers/lily-water-white.webp -> public/icons/commodities/flowers/lily-water-white.webppublic/icons/commodities/gems/gem-faceted-diamond-silver-.webp
public/icons/commodities/stone/geode-raw-brown.webp -> public/icons/commodities/stone/geode-raw-brown.webppublic/icons/commodities/stone/stone-cratered-brown.webp
public/icons/containers/kitchenware/vase-013.webp -> public/icons/containers/kitchenware/jug-bottle-clay-brown-gold-blue.webp
public/icons/containers/kitchenware/vase-015.webp -> public/icons/containers/kitchenware/jug-clay-brown-sealed.webp
public/icons/containers/kitchenware/vase-003.webp -> public/icons/containers/kitchenware/jug-clay-brown.webp
public/icons/containers/kitchenware/vase-001.webp -> public/icons/containers/kitchenware/jug-terracotta-orange.webp
public/icons/containers/kitchenware/vase-004.webp -> public/icons/containers/kitchenware/jug-wrapped-red.webp
public/icons/containers/kitchenware/vase-009.webp -> public/icons/containers/kitchenware/vase-bottle-brown.webp
public/icons/containers/kitchenware/vase-014.webp -> public/icons/containers/kitchenware/vase-clay-brown-large.webp
public/icons/containers/kitchenware/vase-002.webp -> public/icons/containers/kitchenware/vase-clay-brown.webp
public/icons/containers/kitchenware/vase-012.webp -> public/icons/containers/kitchenware/vase-clay-cracked-grey.webp
public/icons/containers/kitchenware/vase-011.webp -> public/icons/containers/kitchenware/vase-clay-cracked-white.webp
public/icons/containers/kitchenware/vase-007.webp -> public/icons/containers/kitchenware/vase-clay-etched-brown.webp
public/icons/containers/kitchenware/vase-005.webp -> public/icons/containers/kitchenware/vase-clay-painted-blue-gold.webp
public/icons/containers/kitchenware/vase-008.webp -> public/icons/containers/kitchenware/vase-clay-painted-brown-white.webp
public/icons/containers/kitchenware/vase-006.webp -> public/icons/containers/kitchenware/vase-terracotta-jeweled-orange.webp
public/icons/containers/kitchenware/vase-010.webp -> public/icons/containers/kitchenware/vase-wood-wrapped-brown.webp
public/icons/environment/wilderness/mine-entrance-02.webp -> public/icons/environment/wilderness/mine-exterior-entrance.webp
public/icons/environment/wilderness/mine-entrance-01.webp -> public/icons/environment/wilderness/mine-interior-dungeon-door.webp
public/icons/equipment/chest/breastplate-metal-pieced-grey-02.webp -> public/icons/equipment/chest/breastplate-gorget-steel-purple.webp
public/icons/equipment/chest/breastplate-metal-white-01.webp -> public/icons/equipment/chest/breastplate-gorget-steel-white.webp
public/icons/equipment/chest/breastplate-metal-white-02.webp -> public/icons/equipment/chest/breastplate-gorget-steel.webp
public/icons/equipment/chest/breastplate-pieced-black-01.webp -> public/icons/equipment/chest/breastplate-layered-gilded-black.webp
public/icons/equipment/chest/breastplate-pieced-black-02.webp -> public/icons/equipment/chest/breastplate-layered-steel-black.webp
public/icons/equipment/head/goggles-03.webp -> public/icons/equipment/head/goggles-leather-tan.webp
public/icons/equipment/head/helm-norman-02.webp -> public/icons/equipment/head/helm-barbute-steel-grey.webp
public/icons/equipment/head/helm-norman-03.webp -> public/icons/equipment/head/helm-rounded-reinforced-leather.webp
public/icons/equipment/head/hood-green-02.webp -> public/icons/equipment/head/hood-cloth-green-white.webp
public/icons/equipment/head/hood-pink-gilded-02.webp -> public/icons/equipment/head/hood-cloth-trimmed-pink-gold.webp
public/icons/equipment/head/hood-purple-mask-02.webp -> public/icons/equipment/head/hood-cowl-mask-purple.webp
public/icons/equipment/leg/pants-brown-leather-pants-fur-01.webp -> public/icons/equipment/leg/pants-leather-furred-brown-white.webp
public/icons/equipment/leg/pants-brown-leather-pants-fur-02.webp -> public/icons/equipment/leg/pants-leather-furred-tan-white.webp
public/icons/equipment/waist/sash-purple-cloth-03.webp -> public/icons/equipment/waist/cloth-sash-purple.webp
public/icons/equipment/wrist/bracer-purple-gilded-02.webp -> public/icons/equipment/wrist/bracer-burnished-steel-purple.webp
public/icons/equipment/wrist/bracer-shadow-02.webp -> public/icons/equipment/wrist/bracer-leather-black--steel.webp
public/icons/equipment/wrist/bracer-purple-gilded-03.webp -> public/icons/equipment/wrist/bracer-leather-purple-steel.webp
public/icons/sundries/flags/banner-flag-pirate-02.webp -> public/icons/sundries/flags/banner-flag-pirate-blue.webp
public/icons/sundries/misc/pipe-wooden-02.webp -> public/icons/sundries/misc/pipe-wooden-curved-oak.webp
public/icons/sundries/misc/pipe-wooden-01.webp -> public/icons/sundries/misc/pipe-wooden-straight-brown.webp
public/icons/weapons/staves/staff-orb-red-02.webp -> public/icons/weapons/staves/staff-ornate-orb-steel-red.webp
```
</details>