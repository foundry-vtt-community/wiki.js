---
title: From Load to Render
description: Tracking the permutation of data from the server database to a document sheet rendering.
published: false
date: 2024-02-13T08:07:20.057Z
tags: documentation
editor: markdown
dateCreated: 2024-02-13T08:07:20.057Z
---


![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

Data in Foundry progresses through many stages to be rendered in a document sheet. This guide is intented


This guide will reference file locations rather than the official API docs because this is focusing on the internal workings of methods that are not well-commented, 

All file paths are all relative to `foundryInstallPath\resources\app`. Technically your installation just loads the general `public\scripts\foundry.js` file, but the files in `client` and `common` are smaller and easier to follow.

> **Legend**
> `Class.method` is a static method or property on the given class
> `Class#method` is an instance method or property
{.is-info}
  
## Data Model Instantiation

### Game Setup

Foundry begins its path with `client\tail.js`, which adds an event listener, `DOMContentLoaded`, that fires after all of the files Foundry is pulling in have loaded (e.g. all the `esmodules`). `Game.create` is called and the returned reference is stashed in `globalThis.game`. This method establishes a socket connection, uses it to fetch the raw data from the world databases with `Game.getData()`, and constructs the `Game` instance, putting all the raw database info in `game.data` - the first step of our Document journey.

The next step is `Game#initialize`, where we get that `init` Hook call. This method eventually calls `Game#_initializeGameView`, which is really just a license-check wrapper for `Game#setupGame`. Inside *that* method we get `Game#initializePacks` and `Game#initializeDocuments`, the second step of our Document journey (after which the `setup` hook is called).

The first of these two methods, `initializePacks`, only instantiates the indices of the packs. The second, `initializeDocuments`, loops through all of the document data stashed in `game.data`, calls the appropriate constructor, and then puts it in the appropriate world collection. For example, `game.data.actors` gets fed into `new Actor` and the result is poured inside `game.actors`.


## Data Model

The top level inheritance for documents traces back to the `DataModel` class, found in `common\abstract\data.mjs`, which controls the earliest steps of processing.

### DataModel#\_initializeSource
In the DataModel constructor, the data is first run through `DataModel#_initializeSource` and then stored in `_source`. This method applies `DataModel#migrateDataSafe`, `DataModel#cleanData`, and `DataModel#shimData`, ensuring the constructed document matches the specifications before saving it in `_source`. After putting the cleaned up data in `_source`, `DataModel#validate` runs for one last check.

### DataModel#\_initialize

## Document

### Document#prepareBaseData

### Document#prepareEmbeddedDocuments

### Document#prepareDerivedData

## DocumentSheet

### DocumentSheet#getData