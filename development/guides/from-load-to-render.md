---
title: From Load to Render
description: Tracking the permutation of data from the server database to a document sheet rendering.
published: true
date: 2025-09-20T21:53:16.446Z
tags: documentation
editor: markdown
dateCreated: 2024-02-13T08:07:20.057Z
---


![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

Data in Foundry progresses through many stages to be rendered in a document sheet. This guide is intended to be a reference for developers trying to understand where they should define data the order in which it is processed.

This guide will reference file locations rather than the official API docs because this is focusing on the internal workings of methods that are not well-commented. All file paths are all relative to `foundryInstallPath/resources/app`. Technically your installation just loads the general `public/scripts/foundry.mjs` file, but the files in `client` and `common` are smaller and easier to follow.

**Legend**
```
Class.method is a static method or property on the given class
Class#method is an instance method or property
```  
## Loading from the Database

The server loads up data from the various databases as part of opening a world from the setup screen, transmitting it to users who login to or refresh the `/game` page.

### Game.getData

Foundry begins its path with `client/client.mjs`, which adds an event listener, `DOMContentLoaded`, that fires after all of the files Foundry is pulling in have loaded (e.g. all the `esmodules`). `Game.create` is called and the returned reference is stashed in `globalThis.game`. This method establishes a socket connection, uses it to fetch the raw data from the world databases with `Game.getData()`, and constructs the `Game` instance, putting all the raw database info in `game.data` - the first step of our Document journey.

### Calling the Document Constructor

The next step is `Game#initialize`, where we get that `init` Hook call and `"Foundry VTT | Initializing Foundry Virtual Tabletop Game"` printed to console. This method eventually calls `Game##initializeGameView`, which is really just a license-check wrapper for `Game#setupGame`. Inside *that* method we get `Game#initializePacks` and `Game#initializeDocuments`, the second step of our Document journey (after which the `setup` hook is called).

The first of these two methods, `initializePacks`, only instantiates the indices of the packs. The second, `initializeDocuments`, loops through all of the document data stashed in `game.data`, calls the appropriate constructor, and then puts it in the appropriate world collection. For example, `game.data.actors` gets fed into `new CONFIG.Actor.documentClass` and the result is poured inside `game.actors`. When all of these documents are loaded `"Foundry VTT | Prepared World Documents in ${Math.round(dt)}ms"` is printed to console.debug.

The following sections describe what happens inside these constructor calls as well as what happens after a database `update`.

## DataModel

API Reference: https://foundryvtt.com/api/classes/foundry.abstract.DataModel.html

The top level inheritance for documents traces back to the [DataModel](/en/development/api/DataModel) class, found in `common/abstract/data.mjs`, which controls the earliest steps of processing. These are run 

### DataModel#\_initializeSource
In the DataModel constructor, the data is first run through `DataModel#_initializeSource` and then stored in `_source`. This method applies `DataModel#migrateDataSafe`, `DataModel#cleanData`, and `DataModel#shimData`, ensuring the constructed document matches the specifications from its `schema` property before saving it in `_source`. 

DataModel then calls `_configure`, which does nothing in the base class but in Document is responsible for assigning the `parentCollection` and `pack` properties. After these adjustments, `DataModel#validate` runs for one last check.

**Validation**: Most validation occurs on the `DataField` level. If a validator returns a boolean, no further checks are needed - e.g. `_validateSpecial` will return either `true` or `false` if the `value` is `null` or `undefined`, but if it's neither then it will return `void` to pass the value on to be checked by `_validateType`. Unlike the other steps in `_initializeSource`, `validate` is also called every update.

### DataModel#\_initialize

This method copies data from `_source` to the top level, using the `schema` field as a guide. Each property of the schema gets run through the appropriate `DataField#initialize` method, which performs operations like transforming a stored ID string in `ForeignDocumentField#initialize` to a pointer to the document instance. Unlike `_initializeSource`, this runs after every document update.

**TypeDataField**: This field stands out because it grabs a reference to `CONFIG?.[this.documentName]?.dataModels?.[type]` to check if there's a class to instantiate for the `system` property, otherwise it's created as a plain object. This means that `system` is baseline an open-ended ObjectField; it will save any data so long as you don't directly try to save `'doc.system': false` or some kind of non-object value. 

If you're only using `template.json` to define properties, any missing properties will get filled in from the template but otherwise it provides no other validation or handling; if a user updates a property to be a boolean but your `template.json` value is a string, Foundry won't reject the update, but if a user deletes a property provided by `template.json` then it will replace that property with the default.

## ClientDocument

API Reference: https://foundryvtt.com/api/classes/foundry.ClientDocument.html

The next step is specific to client documents, as these processes are not run in the server. The `ClientDocumentMixin` is found in `client/documents/abstract/client-document.mjs` and takes a `Document` as its argument, for example `BaseActor`. This document provides the necessary schema information for all of the other processes to work.

Our main function of concern here is `ClientDocument#prepareData`, which is wrapped by `ClientDocument#_safePrepareData` with error-catching protections. The actual function is quite straightforward:

```javascript
    prepareData() {
      const isTypeData = this.system instanceof foundry.abstract.TypeDataModel;
      if ( isTypeData ) this.system.prepareBaseData();
      this.prepareBaseData();
      this.prepareEmbeddedDocuments();
      if ( isTypeData ) this.system.prepareDerivedData();
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

### Setup

After all documents have prepared their data for the first time the `setup` hook fires.

## DocumentSheet

[*App V1*](/development/api/application)

The final step in the journey to displaying a document involves the application class stack. Many classes extend `DocumentSheet`, which can be found in `client\apps\form.js`. This extends `FormApplication` which extends `Application`. There's a lot of logic here that isn't relevant to our goal, which is "How does the document data we've defined in the previous sections get displayed to an end user"; other features are covered in [Application](/en/development/api/application).

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

Things that are good to do in `getData`
- Associate labels
- Enrich editor data
- Split embedded documents up into categories (e.g. items by type and active effects by enabled)
- Sort embedded documents (ActorSheet already does this for items)
- Construct `selectOptions` for dropdown fields

After the template is rendered, the `context` is handed off to the `renderDocumentSheet` hook and then evaporates, which is why any non-display logic should be processed in the previous steps.

## DocumentSheetV2

[*AppV2*](/development/api/applicationv2)

Alternatively, for `HandlebarsApplicationMixin(DocumentSheetV2)`, the relevant rendering processes involve three classes; `ApplicationV2`, `DocumentSheetV2`, and `HandlebarsApplicationMixin`, all of which can be found in the `resources/app/client-esm/applications/api` directory.

### ApplicationV2#render

The journey starts with `ApplicationV2#render({ force: true })`; this `options` object is of type [ApplicationRenderOptions](https://foundryvtt.com/api/interfaces/foundry.applications.types.ApplicationRenderOptions.html). Like AppV1, `force` controls whether to freshly-render an application not displayed on the screen. Unlike AppV1, `render` *is* asynchronous and returns a promise that resolves when the application is fully rendered.

The `render` method then calls its private partner, `ApplicationV2##render`. This calls a series of protected functions, only some of which are used by the three classes used above. You can always extend or override these; just keep in mind that the choice of if and where to call `super` can impact expected functionality.

1. `_canRender`, which in DocumentSheetV2 checks against the user's `viewPermission`.
2. `_configureRenderOptions`, which is used by ApplicationV2 to update the window and position properties and by HandlebarsApplicationMixin to decide what parts to show (by default all).
3. `_prepareContext`, which will prepare tabs for applications with a single tab group, and in DocumentSheetV2 will assign commonly referenced properties like `document` and `editable`. (details below).
4. `_preFirstRender`, which is blank in ApplicationV2 & DocumentSheetV2 but implemented by some core applications.
5. `_preRender`, which is used by HandlebarsApplicationMixin to load the additional templates necessary for rendering.
6. `_renderFrame`, which is used by both ApplicationV2 and DocumentSheetV2 to initialize key application properties such as the close and UUID buttons.
7. `_attachFrameListeners`, which is used by ApplicationV2 to attach listeners to the whole frame such as Form Submission handling.
8. `_renderHTML`, which is implemented by HandlebarsApplicationMixin to generate the HTML of all the parts. This function calls `_preparePartContext` for additional support.
9. `_replaceHTML`, which is implemented by HandlebarsApplicationMixin and replaces any previously rendered HTML for a part with the freshly generated HTML.
10. `_insertElement`, which is used by ApplicationV2 to add the Application to the window.
11. `_updateFrame`, which is used by ApplicationV2 to update the header bar properties.
12. `_onFirstRender`, which is used by DocumentSheetV2 to add the application to the document's `apps` record. 
13. `_onRender`, which is used by DocumentSheetV2 to disable inputs if the document is not editable.
14. The `renderApplicationV2` hook is called but not awaited, so any asynchronous callbacks are not guaranteed to finish before the render proceeds.
15. `_postRender`, which is blank in ApplicationV2 & DocumentSheetV2 but implemented by some core applications.

Broadly speaking, `ApplicationV2` handles the window frame + header, while `HandlebarsApplicationMixin` is responsible for the internal HTML. Everything except `_replaceHTML`, `_insertElement`, and `_updateFrame` is asynchronous and awaited. 

### \_prepareContext and \_preparePartContext

The most important step for the typical developer and one that almost certainly needs overrides are the asynchronous functions [`_prepareContext`](https://foundryvtt.com/api/classes/foundry.applications.api.ApplicationV2.html#_preparecontext) (added by ApplicationV2) and [`_preparePartContext`](https://foundryvtt.com/api/classes/foundry.HandlebarsApplication.html#_preparepartcontext) (added by HandlebarsApplicationV2). 

The reason this step is important is that the `context` object returned by these functions is fed into a `renderTemplate` call by `HandlebarsApplicationMixin#_renderHTML`. This is where all *display* logic should be handled, such as:
- Associate labels
- Enrich editor data
- Split embedded documents up into categories (e.g. items by type and active effects by enabled)
- Sort embedded documents
- Construct `selectOptions` for dropdown fields

A very basic `_prepareContext` call should look something like the following. `this.document` and `this.isEditable` are two getters defined by DocumentSheetV2.

```js
async _prepareContext(options) {
  const context = await super._prepareContext(options)
  
  return Object.assign(context, {
    system: this.document.system,
    systemFields: this.document.system.schema.fields,
  })
}
```

Then, in `_preparePartContext`, you can wrap your logic in a `switch(partId) {}` statement to prepare elements as needed. This can both reduce the amount of work if you're only selectively rendering parts (such as if a user only has limited permissions) as well as let you cleanly redefine variables across different parts.

> Don't be afraid to delegate complex preparation logic to protected or private functions; `context.items = await this._prepareItems(context, options)` can help keep your `_prepareContext` function readable.
{.is-info}

After the context from `_preparePartContext` is used in the `renderTemplate` call it's discarded, so non-display logic should always happen in the earlier document preparation steps. Similarly, while the `context` object from `_prepareContext` is passed into many of the functions outlined earlier (e.g. `_onFirstRender`), it is not "saved" anywhere.

## Conclusion

This hopefully was a helpful walkthrough of how information flows from the database to actually being rendered on a user's screen.