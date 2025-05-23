---
title: Compendium Collection
description: A collection of Document objects contained within a specific compendium pack.
published: true
date: 2025-05-23T07:09:57.704Z
tags: documentation
editor: markdown
dateCreated: 2024-02-22T09:00:31.352Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

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

The `packFolders` are only loaded once per world (per package); if you want to see how updates to the module property are working, in addition to restarting foundry, you must run `game.settings.set("core", "compendiumConfiguration", {});` and refresh your page twice (make sure the first refresh fully finishes before refreshing again).

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

Compendiums are not documents, which means they do not have an `update` method. Instead, `CompendiumCollection#configure(configuration)` allows you to edit them. The `configuration` argument is an object with the following optional fields:

- **locked** (Boolean) - Whether documents in this compendium can be edited.
- **folder** (Folder|string|null) - A folder or folder ID to put this compendium into. Null returns it to the root of the directory.
- **sort** (Number) - The sort value if the containing folder is set to manual.
- **ownership** (Record<CONST.USER_ROLES,CONST.DOCUMENT_OWNERSHIP_LEVELS>) The permissions for the compendium.

This operation is asynchronous; it's a wrapper for an update to `game.settings`. Any API call looking to batch edit compendiums should consider opening and closing with `collection.configure({locked: false})` and `collection.configure({locked: true})`.

### The Index

When a compendium is loaded into a world, it establishes an `index` of all the documents inside of it. This index has a very limited subset of the document data, for example Actor and Item store these fields:

```js
{
  _id: "string",
  uuid: "string", // Special case: UUID is derived from _id, but is not actually stored
  name: "string",
  img: "string",
  type: "string",
  sort: number,
  folder: "string" // the parent folders id
}
```

