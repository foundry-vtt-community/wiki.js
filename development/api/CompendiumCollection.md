---
title: Compendium Collection
description: A collection of Document objects contained within a specific compendium pack.
published: false
date: 2024-02-22T20:13:24.780Z
tags: documentation
editor: markdown
dateCreated: 2024-02-22T09:00:31.352Z
---

Systems and modules that wish to provide prebuilt documents to their users must use compendiums to store their data.

Official documentation
- [Knowledge Base](https://foundryvtt.com/article/compendium/)
- [API Docs](https://foundryvtt.com/api/classes/client.CompendiumCollection.html)

**Legend**
```js
CompendiumCollection.createCompendium // `.` indicates static method or property
CompendiumCollection#getDocument // `#` indicates instance method or property
```

## Overview

Compendium Collections are a form of document collection that is not kept live in the world; rather, they provide an index from which individual documents can be fetched from the server. This is a core feature that improves the performance of Foundry.

In terms of inheritance, `class CompendiumCollection extends DirectoryCollectionMixin(DocumentCollection)`, and `DocumentCollection extends foundry.utils.Collection extends Map`. CompendiumCollection overrides and extends many of the methods it inherits, so there's less need to trace up the extensions than normal, but it's always good practice to check the full chain of inheritance.

## Key Concepts

Keep the following things in mind when working with compendiums.

### Server-side storage

Documents in compendiums are stored on the server and only available after an asynchronous call to the server to retrieve them. This is very different from documents in world collections, even deeply embedded documents (like an item in an actorDelta in a token in a scene), which are all stored in-memory on the client. The upside to this is performance — a world that doesn't use compendiums can be extremely laggy and rapidly exceed the client computers capabilities. The downside is operations with compendium documents requires understanding [Javascript's handling of Asynchronous functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) and the limitations that brings (for example, the `pre` document lifecycle hooks operates synchronously and cannot `await` a call to check in with a compendium document).

### Manifest Definition

Compendiums must be defined in your package manifest (`module.json` or `system.json`). They have two relevant fields: `packs` and `packFolders`, which are explained below.

#### packs

An array of packs, which are defined as follows:

```json
{
	"name": "string",
  "label": "string",
  "banner": "string",
  "path": "string",
  "type": "string",
  "system": "string",
  "ownership": Record<CONST.USER_ROLES,CONST.DOCUMENT_OWNERSHIP_LEVELS>
  "flags": {}
}
```

- name: Effectively the ID of the compendium for fetching it, used by the API
- label: The displayed string of the compendium. This is NOT run through localization at any point.
- banner: An image file path for the background banner of the compendium.
- path: A file path to the levelDB files that represent this compendium. You do not need to manually set this, Foundry will handle it.
- type: The document type associated with the compendium, e.g. Actor or Item. Defined by CONST.COMPENDIUM_DOCUMENT_TYPES.
- system: The system ID this compendium can be used with; mandatory for Actors and Items. Defined by CONST.SYSTEM_SPECIFIC_COMPENDIUM_TYPES.
- ownership: An object of user roles and ownership levels. Set all roles to NONE to make a compendium effectively invisible to users but still accessible programatically.

#### packFolders

An array of folders, which are defined as follows:

```json
{
	"name": "string",
	"sorting": "a",
	"color": "string",
	"packs": ["strings"],
	"folders": []
}
```

- name: The name of the folder as it will display
- sorting: `'m'` or `'a'` for manual or alphabetic.
- color: A 6-character color hex code e.g. `#123456`
- packs: An array of pack `name` that go in this folder
- folders: An array of further-nested folders, repeating this structure. Maximum depth of 4.

The `packFolders` are only loaded once per world (per package); if you want to see how updates to the module property are working, run `game.settings.set("core", "compendiumConfiguration", {});`.

## API Interactions

The following sections elaborate on specific methods and properties of the CompendiumCollection class.

### Accessing a CompendiumCollection

The usual way to access a CompendiumCollection is with `game.packs.get(packageID.packName)`; note here that this call namespaced by its parent package's ID, meaning you can name a compendium pack `spells` without collision concerns with other packages that also have a `spells` compendium. World compendiums are a special case - they simply use `world` rather than the world's ID as a namespace.

`game.packs` is an instance of `class CompendiumPacks extends DirectoryCollectionMixin(Collection)`, which mostly means there's some built-in folder handling and sorting on top of the core [Collection API](https://foundryvtt.com/api/classes/foundry.utils.Collection.html). In additiont to `get`, you can use `filter`, `forEach`, `map`, and `reduce` to programatically access many compendiums. 

```js
// Returns an array of compendiums
const itemPacks = game.packs.filter(p => p.documentName === "Item")

console.log(itemPacks)

// Produces a record of compendiums that could be used in a Select dropdown
const packRecord = game.packs.reduce((record, p) => {
   // two getters that fetch metadata.id and metadata.label respectively
	record[p.collection] = p.title;
  return record;
}, {})

console.log(packRecord)
```

And of course like in core JS, you can chain these operations, like leading with a `filter` then executing a `map` or `reduce`.

### Configuration

Compendiums are not documents, which means they do not have an `update` method. Instead, `CompendiumCollection#configure(configuration)` allows you to edit them. The `configuration` argument is an object with the following arguments:

- **locked** (Boolean) - Whether documents in this compendium can be edited.
- **folder** (Folder|string|null) - A folder or folder ID to put this compendium into. Null returns it to the root of the directory.
- **sort** (Number) - The sort value if the containing folder is set to manual.
- **ownership** (Record<CONST.USER_ROLES,CONST.DOCUMENT_OWNERSHIP_LEVELS>) The permissions for the compendium.

This operation is asynchronous; it's a wrapper for an update to `game.settings`. Any API call looking to batch edit compendiums should consider opening and closing with `collection.configure({locked: false})` and `collection.configure({locked: true})`.

### CompendiumCollection#index

When a compendium is loaded into a world, it establishes an index of all the documents inside of it. This index has a very limited subset of the document data, for example Actor and Item store these fields:

```js
{
	_id: string,
	name: string,
  img: string,
  type: string,
  sort: number,
  folder: string // the parent folders id
}
```

The advantage is that the index is accessible in a synchronous fashion. The index is a [Collection](https://foundryvtt.com/api/classes/foundry.utils.Collection.html), so all the usual operations are availabe; `get` and `getName` to fetch individual index entries, `map` and `filter` to obtain subsets, and other such operations.

#### Modifying the indexable fields

By default, the indexed fields are set in `Document.metadat.compendiumIndexedFields` for any given document type. However, you can configure additonal fields in `CONFIG[documentName].compendiumIndexFields`; for example, dnd5e sets `CONFIG.Item.compendiumIndexFields = ["system.container"]` so it can properly render how items are stored in containers.

### Fetching Documents

Developers frequently need to get data from documents in compendia. This lists out the most popular ways to do so. After fetching a document, it lives in the collection's cache for 5 minutes; after that time is up, it's flushed out of the cache and must be retrieved from the server.

#### Single Document

**CompendiumCollection#get(id)/getName(name)**: Unlike in-world collections, these methods are only able to fetch from the cache, so unless you've just accessed the document it's likely to return undefined. If you need to synchronously grab an item, use the index with a call like `CompendiumCollection#index.getName(name)`.

**async CompendiumCollection#getDocument(id)**: The canonical way to grab a document from a `CompendiumCollection` is with `getDocument`. This operation is asynchronous.

**async fromUuid(uuid)**: This is a globally-available function that will parse a UUID and retrieve the document from a compendium if it is inside one. This operation is asynchronous.

**fromUuidSync(uuid)**: This is a globally-available function that will parse a UUID. Unlike the other functions here, if the document is inside a compendium it will return the index; an improvement on `get`/`getName` but without the asynchronous retrieval of `getDocument`/`fromUuid`. This is a very useful operation when you need to synchronously check a `name` or `type` field, since those are contained in the index.

#### Many Documents

**async CompendiumCollection#getDocuments(query)**: This method returns an array of documents. The query parameters are currently undocumented, but the client-side code provdies the following examples:

```js
// Get Documents that match the given value only.
await pack.getDocuments({ type: "weapon" });

// Get several Documents by their IDs.
await pack.getDocuments({ _id__in: arrayOfIds });

// Get Documents by their sub-types.
await pack.getDocuments({ type__in: ["weapon", "armor"] });
```

If no query parameter is passed, `getDocuments` returns an array of *ALL* documents in the compendium.

The standard Collection methods `filter`, `forEach`, `reduce`, `map`, like `get` and `getName` are limited to the documents in the cache; you're better off calling those methods on either the return of `getDocuments` or on the index then using its ID property to fetch documents.

### Updating Documents

Updating a document in a compendium requires that it is unlocked; you can perform this operation programatically with `CompendiumCollection#configure({locked: false})`.

#### Single Document

Updating a single document is simple once you've fetched it - you can use the normal `Document#update` method, like you would with any in-world document.

#### Many Documents

If you need to perform updates on many documents in a collection you can use the `updateAll` method that is inherited from DocumentCollection.

```js
  /**
   * Update all objects in this DocumentCollection with a provided transformation.
   * Conditionally filter to only apply to Entities which match a certain condition.
   * @param {Function|object} transformation    An object of data or function to apply to all matched objects
   * @param {Function|null}  condition          A function which tests whether to target each object
   * @param {object} [options]                  Additional options passed to Document.update
   * @return {Promise<Document[]>}              An array of updated data once the operation is complete
   */
  async updateAll(transformation, condition=null, options={})
```

For example, to set the image of all documents in the collection to `icons/svg/biohazard.svg`, you could run `CompendiumCollection#updateAll({img: 'icons/svg/biohazard.svg'})`. Alternatively, you can pass functions for both Transformation and Condition; the signature in each case is `(doc) => object` and `(doc) => boolean` respectively. Examples of this can be [found below](#using-updateall).

### Importing documents

#### Single Document

**CompendiumCollection#importDocument**

#### Many Documents

**CompendiumCollection#importFolder**

## Specific Use Cases

### Using updateAll

### foundryvtt-cli

[The official Foundry VTT CLI](https://github.com/foundryvtt/foundryvtt-cli) is a tool to unpack compendiums from binary files into a human readable state - JSON or YAML.

## Troubleshooting