---
title: Document
description: An extension of the base DataModel which defines a Document. Documents are special in that they are persisted to the database and referenced by _id.
published: true
date: 2024-04-21T01:02:53.121Z
tags: development, api, documentation, docs
editor: markdown
dateCreated: 2021-11-15T16:03:42.636Z
---

# Document

![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

If you're dealing with the underlying data of the Foundry API, you almost certainly are going to need to work with Documents.

Official Documentation
- [API frontpage subsection](https://foundryvtt.com/api/#documents-and-data)
- [Document](https://foundryvtt.com/api/classes/foundry.abstract.Document.html)

**Legend**

```javascript
Document.metadata // `.` indicates static method or property
Document#update // `#` indicates instance method or property
```

## Overview

Everything that is stored in the database is a `Document`. Some Documents are Embedded Documents which live fully inside a parent document (e.g. Items in Actors or Tiles in Scenes). Embedding can be multiply nested - a `Scene` could have a `TokenDocument` which has an `Actor` that has an `Item` that has an `ActiveEffect`. All layers of embedding are collectively known as "Descendant Documents".

Documents do not handle rendering; that is the job of [Applications](/en/development/api/application) and the Canvas. Many document lifecycle events impact rendering, but the actual internal functions of those renders to translate the data changes to visual changes are handled by the aforementioned sections of the API.

The official api documentation has a list of all Documents, their Data schemas, and other classes related to those documents. Document schemas are tracked in the "base" class, e.g. the valid data for an [Actor](https://foundryvtt.com/api/classes/client.Actor.html) document is derived from [`BaseActor.defineSchema`](https://foundryvtt.com/api/classes/foundry.documents.BaseActor.html#defineSchema).

---

## Key Concepts

These are the most important thing to know with actors.

### Document Storage

Broadly speaking, there are two general groups of document types: primary documents and embedded documents. The distinction is that the server maintains a database for each type of primary document and provides access to them in the `game` global object (such as `game.actors` and `game.scenes`) while embedded documents exist in a collection embedded inside the data of another document (for example, the `scene.tokens` for a given `scene` instance).

In addition to the databases for primary documents, most primary document types can also be stored in compendiums (barring things like ChatMessage, Combat). Compendiums are additional databases, but their contents are only loaded as-needed, rather than being sent to all users as they connect to the world in the first place.

Beyond world and embedded documents, the [Adventure](https://foundryvtt.com/api/classes/client.Adventure.html) document type is an outlier. Adventure documents only exist inside compendiums. They exist as a way to bundle world documents and bulk import them to a world as-needed; the functionality is primarily used for distributing packaged adventures as modules that can be imported and played.

#### Primary Documents

These are stored in their own database table within the active World or Compendium.

**Compendium Eligible:**

- [Actor](https://foundryvtt.com/api/classes/client.Actor.html)
- [Cards](https://foundryvtt.com/api/classes/client.Cards.html)
- [Folder*](https://foundryvtt.com/api/classes/client.Folder.html)
- [JournalEntry](https://foundryvtt.com/api/classes/client.JournalEntry.html)
- [Item](https://foundryvtt.com/api/classes/client.Item.html)
- [Macro](https://foundryvtt.com/api/classes/client.Macro.html)
- [Scene](https://foundryvtt.com/api/classes/client.Scene.html)
- [Rollable Table](https://foundryvtt.com/api/classes/client.RollTable.html)

\* As of v11, folders CAN go in compendiums and are displayed alongside the normal sidebar but data-wise are in their own, seperate collection within a compendium.

**Cannot go in compendiums:**

- [Chat Message](https://foundryvtt.com/api/classes/client.ChatMessage.html)
- [Combat Encounter](https://foundryvtt.com/api/classes/client.Combat.html)
- [Fog Exploration](https://foundryvtt.com/api/classes/client.FogExploration.html)
- [Playlist](https://foundryvtt.com/api/classes/client.Playlist.html)
- [Setting](https://foundryvtt.com/api/classes/client.ClientSettings.html)
- [User](https://foundryvtt.com/api/classes/client.User.html)

#### Embedded Documents

Embedded Documents are document types which only exist within a Collection on a parent Primary Document and thus have no dedicated database entry. Some Primary Documents support being embedded in other Primary Documents, but most do not.

**Actor-related:**

- [Active Effects](https://foundryvtt.com/api/classes/client.ActiveEffect.html) can be embedded in Items and Actors
- [Combatants](https://foundryvtt.com/api/classes/client.Combatant.html) can be embedded in Combat Encounters
- Items are embeddable within Actors

**Scene-related:**

- [Ambient Light](https://foundryvtt.com/api/classes/client.AmbientLightDocument.html)
- [Ambient Sound](https://foundryvtt.com/api/classes/client.AmbientSoundDocument.html)
- [Drawing](https://foundryvtt.com/api/classes/client.DrawingDocument.html)
- [Measured Template](https://foundryvtt.com/api/classes/client.MeasuredTemplateDocument.html)
- [Note](https://foundryvtt.com/api/NoteDocument.html)
- [Tile](https://foundryvtt.com/api/TileDocument.html)
- [Token*](https://foundryvtt.com/api/classes/client.TokenDocument.html)
- [Wall](https://foundryvtt.com/api/classes/client.Wall.html)

\* Actors and combatatants have *links* to tokens, but the token document itself is embedded in a scene. Unlinked tokens also embed a synthetic actor via [Actor Delta](https://foundryvtt.com/api/classes/client.ActorDelta.html))

**Other:**

- [Journal Entry Pages](https://foundryvtt.com/api/classes/client.JournalEntryPage.html) embed in Journal Entries
- [Playlist Sounds](https://foundryvtt.com/api/classes/client.PlaylistSound.html) embed in Playlists
- [Table Results](https://foundryvtt.com/api/classes/client.TableResult.html) embed in Rollable Tables

### Inheritance

Documents serve as the core of Foundry's data structure, but are not themselves the base class - instead, `class Document extends DataModel`. (Learn more about the [DataModel](/en/development/api/DataModel) class). What you need to know from the DataModel is that the  properties on a document are strictly controlled - you can't just add new ones wherever and hope it will save to the database. The two areas system and module developers *do* have control over are the `flags` field, which is present on all documents, and the `system` field, which is present on only a subset of document types.

Another key piece of inheritance in every Document is `ClientDocumentMixin`, which due to the limitations of the auto-generated JSDoc is not documented on the official API website, and actually breaks most of the inheritance tracking. Despite this, you can still see the function for yourself at `yourFoundryInstallPath\resources\app\client\data\abstract\client-document.js` in your local installation.

Some important properties introduced in ClientDocumentMixin:

**Getters**
- `collection`
- `compendium`
- `isOwner`
- `hasPlayerOwner`
- `limited`
- `link`
- `permission`
- `sheet`
- `uuid`
- `visible`

**Methods**
- `prepareData`
- `render`
- `getRelativeUUID`
- `static defaultName`
- `static createDialog`
- `static DeleteDialog`

#### DocumentName and Type

A Document's 'kind' can be obtained by checking the document's `documentName`. E.g. `"Actor"` for actors, `"Scene"` for scenes, `"Combatant"` for combatants, and so on. It is impossible to create a Document with an arbitrary documentName.

Some documents have a `type` property in their Schema. These are also validated and thus it is impossible to create a Document with an arbitrary `type` property. As a system, you define the available types in your system.json; as a module, you can use [module sub-types](https://foundryvtt.com/article/module-sub-types/) to define new types.

**System-defined types**

- Actor
- Card
- Cards
- Item
- JournalEntryPage

**Foundry-defined types**

- Folder (eligible types are the same as Primary Documents that can go in compendiums)
- Macro (chat or script)
- Table result (text, document, or compendium)

### `id` vs `uuid`

Every document has both an `id` and `uuid`.

The `id` is used as the key in all Collections the Document is a member of, making it simple to get a document instance whose Collection and `id` are known.

The `uuid` is unique across all collections and allows for an easy way (via the `fromUuid` or `fromUuidSync` functions) to find any arbitrary document whose collection might not be known.

You can use the book icon in the header bar of a document sheet to retrieve these values; a left click copies the ID to clipboard, while a right click copies the UUID to clipboard.

## API Interactions

Many document operations take a `data` and `context` argument, which are each always objects.

**Data:** This argument directly translates to the properties of the document. The eligible properties of a document come from the `defineSchema` method of the base document class, e.g. `BaseActor#defineSchema`; you can see the list of base document classes at the top of the [document API page](https://foundryvtt.com/api/classes/foundry.abstract.Document.html)

**Context:** The [`DocumentConstructionContext`](https://foundryvtt.com/api/interfaces/types.DocumentConstructionContext.html) and [`DocumentModificationContext`](https://foundryvtt.com/api/interfaces/types.DocumentModificationContext.html) types affects _how_ the document is created/modified on the backend. The primary way to control whether a document being created is an Embedded Document is by providing a `parent` in the context. By default (i.e. with no context provided) a new document tries to be created as a Primary Document in the correct `WorldCollection`.


### Create

There are a variety of API paths for creating new document on the backend, each have their use cases.

#### Single Document

At the most basic, [`DocumentClass.create`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#create) will create a document of that DocumentClass.

Certain required fields must be provided during a creation operation, but most will be filled in with defaults. Failing to provide these will result in an error thrown which details which fields were missing. 

In general, the required fields are `name` and `type` if the document is one which has multiple subtypes.

```javascript
const newJournalEntry = JournalEntry.create({
  name: 'Foo'
});
```

> **Calling the Constructor**
> It is technically possible to instantiate a Document by calling the constructor with `new Document(data)` instead of using `Document.create()`. This is very rarely the correct way to do things, because it only creates an object in memory, it does not send the document to the database to be persisted or sent to other clients.
{.is-warning}

##### Embedded Document

If the document type being created is one which can be embedded within a primary document, providing the `parent` document in the context will create it as an embedded document instead.

```javascript
const actor = game.actors.getName('Foo');

const newActiveEffect = ActiveEffect.create({
  name: 'New Effect'
}, {
  parent: actor
});
```

#### Bulk Creation

The [`DocumentClass.createDocuments`](https://foundryvtt.com/api/abstract.Document.html#.createDocuments) method is useful for creating many documents in the same operation. The same limitations regarding required fields are present as in `create`.

```javascript
const newJournalEntries = JournalEntry.createDocuments([
  {
    name: 'Foo'
  },
  {
    name: 'Bar'
  }
]);
```

Any modification context options provided will apply to all created documents. E.g. `parent` to create a bunch of embedded documents at once, or `temporary` to create a bunch of documents without creating them in the database.

##### Embedded Documents

In cases where one has the parent document and wishes to create embedded documents on it, `Document#createEmbeddedDocuments` has a similar syntax to `Document.createDocuments`.

```javascript
const actor = game.actors.getName('Foo');

actor.createEmbeddedDocuments("Item", [
  {
    name: 'Foo',
    type: 'weapon',
  },
  {
    name: 'Bar'
    type: 'weapon',
  }
])
```

### Read

Primary Documents in the world are found in [`WorldCollection`](https://foundryvtt.com/api/WorldCollection.html)s within the `game` instance.

```javascript
game.actors.getName('Foo')
```

Embedded Documents are always in [`EmbeddedCollection`](https://foundryvtt.com/api/EmbeddedCollection.html)s at the root of the containing Primary Document's instance. A reference to this collection can also be found in the Primary Document's DocumentData.

```javascript
const actor = game.actors.getName('Foo');

actor.items.getName('Bar');
```

A Primary Document might also be in a Compendium. Documents in compendiums are not necessarily available immediately in the world and often require asynchronous operations to retrieve first. See the Compendium article for more details.


### Update the Database

The `Document#update(data, context={})` method is used for updating a document in the databases that the server maintains, which then gets propagated to all of the clients (including the one that issued the update in the first place).

Updates are done using an object with the changes that need to be written to the document in the database. They can either be properly nested objects of data or you can use the `'parent.child'` notation to define a specific sub-property to update. `actor.update({'system.key': newValue})` and `actor.update({system: {key: newValue}})` will achieve the same end result of updating `actor.system.key` to `newValue`.

#### Single Update

To update a single document, use the `Document#update()` method on that document's instance.

```javascript
someDocument.update({ someSchemaKey: newValue });
```

Directly update the document's entry on the database. This persists the change and pushes the new document to all clients.

**Deleting Properties:** Because Foundry uses differences and merges to only send data being changed to the database, a special notation is needed to signal to the database that a value is being intentionally deleted from the database (because omitting a value is done when a value simply isn't being updated). To delete the `key` from `actor.system.key`, you need to make an update like `actor.update({'actor.system.-=key': null})`; the `-=` before the `key` to be deleted is the key there, that signals the database to delete the key. The value ostensibly being written doesn't matter, but convention is to use `null` for that to further indicate that the key is being deleted.

**Working with Arrays:** Because the updates work using object key merges, whereas arrays purely work via index, Foundry cannot modify specific indexes inside an array via update, arrays need to be written as a cohesive whole. To update an array in a document's data, you need to edit a copy of the array and then write the new array as a whole with an update.

#### Bulk Update

The [`DocumentClass.updateDocuments()`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#updateDocuments) method is how one can perform an array of updates in a single server operation.

```javascript
const updateData = {
  _id: "some-id",
  name: "New Name!",
};

const updates = [updateData];

Actor.updateDocuments(updates);
```

Ultimately this method is called by every other update method and is directly responsible for calling the database update call. It expects an `_id` key in each `updateData` provided within the `updates` array.

#### Bulk update with a Function

To update many documents at once in a programatic fashion, the [`DocumentCollection.updateAll`](https://foundryvtt.com/api/DocumentCollection.html#updateAll) method operates on all documents in the given Collection with a provided transformation.

The transformation can be an object, but more interestingly it can be a function. The function provided will run for each document, allowing construction of the update data differently for different documents.

Additionally, `updateAll` accepts a second parameter which is a filter function to limit which documents have the transformation applied.

```javascript
// update 'sm' to 'md'; all else to 'lg'
game.actors.updateAll(
  (document) => {
    return {
      "system.traits.size": document.system.traits.size === "sm" ? "md" : "lg",
    };
  },
  (document) => {
    return document.system.traits.size !== "tiny";
  }
);
```

#### Embedded Documents

Updating a primary document's embedded documents can be done with [`Document#updateEmbeddedDocuments`](https://foundryvtt.com/api/abstract.Document.html#updateEmbeddedDocuments).

This method must be provided with the embedded collection's name.

```javascript
const actor = game.actors.getName("Foo");

actor.updateEmbeddedDocuments("Item", [
  {
    _id: "some-id",
    someSchemaKey: newValue,
  },
]);
```

### Update vs. Assignment

`Document#update({prop: newValue})` is an asynchronous operation that makes a call to the database and propagates out across all connected clients. `Document#prop = newValue` is an in-memory assignment that runs synchronously and will not be shared with other clients. 

It's important to recognize the distinction between these two methods. The main time assignment is used for writing document data is during the derived data preparation, where you are intentionally calculating derived values that don't need to be stored in the database.

### Delete

Deletion operations use the DocumentModifyContext type.

#### Single Document

[`Document#delete(context)`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#delete) will delete that document instance's document.

#### Bulk Delete

[`Document.deleteDocuments([ids], context)`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#deleteDocuments) accepts an array of document Ids to be deleted all at once.

#### Embedded Documents

Deleting a primary document's embedded documents can be done with [`Document#deleteEmbeddedDocuments`](https://foundryvtt.com/api/abstract.Document.html#deleteEmbeddedDocuments).

This method must be provided with the embedded collection's name.

```javascript
const actor = game.actors.getName("Foo");

actor.deleteEmbeddedDocuments("Item", ["some-id"]);
```

## Specific Use Cases

### Temporary Document

It's possible to create an ephemeral document on the client side which does not get persisted to the database. This can be useful for complex creation operations where it is helpful to have the rest of a Document's API available before actually committing to creating a document on the backend.

```javascript
const tempDocument = SomeDocumentClass.create(documentData, { temporary: true });
```

### Updating Embedded Documents as part of the parent Update

It is possible to update embedded documents in the same operation used to update a primary document. This is more intuitive after examining the `_source` of the primary document being updated.

Embedded Documents are ultimately stored as an array of document data. To modify an embedded document at the same time as its parent primary document, provide an array of data updates in the primary document update object. It is required to pass in the `_id` of the embedded document in addition to any updates so Foundry knows which item to update (this is a special case of array updates where you don't need to pass the full array).

```javascript
someDocument.update({
  schemaKey: "New Value",
  [embeddedDocumentCollection]: [
    {
      _id: "some-existing-id",
      embeddedDocumentSchemaKey: "New Value",
    },
  ],
});
```

> Updating an embedded document in this way will bypass that embedded document's [update event cycle](#update-event-cycle). In the example below, only the Actor will go through the event cycle.
{.is-warning}

#### Example: Updating an Item on an Actor

The following snippet will change the image for the parent actor and one of the existing items on that actor in the same `update` call.

```javascript
const actorFoo = game.actors.getName("Foo");

actorFoo.update({
  img: "some/new/image/path",
  items: [
    {
      _id: "existingItemId",
      img: "some/other/new/image/path",
    },
  ],
});
```

### Creating Embedded Documents as part of the parent Update


> Documents created in this way do not undergo the full document [creation event cycle](#creation-event-cycle).
{.is-warning}

Similar to Updating embedded documents as part of the parent update, it is possible to create new documents by providing all required fields but no `_id`.

For example, `Item`'s required fields are `name` and `type`. This snippet will create a new item while also updating the parent's `img`.

```javascript
const actorFoo = game.actors.getName("Foo");

actorFoo.update({
  img: "some/new/image/path",
  items: [
    {
      name: "New Item!",
      type: "weapon",
      img: "some/other/new/image/path",
      system: {
        someSchemaKey: value,
      },
    },
  ],
});
```

### Relationships between Primary Documents

Sometimes it is desirable to create a relationship between documents which cannot be embedded within one another (e.g. Items within other Items). The best way to accomplish this is by recording the ID or UUID on one or both of the documents. Then, when you need to 

#### Unlinked Actors/Tokens

Tokens are a form of canvas document that are embedded within scenes. When a token is linked to an actor, `Token#actor` simply returns that actor. When a token is NOT linked to an actor, the token stores an `ActorDelta` document that records the differences between the unlinked actor and the source actor (e.g. name, HP), then constructs a synthetic actor which it returns in that `actor` getter. 

## Event Cycles

Each Create, Update, and Delete operation undergoes an event cycle consisting of a sequence of instance methods and Hooks being fired. These allow packages to intercept and react to these operations.

The event cycle triggered is dependant on the endpoint used to create/update/delete the document. The quick way to tell is to see if the 'key word' is in the method name (e.g. methods with "update" in their name will run the Update Event Cycle).

| Method                             | Event Cycle |
| ---------------------------------- | ----------- |
| `Document.create`                  | Create      |
| `Document.createDocuments`         | Create      |
| `Document#createEmbeddedDocuments` | Create      |
| `Document.update`                  | Update      |
| `Document.updateDocuments`         | Update      |
| `Document#update`                  | Update      |
| `Document#updateEmbeddedDocuments` | Update      |
| `Document.delete`                  | Delete      |
| `Document.deleteDocuments`         | Delete      |
| `Document#delete`                  | Delete      |
| `Document#deleteEmbeddedDocuments` | Delete      |

Embedded Documents created or updated as part of a Primary Document's `update` operation do not undergo their own Update Event Cycle.

### Event Cycle Commonalities

These cycles all follow roughly the same format (`...` is the name of the operation being done):

1. `Document#_pre...`
2. `Hooks.call('pre...[DocumentName]')`
3. `Document#_on...`
4. `Hooks.callAll('...[DocumentName]')`

### Creation Event Cycle

The first steps of a Document Create runs locally:

#### 0. New Document is Instantiated Locally

The client requesting the creation instanciates a new Document with the provided data, allowing all of the normal DocumentData apis to be available in the next steps.

#### 1. [`Document#_preCreate`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#_preCreate)

Typically systems will declare a method here for Actors and Items when they need to intercept the creation and mutate the request.

```javascript
async _preCreate(data, options, user) {
  await super._preCreate(data, options, user);

  // update the document's source 
  // this is a synchronous operation 
  // because it's executed locally BEFORE the item data is sent to the database
  return this.updateSource({ name: "Some other name" });
}
```

##### Example: Adding Items to an Actor during preCreate

It's possible to use `Actor#_preCreate` to add items to the parent's embedded document collection.

```javascript
async _preCreate(data, options, user) {
  await super._preCreate(data, options, user);

  const item = new CONFIG.Item.documentClass({name: 'Foo', type: 'feat'});

  const items = this.items.map(i => i.toObject());

  items.push(item.toObject());

  this.updateSource({ items });
}
```

#### 2. [`Hooks.call('preCreate[DocumentName]')`](https://foundryvtt.com/api/modules/hookEvents.html#preCreateDocument)

Modules are recommended to hook into the sequentially called `pre` hooks to make changes to the impending update `change`.

```javascript
Hooks.on("preCreateActor", (document, data, options, userId) => {
  //update the document's DocumentData instead of mutating the arguments
  document.updateSource({ name: "Some other name" });
});
```

##### Preventing Creation

It is possible to stop the creation of a document by returning `false` explicitly.

```javascript
// System method
class MyActor extends Actor {
	/** @override */
  async _preCreate(data, options, user) {
  	if (data.name === "Don't Create") {
      return false;
    }
  }
}

// Module method
Hooks.on("preCreateActor", (document, data, options, userId) => {
  if (document.name === "Don't Create") {
    return false;
  }
});
```

#### 3. Server creates Database entry

After the database is updated by the server, a socket broadcast triggers the creation on all connected Clients.

As part of this create, the document is initialized, then the following are invoked:

#### 4. [`Document#_onCreate`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#_onCreate)

Allows some followup operations to be declared by the system in the case of Actors and Items.

The use cases here are as varied and endless as the imagination, but be mindful of causing too many "ping pong" server requests. When practical, do work up-front in the `pre` hooks to avoid excess database operations.

Be wary that these methods are called on all connected clients. The updater's `userId` is provided in the arguments.

```javascript
_onCreate(data, options, userId) {
  super._onCreate(data, options, userId);

  if ( userId !== game.user.id ) return;
  // do other things
}
```

#### 5. [`Hooks.callAll('create[DocumentName]')`](https://foundryvtt.com/api/modules/hookEvents.html#createDocument)

Similar to `_onCreate`, modules are encouraged to use `create` hooks to react to new Document creation.

### Update Event Cycle

The update cycle is very similar to the creation cycle, first steps run locally:

#### 0. `Document.update`

First determines the minimum diff between provided update arguments and the current document's `data._source`. This prevents more data being sent in the update request than is actually changing.

#### 1. [`Document#_preUpdate`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#_preUpdate)

Typically systems will declare a method here for Actors and Items when they need to intercept the update and mutate the change being requested.

```javascript
async _preUpdate(changed, options, user) {
  // directly mutating the arguments is fine
  changed[someSchemaKey] = "new value";

  return super._preUpdate(changed, options, user);
}
```

#### 2. [`Hooks.call('preUpdate[DocumentName]')`](https://foundryvtt.com/api/modules/hookEvents.html#preUpdateDocument)

Modules are recommended to hook into the sequentially called `pre` hooks to make changes to the impending update `change`.

```javascript
Hooks.on("preUpdateActor", (document, change, options, userId) => {
  change[someSchemaKey] = "New Value";
});
```

##### Preventing Updates

It is possible to stop the update by returning `false` explicitly.

```javascript
// System method
class MyActor extends Actor {
	/** @override */
  async _preCreate(data, options, user) {
  	if (data.name === "Don't Touch") {
      return false;
    }
  }
}

// Module method
Hooks.on("preUpdateActor", (document, change, options, userId) => {
  if (document.name === "Don't Touch") {
    return false;
  }
});
```

#### 3. Server updates Database entry

After the database is updated by the server, a socket broadcast triggers the update on all connected Clients.

As part of this update, the document is re-initialized, then the following are invoked:

#### 4. [`Document#_onUpdate`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#_onUpdate)

Allows some follow-up operations to be declared by the system in the case of Actors and Items. Again be cautious of triggering too many "ping pong" backend requests and use the `pre` method/hook when practical.

Also be wary that these methods are called on all connected clients. The updater's `userId` is provided in the arguments.

```javascript
_onUpdate(changed, options, userId) {
  super._onUpdate(changed, options, userId);

  if ( userId !== game.user.id ) return;
  // do other things
}
```

#### 5. [`Hooks.callAll('update[DocumentName]')`](https://foundryvtt.com/api/modules/hookEvents.html#updateDocument)

Similar to `_onUpdate`, modules are encouraged to use `update` hooks to react to Document updates.

### Delete Event Cycle

The delete event cycle has much the same characteristics of the Update and Create cycles.

Runs Locally:

1. [`Document#_preDelete`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#_preDelete)
2. [`Hooks.call('preDelete[DocumentName]')`](https://foundryvtt.com/api/modules/hookEvents.html#preDeleteDocument)
3. Database entry is Deleted

Runs on all connected Clients:

4. [`Document#_onDelete`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#_onDelete)
5. [`Hooks.callAll('delete[DocumentName]')`](https://foundryvtt.com/api/modules/hookEvents.html#deleteDocument)

### Descendant Document Updates

When a descendant document is updated (or created or deleted), the chain parent documents do *not* receive the `#_onUpdate` method nor is the hook called. Instead, they call `_preUpdateDescendantDocuments` and `_onUpdateDescendantDocuments`.

```js
// Event can be Create, Update, or Delete
ClientDocument#_pre${Event}DescendantDocuments(parent, collection, data, options, userId)
ClientDocument_on${Event}DescendantDocuments
```

These events fire on all connected clients, but only if the modified document is in an embedded collection, and loop upwards through each parent document, starting with the first parent. E.g. if you delete an ActiveEffect on an Item on an Actor, `Item#_preDeleteDescendantDocuments` will fire then `Actor#_preDeleteDescendantDocuments`. 

The first, `_pre${Event}DescendantDocuments` is called before `_on${Event}` for the document experiencing the CRUD operation. The second, `_on${Event}`, is called at the very end after the hook.

The full signatures are provided below, on account of the official API not representing the ClientDocumentMixin well - keep in mind that some of these are directly modified by the various document classes, so you should call `super` to maintain useful upstream behaviors.

```js
// note: pseudo-class, the actual signature is 
// function ClientDocumentMixin(Base) { return class ClientDocument extends Base {} }
class ClientDocument {
    /**
     * Actions taken after descendant documents have been created, but before changes are applied to the client data.
     * @param {Document} parent         The direct parent of the created Documents, may be this Document or a child
     * @param {string} collection       The collection within which documents are being created
     * @param {object[]} data           The source data for new documents that are being created
     * @param {object} options          Options which modified the creation operation
     * @param {string} userId           The ID of the User who triggered the operation
     * @protected
     */
    _preCreateDescendantDocuments(parent, collection, data, options, userId)
  
  
    /**
     * Actions taken after descendant documents have been created and changes have been applied to client data.
     * @param {Document} parent         The direct parent of the created Documents, may be this Document or a child
     * @param {string} collection       The collection within which documents were created
     * @param {Document[]} documents    The array of created Documents
     * @param {object[]} data           The source data for new documents that were created
     * @param {object} options          Options which modified the creation operation
     * @param {string} userId           The ID of the User who triggered the operation
     * @protected
     */
    _onCreateDescendantDocuments(parent, collection, documents, data, options, userId)
  
  
    /**
     * Actions taken after descendant documents have been updated, but before changes are applied to the client data.
     * @param {Document} parent         The direct parent of the updated Documents, may be this Document or a child
     * @param {string} collection       The collection within which documents are being updated
     * @param {object[]} changes        The array of differential Document updates to be applied
     * @param {object} options          Options which modified the update operation
     * @param {string} userId           The ID of the User who triggered the operation
     * @protected
     */
    _preUpdateDescendantDocuments(parent, collection, changes, options, userId)

    /* -------------------------------------------- */

    /**
     * Actions taken after descendant documents have been updated and changes have been applied to client data.
     * @param {Document} parent         The direct parent of the updated Documents, may be this Document or a child
     * @param {string} collection       The collection within which documents were updated
     * @param {Document[]} documents    The array of updated Documents
     * @param {object[]} changes        The array of differential Document updates which were applied
     * @param {object} options          Options which modified the update operation
     * @param {string} userId           The ID of the User who triggered the operation
     * @protected
     */
    _onUpdateDescendantDocuments(parent, collection, documents, changes, options, userId)

    /* -------------------------------------------- */

    /**
     * Actions taken after descendant documents have been deleted, but before deletions are applied to the client data.
     * @param {Document} parent         The direct parent of the deleted Documents, may be this Document or a child
     * @param {string} collection       The collection within which documents were deleted
     * @param {string[]} ids            The array of document IDs which were deleted
     * @param {object} options          Options which modified the deletion operation
     * @param {string} userId           The ID of the User who triggered the operation
     * @protected
     */
    _preDeleteDescendantDocuments(parent, collection, ids, options, userId)

    /* -------------------------------------------- */

    /**
     * Actions taken after descendant documents have been deleted and those deletions have been applied to client data.
     * @param {Document} parent         The direct parent of the deleted Documents, may be this Document or a child
     * @param {string} collection       The collection within which documents were deleted
     * @param {Document[]} documents    The array of Documents which were deleted
     * @param {string[]} ids            The array of document IDs which were deleted
     * @param {object} options          Options which modified the deletion operation
     * @param {string} userId           The ID of the User who triggered the operation
     * @protected
     */
    _onDeleteDescendantDocuments(parent, collection, documents, ids, options, userId)

}
```
## Troubleshooting


> Stub
> This section is a stub, you can help by contributing to it.
