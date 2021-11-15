---
title: Document
description: 
published: true
date: 2021-11-15T16:24:39.782Z
tags: development, api, documentation, docs
editor: markdown
dateCreated: 2021-11-15T16:03:42.636Z
---

# Document

![Up to date as of v8](https://img.shields.io/badge/FoundryVTT-v8-informational)

## Overview

Much of this article is copied from the [official API Docs](https://foundryvtt.com/api/#documents-and-data). The official api documentation has a list of all Documents, their Data schemas, and other classes related to those documents.

> Data in Foundry Virtual Tabletop is organized around the concept of Documents. Each document represents a single coherent piece of data which represents a specific object within the framework. The term Document refers to both the definition of a specific type of data (the Document class) as well as a uniquely identified instance of that data type (an instance of that class).

Everything that is stored in the database is a `Document`. Some Documents are EmbeddedDocuments which live fully inside a parent document (e.g. Items in Actors or Tiles in Scenes).

There is a [System Diagram](https://gitlab.com/foundrynet/foundryvtt/-/issues/4373) which might help explain the architecture.

### Legend

```javascript
Document.updateDocuments // `.` indicates static method or property
Document#update // `#` indicates instance method or property
```

---

## Key Concepts

### Primary and Embedded

There are 2 kinds of `Document`: Primary and Embedded. Embedded Documents are full Documents same as the Primary, and the API for interacting with them is identical. There are some helper methods for interacting with a Primary Document's Embedded Collections.

- Primary Documents can contain Embedded Documents.
- Embedded Documents are not directly indexed in a database table.

#### Primary Documents

These are stored in their own database table within the active World or Compendium.

Notable Examples:

- [Actor](https://foundryvtt.com/api/Actor.html)
- [Item](https://foundryvtt.com/api/Item.html)
- [JournalEntry](https://foundryvtt.com/api/JournalEntry.html)
- [Scene](https://foundryvtt.com/api/Scene.html)
- [User](https://foundryvtt.com/api/User.html)

#### Embedded Documents

Embedded Documents are document types which only exist within a Collection on a parent Primary Document and thus have no dedicated database entry. Some Primary Documents support being embedded in other Primary Documents, but most do not.

Notable Examples:

- All Scene embeds are EmbeddedDocuments within the Scene Primary Document:
  - [AmbientSound](https://foundryvtt.com/api/AmbientSoundDocument.html)
  - [Tile](https://foundryvtt.com/api/TileDocument.html)
  - [Note](https://foundryvtt.com/api/NoteDocument.html)
- Items are embeddable within Actors
- ActiveEffects are embeddable within both Items and Actors

### Document Data

Every Document stores a `Document#data._source` property which conforms to the Document Type's Schema. A given document's schema can be seen on the official API docs for that document, or by examining `Document#schema`.

This `data._source` property should rarely be accessed, instead access a Document's `data` property to get all of its prepared data in addition to its source data _(see [Data Preparation](#Data-Preparation) below for more about prepared data)_.

> This is not to be confused with the `data` property within the Actor and Item document schema. That `data` property's schema is defined by the system's `template.json` and is accessed by `_data.data` or `data.data`.
{.is-warning}

#### Validation

`DocumentData` is governed by a Schema, `DocumentData#validate` is run during initialization and updates which will purge any 'unexpected' data (i.e. arbitrary data not in the Schema) from itself. As a result of these validations, it is impossible for a package to store arbitrary data on a Document without using Flags.

> An exception to this rule is Systems which can define Actor and Item data templates. But even they cannot store arbitrary data not present in the template.
{.is-warning}

#### DocumentName and Type

A Document's 'kind' can be obtained by checking the document's `documentName`. E.g. `"Actor"` for actors, `"Scene"` for scenes, `"Combatant"` for combatants, and so on. It is impossible to create a Document with an arbitrary documentName.

Some documents have a `type` property in their Schema (Notable examples: Folder, Actor, Item; where Actor and Item types are system defined). These are also validated and thus it is impossible to create a Document with an arbitrary `type` property.

### Data Preparation

Documents do not need to store all the data needed to interact with them, often times, this data can be derived on the clientside when the document is initialized.

A common example would be an Actor attribute which can be calculated from the values of several other of the Actor's attributes.

The following synchronous methods are fired by the `Document#prepareData` method in order when a document is initialized:

#### [`Document#prepareBaseData`](https://foundryvtt.com/api/ClientDocumentMixin.html#prepareData)

Any information which can be derived with just the base data of the document is prepared.

This information is then available to embedded documents to use in their `prepareData` process.

#### [`Document#prepareEmbeddedEntities`](https://foundryvtt.com/api/ClientDocumentMixin.html#prepareEmbeddedEntities)

All of the embedded collections have their documents initialized.

> Actors call their [`Actor#applyActiveEffects`](https://foundryvtt.com/api/Actor.html#applyActiveEffects) method at the end of this method.
{.is-info}

#### [`Document#prepareDerivedData`](https://foundryvtt.com/api/ClientDocumentMixin.html#prepareDerivedData)

Final step in the core preparation process. All embedded documents have been initialized and their derived data can be used to affect the primary document's data.

### `id` vs `uuid`

Every document has both an `id` and `uuid`.

The `id` is used as the key in all Collections the Document is a member of, making it simple to get a document instance whose Collection and `id` are known.

The `uuid` is unique across all collections and allows for an easy way (via the `fromUuid` function) to find any arbitrary document whose collection might not be known.

### Document Modification Context

All Document update operations accept a final argument: the [`DocumentModificationContext`](https://foundryvtt.com/api/global.html#DocumentModificationContext) (a.k.a. "context"). This context affects _how_ the document is created/modified on the backend.

The primary way to control whether a document being created is an Embedded Document is by providing a `parent` in the context.

By default (i.e. with no context provided) a new document tries to be created as a Primary Document in the correct `WorldCollection`.

---

## API Interactions

### Create

There are a variety of API paths for creating new document on the backend, each have their use cases.

#### Single Document

At the most basic, [`DocumentClass.create`](https://foundryvtt.com/api/abstract.Document.html#.create) will create a document of that DocumentClass.

Certain required fields must be provided during a creation operation, but most will be filled in with defaults. Failing to provide these will result in an error thrown which details which fields were missing.

In general, the required fields are `name` and `type` if the document is one which has multiple subtypes.

```javascript
const newJournalEntry = JournalEntry.create({
  name: 'Foo'
});
```

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

Reading a document's data is as simple as examining the `data` property of that Document's instance. Primary Documents in the world are found in [`WorldCollection`](https://foundryvtt.com/api/WorldCollection.html)s within the `game` instance.

```javascript
game.actors.getName('Foo').data
```

Embedded Documents are always in [`EmbeddedCollection`](https://foundryvtt.com/api/EmbeddedCollection.html)s at the root of the containing Primary Document's instance. A reference to this collection can also be found in the Primary Document's DocumentData.

```javascript
const actor = game.actors.getName('Foo');

actor.items.getName('Bar');

actor.data.items.getName('Bar'); // also works
```

A Primary Document might also be in a Compendium. Documents in compendiums are not necessarily available immediately in the world and often require asynchronous operations to retrieve first. See the Compendium article for more details.

### Update the Database

> It is possible to delete data necessary for a Document's initialization when updating a document.
{.is-danger}

When updating a document, regardless of whether `Document.update` or `DocumentData.update` is being called, the context for the update object given is the same. These methods both expect the shape or paths provided in the update object to conform to the Document's Data Schema shape.

```javascript
const someDocument = {
  data: {
    schemaKeyOne: 'foo',
    schemaKeyTwo: 'bar'
  },
  nonDbData: 'biz'
}
```

For a document with the structure above, the update object should have the same shape as `document.data`:

```javascript
const updateData = {
  schemaKeyOne: 'foobar'
}

someDocument.update(updateData);

// Resulting someDocument:
// {
//   data: {
//     schemaKeyOne: 'foobar',
//     schemaKeyTwo: 'bar'
//   },
//   nonDbData: 'biz'
// }
```

#### Single Update

To update a single document, use the `Document#update()` method on that document's instance.

```javascript
someDocument.update({ someSchemaKey: newValue });
```

Directly update the document's entry on the database. This persists the change and pushes the new document to all clients.

#### Bulk Update

The [`DocumentClass.updateDocuments()`](https://foundryvtt.com/api/abstract.Document.html#.updateDocuments) method is how one can perform an array of updates in a single server operation.

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
      "data.traits.size": document.data.data.traits.size === "sm" ? "md" : "lg",
    };
  },
  (document) => {
    return document.data.data.traits.size !== "tiny";
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

### Updating Locally

The `DocumentData#update()` method effectively mutates a Document's local of `document.data` directly. The key difference between using `DocumentData.update` and directly manipulating the `document.data` is that `update` has special logic for handling embedded document collections.

```javascript
someDocument.data.update({ someSchemaKey: newValue });
```

Any update made with this method does not persist to the database.

An example usecase for this method is updating with Scene EmbeddedDocuments to temporarily have a local state that is different from the database state (e.g. animating the movement of a token).

### Delete

#### Single Document

[`Document#delete()`](https://foundryvtt.com/api/abstract.Document.html#delete) will delete that document instance's document.

#### Bulk Delete

[`DocumentClass.deleteDocuments()`](https://foundryvtt.com/api/abstract.Document.html#.deleteDocuments) accepts an array of document Ids to be deleted all at once.

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

It is possible to update embedded documents in the same operation used to update a primary document. This is more intuitive after examining the `data._source` of the primary document being updated.

Embedded Documents are ultimately stored as an array of document data. To modify an embedded document at the same time as its parent primary document, provide an array of data updates in the primary doucment update object. It is required to pass in the `_id` of the embedded document in addition to any updates.

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

> Updating an embedded document in this way will bypass that embedded document's [update event cycle](#Update-Event-Cycle). In the example below, only the Actor will go through the event cycle.
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


> Documents created in this way do not undergo the full document [creation event cycle](#Creation-Event-Cycle).
> 
> This has some dangers when used in conjunction with unlinked tokens (which this author does not understand yet).
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
      data: {
        someSchemaKey: value,
      },
    },
  ],
});
```

### Relationships between Primary Documents

Sometimes it is desirable to create a relationship between documents which cannot be embedded within one another. The best way to accomplish this is by recording the ID or UUID on one or both of the documents, then constructing the relationship during `prepareData`.

```javascript
// TODO: Example
```

### Scene Embeds

#### Unlinked Actors/Tokens

Here be dragons which this author knows nothing about... yet.

---

## Troubleshooting

> Stub
> This section is a stub, you can help by contributing to it.

---

## Event Cycles

Each Create, Update, and Delete operation undergoes an event cycle consisting of a sequence of instance methods and Hooks being fired. These allow packages to intercept and react to these operations.

The event cycle triggered is dependant on the endpoint used to create/update/delete the document. The quick way to tell is to see if the 'key word' is in the method name (e.g. methods with "update" in their name will run the Update Event Cycle).

| Method                                 | Event Cycle |
| -------------------------------------- | ----------- |
| `DocumentClass.create`                 | Create      |
| `DocumentClass.createDocuments`        | Create      |
| `someDocument#createEmbeddedDocuments` | Create      |
| `DocumentClass.update`                 | Update      |
| `DocumentClass.updateDocuments`        | Update      |
| `someDocument#update`                  | Update      |
| `someDocument#updateEmbeddedDocuments` | Update      |
| `DocumentClass.delete`                 | Delete      |
| `DocumentClass.deleteDocuments`        | Delete      |
| `someDocument#delete`                  | Delete      |
| `someDocument#deleteEmbeddedDocuments` | Delete      |

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

#### 1. [`Document#_preCreate`](https://foundryvtt.com/api/abstract.Document.html#_preCreate)

Typically systems will declare a method here for Actors and Items when they need to intercept the creation and mutate the request.

```javascript
async _preCreate(data, options, user) {
  await super._preCreate(data, options, user);

  // update the document's DocumentData instead of `data`
  return this.data.update({ name: "Some other name" });
}
```

It is not possible to stop the creation process during `Document#_preCreate`.

##### Example: Adding Items to an Actor during preCreate

It's possible to use `Actor#_preCreate` to add items to the parent's embedded document collection.

```javascript
async _preCreate(data, options, user) {
  await super._preCreate(data, options, user);

  const item = new CONFIG.Item.documentClass({name: 'Foo', type: 'feat'});

  const items = this.items.map(i => i.toObject());

  items.push(item.toObject());

  this.data.update({ items });
}
```

#### 2. [`Hooks.call('preCreate[DocumentName]')`](https://foundryvtt.com/api/alpha/hookEvents.html#.preCreateDocument)

Modules are recommended to hook into the sequentially called `pre` hooks to make changes to the impending update `change`.

```javascript
Hooks.on("preCreateActor", (document, data, options, userId) => {
  //update the document's DocumentData instead of mutating the arguments
  document.data.update({ name: "Some other name" });
});
```

##### Preventing Updates

It is possible to stop the update in the pre Hook by returning `false` explicitly.

```javascript
Hooks.on("preCreateActor", (document, data, options, userId) => {
  if (document.name === "Don't Create") {
    return false;
  }
});
```

#### 3. Server creates Database entry

After the database is updated by the server, a socket broadcast triggers the creation on all connected Clients.

As part of this create, the document is initialized, then the following are invoked:

#### 4. [`Document#_onCreate`](https://foundryvtt.com/api/abstract.Document.html#_onUpdate)

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

#### 5. [`Hooks.callAll('create[DocumentName]')`](https://foundryvtt.com/api/hookEvents.html#.updateDocument)

Similar to `_onCreate`, modules are encouraged to use `create` hooks to react to new Document creation.

### Update Event Cycle

The update cycle is very similar to the creation cycle, first steps run locally:

#### 0. `Document.update`

First determines the minimum diff between provided update arguments and the current document's `data._source`. This prevents more data being sent in the update request than is actually changing.

#### 1. [`Document#_preUpdate`](https://foundryvtt.com/api/abstract.Document.html#_preUpdate)

Typically systems will declare a method here for Actors and Items when they need to intercept the update and mutate the change being requested.

```javascript
async _preUpdate(changed, options, user) {
  // directly mutating the arguments is fine
  changed[someSchemaKey] = "new value";

  return super._preUpdate(changed, options, user);
}
```

It is not possible to stop the update process during `Document#_preUpdate`.

#### 2. [`Hooks.call('preUpdate[DocumentName]')`](https://foundryvtt.com/api/alpha/hookEvents.html#.preUpdateDocument)

Modules are recommended to hook into the sequentially called `pre` hooks to make changes to the impending update `change`.

```javascript
Hooks.on("preUpdateActor", (document, change, options, userId) => {
  change[someSchemaKey] = "New Value";
});
```

##### Preventing Updates

It is possible to stop the update in the pre Hook by returning `false` explicitly.

```javascript
Hooks.on("preUpdateActor", (document, change, options, userId) => {
  if (document.name === "Don't Touch") {
    return false;
  }
});
```

#### 3. Server updates Database entry

After the database is updated by the server, a socket broadcast triggers the update on all connected Clients.

As part of this update, the document is re-initialized, then the following are invoked:

#### 4. [`Document#_onUpdate`](https://foundryvtt.com/api/abstract.Document.html#_onUpdate)

Allows some followup operations to be declared by the system in the case of Actors and Items. Again be cautious of triggering too many "ping pong" backend requests and use the `pre` method/hook when practical.

Also be wary that these methods are called on all connected clients. The updater's `userId` is provided in the arguments.

```javascript
_onUpdate(changed, options, userId) {
  super._onUpdate(changed, options, userId);

  if ( userId !== game.user.id ) return;
  // do other things
}
```

#### 5. [`Hooks.callAll('update[DocumentName]')`](https://foundryvtt.com/api/hookEvents.html#.updateDocument)

Similar to `_onUpdate`, modules are encouraged to use `update` hooks to react to Document updates.

### Delete Event Cycle

The delete event cycle has much the same characteristics of the Update and Create cycles.

Runs Locally:

1. [`Document#_preDelete`](https://foundryvtt.com/api/abstract.Document.html#_preDelete)
2. [`Hooks.call('preDelete[DocumentName]')`](https://foundryvtt.com/api/alpha/hookEvents.html#.preDeleteDocument)
3. Database entry is Deleted

Runs on all connected Clients:

5. [`Document#_onDelete`](https://foundryvtt.com/api/abstract.Document.html#_onDelete)
6. [`Hooks.callAll('delete[DocumentName]')`](https://foundryvtt.com/api/hookEvents.html#.deleteDocument)
