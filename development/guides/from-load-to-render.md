---
title: From Load to Render
description: Tracking the permutation of data from the server database to a document sheet rendering.
published: true
date: 2024-02-14T19:57:58.228Z
tags: documentation
editor: markdown
dateCreated: 2024-02-13T08:07:20.057Z
---


![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

Data in Foundry progresses through many stages to be rendered in a document sheet. This guide is intended to be a reference for developers trying to understand where they should define data the order in which it is processed.

This guide will reference file locations rather than the official API docs because this is focusing on the internal workings of methods that are not well-commented. All file paths are all relative to `foundryInstallPath\resources\app`. Technically your installation just loads the general `public\scripts\foundry.js` file, but the files in `client` and `common` are smaller and easier to follow.

> **Legend**
> `Class.method` is a static method or property on the given class
> `Class#method` is an instance method or property
{.is-info}
  
## Loading from the Database

### Game.getData

Foundry begins its path with `client\tail.js`, which adds an event listener, `DOMContentLoaded`, that fires after all of the files Foundry is pulling in have loaded (e.g. all the `esmodules`). `Game.create` is called and the returned reference is stashed in `globalThis.game`. This method establishes a socket connection, uses it to fetch the raw data from the world databases with `Game.getData()`, and constructs the `Game` instance, putting all the raw database info in `game.data` - the first step of our Document journey.

### Calling the Document Constructor

The next step is `Game#initialize`, where we get that `init` Hook call and `"Foundry VTT | Initializing Foundry Virtual Tabletop Game"` printed to console. This method eventually calls `Game#_initializeGameView`, which is really just a license-check wrapper for `Game#setupGame`. Inside *that* method we get `Game#initializePacks` and `Game#initializeDocuments`, the second step of our Document journey (after which the `setup` hook is called).

The first of these two methods, `initializePacks`, only instantiates the indices of the packs. The second, `initializeDocuments`, loops through all of the document data stashed in `game.data`, calls the appropriate constructor, and then puts it in the appropriate world collection. For example, `game.data.actors` gets fed into `new Actor` and the result is poured inside `game.actors`. When all of these documents are loaded `"Foundry VTT | Prepared World Documents in ${Math.round(dt)}ms"` is printed to console.

The following sections describe what happens inside these constructor calls.

## DataModel

The top level inheritance for documents traces back to the `DataModel` class, found in `common\abstract\data.mjs`, which controls the earliest steps of processing.

### DataModel#\_initializeSource
In the DataModel constructor, the data is first run through `DataModel#_initializeSource` and then stored in `_source`. This method applies `DataModel#migrateDataSafe`, `DataModel#cleanData`, and `DataModel#shimData`, ensuring the constructed document matches the specifications from its `schema` property before saving it in `_source`. After putting the cleaned up data in `_source`, `DataModel#validate` runs for one last check.

### DataModel#\_initialize

This method copies data from `_source` to the top level, using the `schema` field as a guide. Each property of the schema gets run through the appropriate `DataField#initialize` method, which performs operations like transforming a stored ID string in `ForeignDocumentField#initialize` to a pointer.

## ClientDocument

The next step is specific to client documents, as these processes are not run in the server. The ClientDocumentMixin is found in `client\data\abstract\client-document.js` and takes a `Document` as its argument, for example `BaseActor`. This document provides the necessary schema information for all of the other processes to work.

Our main function of concern here is `ClientDocument#prepareData`, which is wrapped by `ClientDocument#_safePrepareData` with error-catching protections. The actual function is quite straightforward:

```javascript
    /**
     * Prepare data for the Document. This method is called automatically by the DataModel#_initialize workflow.
     * This method provides an opportunity for Document classes to define special data preparation logic.
     * The work done by this method should be idempotent. There are situations in which prepareData may be called more
     * than once.
     * @memberof ClientDocumentMixin#
     */
    prepareData() {
      const isTypeData = this.system instanceof foundry.abstract.TypeDataModel;
      if ( isTypeData || (this.system?.prepareBaseData instanceof Function) ) this.system.prepareBaseData();
      this.prepareBaseData();
      this.prepareEmbeddedDocuments();
      if ( isTypeData || (this.system?.prepareDerivedData instanceof Function) ) this.system.prepareDerivedData();
      this.prepareDerivedData();
    }
```

On initial world load, this call is deferred until after ALL documents have been created. Afterwards, it's called for a document after it's created or updated. It can be manually called again as a side effect of other operations that wouldn't otherwise trigger it.

Everything here should be performed using standard javascript assignment (`=`). Calling `update` within `prepareData` or its subsidiary methods can easily trigger an infinite loop, as that `update` call then creates another `prepareData` call.  

### prepareBaseData

This is a good place to initialize any values that aren't stored in the database but are targets for active effects, such as creating a blank array or setting a value to 0. More complicated calculations should usually wait until `prepareDerivedData` after active effects have been applied, but your specific use case may disagree.

#### TypeDataModel#prepareBaseData

If you have a system data model, you can run type-specific logic here. Keep in mind that you're operating within the `system` object, so you'll need to call `this.parent` to access the actual document properties, e.g. `this.parent.items` to access the items collection.

#### ClientDocument#prepareBaseData

Most documents do not do anything here natively; only ActiveEffect, Scene, and Token natively do things here, and ActiveEffect's handling covers a deprecated field. 


### ClientDocument#prepareEmbeddedDocuments

Baseline, this loops through each of the Document's collections and calls `prepareData` on each document. Each document has a defined hierarchy to handle possible multiple collections; in the case of actors, Items are prepared before ActiveEffects. The Actor class follows this up with a call to `applyActiveEffects`, which provides native handling for data modifications.

Developers usually don't need to make direct edits here; they're better off making changes in the appropriate `prepareData` methods of the embedded documents or in `applyActiveEffects`.

### prepareDerivedData

This is the final place to manipulate a document's data in a way that is generally accessible within the foundry API. Derived data is the place to calculate modifiers, encumbrance, and all of the other pieces of information you want available. What actually needs to be derived depends enormously on what you're building; trad games like D&D, PF, or SWADE have lots of derived data, while more rules light games like various PbtAs have very little derived data.

#### TypeDataModel#prepareDerivedData

If you have a system data model, you can run type-specific logic here. Keep in mind that you're operating within the `system` object, so you'll need to call `this.parent` to access the actual document properties, e.g. `this.parent.items` to access the items collection.

#### ClientDocument#prepareDerivedData


Many document classes have their own native handling here that you should keep in mind to call `super.prepareDerivedData()` for:

* ActiveEffect updates its duration
* Cards ensure all of their faces have images to display
* Chat messages construct rolls from their roll data
* Combats sorts turns
* Combatants pull their display image and update their displayed tracked resource
* Playlists update their `playing` property
* Tiles derive their dimensions
* Users ensure they have images available.


## DocumentSheet

The final step in the journey to displaying a document involves the application class stack. Many classes extend `DocumentSheet`, which can be found in `client\apps\form.js`. This extends `FormApplication` which extends `Application`. There's a lot of logic here that isn't relevant to our goal, which is "How does the document data we've defined in the previous sections get displayed to an end user"; drag handlers, listeners, and all the other methods are the topic of another guide.

### Application#render

The journey starts with a call to `Application#render(true)`, which "forces" the application to render; by default, `render` will only refresh an already displayed application. This encapsulates `_render` with some error handling.

Inside `Application#_render`, there are several stages of processing possible application render states, including printing `"Foundry VTT | Rendering ${this.constructor.name}"` and merging in the options until finally a call is made to `Application#getData`. The return of this is used by `Application#_renderInner` to call `renderTemplate(this.template, data)`, constructing the html that is eventually displayed.

### DocumentSheet#getData

For a `DocumentSheet`, there are many layers of overrides to that `getData` call. Between it and `FormApplication`, you end up with a javascript object that looks like this:
```javascript
const isEditable = this.isEditable;
{
  object: this.object,
	options: this.options,
	title: this.title,
  cssClass: isEditable ? "editable" : "locked",
  editable: isEditable,
  document: this.document,
  data: data,
  limited: this.document.limited,
  options: this.options,
  owner: this.document.isOwner,
  title: this.title
}
```

(note: `object` is the target of the FormApplication and is the same as `document` for a DocumentSheet)

Classes like `ActorSheet` or `ItemSheet` often add pieces to this, e.g. adding another pointer named after the document class. What happens here though is ultimately only for the purposes of being fed into a `renderTemplate` call, and so everything that happens here is for *display* logic.

Things that are good to do in getData
- Associate labels
- Split embedded documents up into categories (e.g. items by type and active effects by enabled)
- Sort embedded documents (ActorSheet already does this for items)
- Construct `selectOptions` for dropdown fields

After the template is rendered, the `context` is handed off to the `renderDocumentSheet` hook and then evaporates, which is why any non-display logic should be processed in the previous steps.

## Conclusion

This hopefully was a helpful walkthrough of how information flows from the database to actually being rendered on a user's screen.