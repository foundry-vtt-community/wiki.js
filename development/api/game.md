---
title: Game
description: The core Game instance which encapsulates the data, settings, and states relevant for managing the game experience. The singleton instance of the Game class is available as the global variable game.
published: true
date: 2024-03-21T16:05:23.612Z
tags: documentation
editor: markdown
dateCreated: 2024-03-21T00:03:08.559Z
---

![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

The Game class is the upper-most level of Foundry's data architecture and is responsible for initializing the application. It's usually referenced by the singleton `game` instance.

Official documentation
- https://foundryvtt.com/api/classes/client.Game.html

The `Game` class relies heavily on read-only properties which are not documented by the official JSDoc, you can see the class in more detail in your local install at `foundryInstallPath\resources\app\client\game.js`.

**Legend**

```js
Game.create // `.` indicates static method or property
Game#system // `#` indicates instance method or property
game.system // Lowercase game also indicates instance method or property
```

## Overview

When a client connects to a Foundry server, after all relevant system and module code is imported, the Game class is initialized in the globally available instance `game`. This process instantiates everything else available to Foundry developers, such as loading information from the databases and initializing the canvas.

More information on how data is processed is available in the [From Load to Render](/en/development/guides/from-load-to-render) guide.

---
## Key Concepts

The following lays out how the `game` object acquires its properties.

### Constructor

The basics of how the `game` object is instantiated and its properties are filled in are as follows

1. `Game.create` is called after all code is imported
	a. `Game.connect` establishes the underlying socket connection with the server
	b. `Game.getData` fetches the live data from the database.
	c. `new Game` constructs the singleton instance which is publicly stashed as `game`
2. `Game.initialize` is called, which progressively builds out the `game` instance and calls a series of hooks.

Note that the globally-available `CONST` and `CONFIG` objects are simply initialized as part of loading the relevant [javascript module files](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) (`common\constants.mjs` and `client\config.js` respectively). However, one should be hesitant to modify them before the `init` hook, to avoid any issues with module load order.

### Game initialization hooks

Developers of all kinds almost always need to work with one or more of the following hooks. This section provides information on what is available at each stage. Each of these hooks is only called once, so there's no functional difference between `Hooks.once("init", callback)` and `Hooks.on("init", callback)`.

#### init

The first `Hook` available to a Foundry developer. The following are typical actions performed in this hook.
- The `CONFIG` object is modified
- Document classes are registered
- Sheet classes are registered
- Game settings are registered

This is called before any of the other processes in `Game#initialize`; as such, the only properties available are the ones added in the constructor. These are constructed as read-only properties and so do not currently show up in Foundry's documentation.
- view
- data
- release
- userId
- system
- modules
- workers
- sessionId
- socket
- time
- audio
- clipboard
- debug
- loading
- ready (starts `false`)

The following properties are technically available but are not yet properly initialized with their data
- collections
- packs
- i18n
- keyboard
- mouse
- gamepad
- nue
- permissions
- settings
- keybindings
- canvas (and its global pointer `canvas`)
- video
- tooltip
- tours
- documentIndex
- issues


#### i18nInit

Technically called at the end of `Localization#initialize`, this hook is called after the the following methods:
- `Game#registerSettings` — Registers various core settings
- `game.i18n` is fully set up, so `game.i18n.localize` and other similar methods are available

#### setup

This hook is less commonly used. It's called after the following are established
- `Game#registerTours` — initializes `game.tours`
- `Game#activateListeners` — initializes `game.tooltip`, `game.video`
- `game.permissions` — loaded from world setting
- `Game#initializePacks` — initalizes `game.packs`
- `Game#initializeDocuments` — initializes `game.collections` and instantiates world collections like `game.actors` for all primary document types.

The main point of the `setup` hook is it's after document data is loaded but before the canvas is initalized.

#### ready

The final step of Foundry's initialization process, this hook is called after all of these other methods are called.
- `Game#initializeRTC` — initializes `game.webrtc`
- `Game#initializeMouse` — initializes `game.mouse`
- `Game#initializeGamepads` — initializes `game.gamepad`
- `Game#initializeKeyboard` — initializes `game.keyboard` and `game.keybindings`
- `Game#initializeCanvas` — initializes `game.canvas`
- `Game#initializeUI` — Renders UI elements like the sidebar
- `DocumentSheetConfig.initializeSheets` — processes registered sheet classes
- `Game#activateSocketListeners` — Enables various pieces of interactivity and data-sharing that occur over sockets
- `DocumentIndex#index` — initializes `game.documentIndex`
- `NewUserExperience#initialize` — initializes `game.nue`
- `game.ready = true`

### World Collections

The `game` object is where the in-world [documents](/en/development/api/document) are stashed. Each of the primary document types has a [WorldCollection](https://foundryvtt.com/api/classes/client.WorldCollection.html), which is sourced from `[Class].metadata.collection`, e.g. `Actor.metadata.collection` returns `"actors"`, and so `game.actors` is a collection of actors.

Unlike [Compendium Collections](/en/development/api/CompendiumCollection), all documents in world collections are stored in-memory on the client, so access can be performed synchronously. Updates are still asynchronous to allow synchronization between clients.

It's worth noting that `game.collections` is itself a [Collection](https://foundryvtt.com/api/classes/foundry.utils.Collection.html) of these world collections. However, it's basically never necessary to use that.


---
## API Interactions

### World Collection Document Access

WorldCollection extends [DocumentCollection](https://foundryvtt.com/api/classes/client.DocumentCollection.html), which provides the bulk of methods for interacting with the stored documents.

#### Accessing individual documents

There's two main ways to get documents from a world collection, both of which run synchronously.
- `WorldCollection#get`, which fetches by document `id`.
- `WorldCollection#getName`, which fetches by the document's `name` property.

The global methods, `fromUuid` and `fromUuidSync` both work with documents in world collections; `fromUuidSync` in particular is quite useful because it remains synchrous and has no problem fetching embedded documents.

**Invalid Documents**: Sometimes, a [data model](/en/development/api/DataModel) update will mark a document as invalid if it fails to cast data (a common example is `Number('30 ft')` returns `NaN`). In this cases, there's a few ways to access invalid documents
- `DocumentCollection#getInvalid(id)` is the primary canonical way.
- `DocumentCollection#invalidDocumentIds` is a [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set), which can be iterated through with a `for` loop or accessed with a simple `invalidDocumentIds.first()`.

#### Accessing many documents

The upstream Collection class provides several ways of accessing the values stored inside: `for` and `forEach` iteration, `map`, `reduce`, `filter`, and `some`.

The `DocumentCollection` class adds the `search` function, which will look through all searchable fields based on the data model's metadata.

A third option is to use `WorldCollection#folders`, but this is generally inferior to just using `filter(d => d.folder.id === 'targetFolderID')` or some other construction with those basic methods inherited from Collection.

---
## Specific Interactions
> Stub
> This section is a stub, you can help by contributing to it.
---
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.