The advantage is that the index is accessible in a synchronous fashion. The index is a [Collection](https://foundryvtt.com/api/classes/foundry.utils.Collection.html), so all the usual operations are availabe; `get` and `getName` to fetch individual index entries, `map` and `filter` to obtain subsets, and other such operations. (e.g. `const weapons = game.packs.get('some.pack').index.filter(i => i.type == "weapon")`). The index is modifiable, see "Modifying the indexable fields" under Specific Use Cases below.

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

### Importing and Exporting documents

One other use set of methods that are important when interacting with a CompendiumCollection is moving documents between the compendium and the world. These operations are always asynchronous because it involves database manipulation.

#### Single Document

If you're working with a single document, you can use the native drag handlers or `import` button. The following information is for programmatic handling.

**Option 1:** `Document.create`

This method works in any direction: Importing and exporting. [`Document.create(data, context)`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#create) takes two important properties for `context`: 
- `pack`: A Compendium pack identifier within which the Document should be created  
- `parent`: A parent Document within which this Document should be embedded

If `pack` is null or undefined, then the document will be created in a world collection like `game.actors`. Otherwise, it will be created in the relevant CompendiumCollection.

**Option 2:** `CompendiumCollection#importDocument`

This method creates a new document inside the compendium collection. [`CompendiumCollection#importDocument(document, options)`](https://foundryvtt.com/api/classes/client.CompendiumCollection.html#importDocument) adds some additional cleaning that can be configured in `options`:
- `clearFlags`: Clear the flags object (default: `false`)
- `clearSource`: Clear any prior sourceId flag (default: `true`)
- `clearSort`: Clear the currently assigned sort order (default: `true`)
- `clearFolder`: Clear the currently assigned folder (default: `false`)
- `clearOwnership`: Clear document ownership (default: `true`)
- `clearState`: Clear fields which store document state (default: `true`)
- `keepId`: Retain the current Document id (default: `false`)

**Option 3:** `WorldCollection#importFromCompendium`

This method creates a new document inside the relevant world collection. [WorldCollection#importFromCompendium(pack, id, updateData, options)](https://foundryvtt.com/api/classes/client.WorldCollection.html#importFromCompendium) has a lot of arguments:
- `pack` is the pointer to the pack from which to import
- `id` is the ID of the document in the pack
- `updateData` gets merged *after* the document is modified by `WorldCollection#fromCompendium`

The `options` passed forward to `fromCompendium(options)` include
- `addFlags`: Add flags which track the import source (default: `true`)
- `clearFolder`: Clear the currently assigned folder (default: `false`)
- `clearSort`: Clear the currently assigned folder and sort order (default: `true`)
- `clearOwnership`: Clear document ownership (default: `true`)
- `keepId`: Retain the Document id from the source Compendium (default: `false`)

#### Many Documents

Just like a single document, there's many ways to programatically create large quantities of documents in a compendium collection or from a compendium collection.

**Option 1:** `Document.createDocuments`

This [small evolution from `Document.create(data, context)`](https://foundryvtt.com/api/classes/foundry.abstract.Document.html#createDocuments) takes an *array* of document data, rather than just a single document's data, for the first argument. In fact, one could simply pass an array of data to `Document.create` and it would work identically to this function call. 

**Option 2:** `CompendiumCollection#importAll`

This method imports all documents *from* the CompendiumCollection *to* the relevant world collection. [`CompendiumCollection#importAll(options)`](https://foundryvtt.com/api/classes/client.CompendiumCollection.html#importAll) has two bonus arguments besides the ones it forwards to `WorldCollection#fromCompendium` and `Document.createDocuments`
- `folderId` specifies a folder to import into
- `folderName` alternatively creates a parent folder before importing the documents with the given name.

### CompendiumCollection#folders

The folders embedded into a compendium are not part of the main collection; `getDocuments()` won't return a single folder. Instead, folders are stored in a separate collection accessible as a `folders` property which gets a [CompendiumFolderCollection](https://foundryvtt.com/api/classes/client.CompendiumFolderCollection.html). This is a form of [DocumentCollection](https://foundryvtt.com/api/classes/client.DocumentCollection.html) - all of the [folder documents](https://foundryvtt.com/api/classes/client.Folder.html) are readily available, without any asynchronous operations in the server. The `contents` of these folder documents are always the *index* entries of the child documents.

## Specific Use Cases

Managing compendium content across core and system updates can be tricky; here are some ways to manage that.

### Using `updateAll`

`CompendiumCollection#updateAll` is an incredibly powerful tool, especially in conjunction with other methods. Here's one example that finds all documents in a specific folder and updates `system.type.value` to `"race"`; this can be useful as a script macro to batch update content. Along the way, it will console.log the name of every document it executes an update on.

```js
const collection = game.packs.get('module-id.pack-id')

ui.notifications.info("Beginning Update")
await collection.updateAll(update, filter)
ui.notifications.info("Completed Update")


function update(doc) {
	console.log(doc.name)
  const update = {system: {}}
  update.system.type = {value: "race"}
  return update;
}

function filter(doc) {
  return doc.folder?.id === "7l9VwmhPTzJstiWD"
}
```

Keep in mind the wide expanse of [string operations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) available within Javascript when doing an update — well-constructed RegEx can save a lot of manual work when updating HTML entries.

### Modifying the indexable fields

By default, the indexed fields are set in `Document.metadata.compendiumIndexedFields` for any given document type. However, you can configure additonal fields in `CONFIG[documentName].compendiumIndexFields`; for example, dnd5e sets `CONFIG.Item.compendiumIndexFields = ["system.container"]` so it can properly render how items are stored in containers. You will need to run `CompendiumCollection#getIndex` in a `"setup"` or `"ready"` hook for every pack, as the initial index is constructured from the base document pack data gathered during the start of world load without knowledge of any changes made client side.

```js
Hooks.once("init", () => {
  CONFIG.Item.compendiumIndexFields = ["system.container"];
});

Hooks.once("ready", async () => {
  for (const pack of game.packs.filter(p => p.documentName === "Item")) {
  	await pack.getIndex()
  }
});
```

Keep in mind there is a performance cost to adding properties to the index, its why there's an index rather than just fully loading the document.

### foundryvtt-cli

[The official Foundry VTT CLI](https://github.com/foundryvtt/foundryvtt-cli) is a tool to unpack compendiums from binary files into a human readable state - JSON or YAML. In general, actual document operations should be conducted *within* Foundry - `updateAll` and similar functions have the important benefit of leveraging Foundry's native data checking to ensure you don't corrupt data.

However, one key weakness of Foundry's native database structure is it relies on binary files that change with every access, which conflicts with good GIT management practices. This is where the CLI can be useful, because the unpacked JSON/YAML files are much easier to track changes. You WILL have to repack the files as part of creating a release, but this is doable with git actions.

To help developers, here are two node scripts. These scripts rely on *locally* installing the foundry CLI, which you can do with `npm install @foundryvtt/foundryvtt-cli --save-dev`. This also assumes you've already initialized your module or system folder as a node package. `fs` and `path` are provided natively by node versions that support Foundry. **These scripts can ONLY be run while the compendiums are not in use by a Foundry world**.

This first script coverts the LDB setup in the `packs` directory to YAML files in `src/packs`; you can change the `const yaml = true` to false if you prefer JSON. The `expandAdventures` will create the items from the adventure as their own files, instead of populating them inside the adventure yaml/json file. The `folders` define if you want to create a similar folders structure presented in the compendium.

As you will see, there is "special handler" for the file names. It checks which type of document is passed, using some properties of these documents to find it.

```js
import { extractPack } from "@foundryvtt/foundryvtt-cli";
import { promises as fs } from "fs";
import path from "path";

const MODULE_ID = process.cwd();
const yaml = true;
const expandAdventures = true;
const folders = true;

const packs = await fs.readdir("./packs");
for (const pack of packs) {
  if (pack === ".gitattributes" || pack === ".DS_Store") continue;
  console.log("Unpacking " + pack);
  const directory = `./src/packs/${pack}`;
  try {
    for (const file of await fs.readdir(directory)) {
      const filePath = path.join(directory, file)
      if (file.endsWith(yaml ? ".yml" : ".json")) await fs.unlink(filePath);
      else fs.rm(filePath, { recursive: true })
    }
  } catch (error) {
    if (error.code === "ENOENT") console.log("No files inside of " + pack);
    else console.log(error);
  }
  await extractPack(
    `${MODULE_ID}/packs/${pack}`,
    `${MODULE_ID}/src/packs/${pack}`,
    {
      yaml,
      transformName,
      expandAdventures,
      folders,
    }
  );
}
/**
 * Prefaces the document with its type
 * @param {object} doc - The document data
 */
function transformName(doc, context) {
  const safeFileName = doc.name.replace(/[^a-zA-Z0-9А-я]/g, "_");
  let type = doc._key?.split("!")[1];
  if (!type) {
    if ("playing" in doc)
      type = "playlist";
    else if (doc.sorting)
      type = `folder_${doc.type}`;
    else if (doc.walls)
      type = "scene";
    else if (doc.results)
      type = "rollTable";
    else if (doc.pages)
      type = "journal";
    else
      type = doc.type;
  }
  const prefix = ["actors", "items"].includes(type) ? doc.type : type;

  let name = `${doc.name ? `${prefix}_${safeFileName}_${doc._id}` : doc._id}.${yaml ? "yml" : "json"}`;
  if ( context.folder ) name = path.join(context.folder, name);
  return name;
}

```

This second script does the reverse - it takes the unpacked files from `src/packs` and assembles LevelDB files. Make sure to adjust `const yaml` in this script as well if you prefer JSON.

```js
import { compilePack } from '@foundryvtt/foundryvtt-cli';
import { promises as fs } from 'fs';

const MODULE_ID = process.cwd();
const yaml = true;
const folders = true;

const packs = await fs.readdir('./src/packs');
for (const pack of packs) {
  if (pack === '.gitattributes') continue;
  console.log('Packing ' + pack);
  await compilePack(
    `${MODULE_ID}/src/packs/${pack}`,
    `${MODULE_ID}/packs/${pack}`,
    { yaml, recursive: folders }
  );
}

```

One general tip: If you add these to the `scripts` property to your node `package.json`, you can run the node scripts from wherever in your project and `process.cwd()` will always be the root. For example, both scripts are in a folder named `tools` and the first script is named `pushLDBtoYML.mjs` while the second is `pullYMLtoLDB.mjs`.
```json
  "scripts": {
    "pushLDBtoYML": "node ./tools/pushLDBtoYML.mjs",
    "pullYMLtoLDB": "node ./tools/pullYMLtoLDB.mjs",
  },
```

When working across multiple computers, after pulling any updates to these files, you'll have to use `npm run pullYMLtoLDB` to ensure your local compendium databases reflect the stored YML.


Note that Foundry does not use the unpacked (JSON/YAML) db files, it will continue to operate against the LDB files. Converting to upacked files is only a way to avoid tracking binary files in your source control management system (eg github) - either set .gitignore to ignore your binary db directory (eg /packs) or don't indiscriminately add files to commits.

If you are distributing your compendiums (eg in a module) you will need to add a pipeline action to your project workflow such as:  
*(for github)*
```
      - name: Install Dependencies
        run: npm ci

      - name: Build Packs
        run: |
          npm run pullYMLtoLDB --if-present
```

## Troubleshooting

Here are some of the most common issues when working with compendiums.

### Asynchronicity

Compendium operations involve a lot of asynchronous code, because they rely on client-server communication. You will need to use the `await` keyword in front of these operations, and there are many situations in Foundry you won't be able to use asynchronous functions, such as in `prepareData` or in a `preCreate` hook. 