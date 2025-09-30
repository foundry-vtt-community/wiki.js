---
title: ApplicationV2
description: The Application class is responsible for rendering an HTMLElement into the Foundry Virtual Tabletop user interface.
published: true
date: 2025-09-30T22:08:41.737Z
tags: documentation
editor: markdown
dateCreated: 2024-04-18T15:30:54.955Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

Applications are a core piece of Foundry's API that almost every developer will have to familiarize themselves with. They allow developers to render HTML windows to display information and provide an interactive UI, from dialogs to character sheets to so much more.

*Official Documentation*

- [ApplicationV2](https://foundryvtt.com/api/v13/classes/foundry.applications.api.ApplicationV2.html)
- [DocumentSheetV2](https://foundryvtt.com/api/v13/classes/foundry.applications.api.DocumentSheetV2.html)

**Legend**

```js
ApplicationV2.DEFAULT_OPTIONS // `.` indicates static method or property
Application#render // `#` indicates instance method or property
```

## Overview

Code for ApplicationV2 and its related classes can be found at `yourFoundryInstallPath/resources/app/client/applications`.

---
## Key Concepts

Here are the core things to know about ApplicationV2, including comparisons to the original Application class.

### ApplicationV2 vs. Application

The ApplicationV2 class and its subclasses were introduced in Foundry V12, with the long-term goal of transitioning all applications to the new framework. The original [Application](/en/development/api/application) class won't be gone until version 16, giving a relatively long deprecation period of 4 full versions (v12 - v15).

- Native light/dark mode support
- Better application window frames
- Architecture supports non-Handlebars rendering engines much more easily
- Support for partial re-rendering in Handlebars
- Better lifecycle events
- Improved a11y handling
- Overall simpler and cleaner to implement

Another major change is there's no more jQuery-by-default in AppV2; all internal functions work exclusively with base javascript DOM manipulation. JQuery is still fully included in Foundry, so developers who prefer it can call `const html = $(this.element)` to get a jQuery representation of the application's rendered HTML.

See [this guide](https://foundryvtt.wiki/en/development/guides/converting-to-appv2) for a detailed walkthrough of converting to AppV2.

### Use of ESModules

Unlike the original Application classes, AppV2 and its subclasses are accessed through nested javascript modules, e.g. `foundry.applications.api.ApplicationV2`. One common trick when dealing with these is the use of [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) to reduce line length and improve comprehensibility, e.g.

```js
// Similar syntax to importing, but note that 
// this is object destructuring rather than an actual import
const { ApplicationV2, HandlebarsApplicationMixin } = foundry.applications.api

class MyHandlebarsApp extends HandlebarsApplicationMixin(ApplicationV2) {}
```

### Which Class to Extend

Unlike the App V1, the base App V2 class handles forms natively, without reliance on a subclass. If you're not writing some kind of Document sheet, in which case you should use the appropriate subclass, everything is going to extend `ApplicationV2`. However, one important thing to know is that you need to use some form of rendering engine, whether that's the Foundry-provided `HandlebarsApplicationMixin` or one created by a community package. 

---
## API Interactions

The following section provides guidance for implementing ApplicationV2 and its related classes

### Basic lifecycle

Once the class has been defined, it can be rendered by calling `new MyApp().render(true)`. Once an application is visible on the screen, it can be refreshed with `myApp.render()` (or more commonly, `this.render()`).

Similarly, `myApp.close()` will remove it from the UI, but the actual class instance will persist until the garbage collector deletes it. This means that if your retain a persistent reference (such as foundry's native handling of document sheets), application properties (like tab state) will persist between cycles of `close()` and `render(true)`.

### BASE_APPLICATION

In ApplicationV2 subclasses, the inheritanceChain determines how far up both `DEFAULT_OPTIONS` and hook calls will go from the currently instantiated class. This is controlled by the static property `BASE_APPLICATION`, which points to a class definition. By default, all ApplicationV2 subclasses inherit all the way up, but subclasses may prefer to limit this.

### DEFAULT_OPTIONS

One property that's important to include is `static DEFAULT_OPTIONS`, which is an instance of  the [ApplicationConfiguration](https://foundryvtt.com/api/v13/interfaces/foundry.applications.types.ApplicationConfiguration.html) type. You can override or extend these options in individual instances of your application by passing an object into the constructor, e.g. `new MyApplication({ position: { width: 600 }})`. 

There's no need to call `super.mergeObject` or anything here; subclasses by default merge their `DEFAULT_OPTIONS` into their parent class, all the way up to through the inheritance chain.

#### Form Handling

Unlike Application, ApplicationV2 has built in form handling with just some configuration changes. These are automatically implemented by DocumentSheetV2, so you only need to make these updates in `DEFAULT_OPTIONS` if you're building a form for a non-document object.

First, set the `tag` property to `"form"` instead of the default `"div"`. This ensures the default `_onSubmitForm` and `_onChangeForm` methods are called.

Second, you must define the sub-properties inside the `form` property - `handler` is the function that actually executes the update, while `submitOnChange` and `closeOnSubmit` are booleans.

To put this all together, including the signature for the handler function, see the snippet below.

```js
class MyApplication extends ApplicationV2 {
  static DEFAULT_OPTIONS = {
  	tag: "form",
    form: {
    	handler: MyApplication.myFormHandler,
      submitOnChange: false,
      closeOnSubmit: false
    }
  }
  
  /**
   * Process form submission for the sheet
   * @this {MyApplication}                      The handler is called with the application as its bound scope
   * @param {SubmitEvent} event                   The originating form submission event
   * @param {HTMLFormElement} form                The form element that was submitted
   * @param {FormDataExtended} formData           Processed data for the submitted form
   * @returns {Promise<void>}
   */
  static async myFormHandler(event, form, formData) {
  	// Do things with the returned FormData
  }
}
```

#### Actions

The `actions` object is a Record of functions that automatically get bound as `click` listeners to any element that has the appropriate `data-action` in its attributes. Importantly, these should be *static* functions, but their `this` value will still point to the specific class instance.

```js
// for proper class definition you'd need to use HandlebarsApplicationMixin
// but it's not used here because these are properties of the base ApplicationV2 class
class MyApplication extends ApplicationV2 {
  static DEFAULT_OPTIONS = {
  	actions: {
    	myAction: MyApplication.myAction
    }
  }
  
  /**
   * @param {PointerEvent} event - The originating click event
   * @param {HTMLElement} target - the capturing HTML element which defined a [data-action]
   */
  static myAction(event, target) {
  	console.log(this) // logs the specific application class instance
  }
}
```

This could pair with the following HTML to add the click event. You can use whatever tags you want, but `<a>` tags and `<button>` tags usually require the least amount of additional CSS.

```html
<a data-action="myAction">Using a link for inline text</a>
```

For those used to Application V1, this largely replaces the role `activateListeners` played. If you have other event listeners to add, you can use `_onRender`, which is explored in the "Specific Use Cases" section.

#### Header Buttons

ApplicationV2 provides a dropdown of header buttons, an alternative to the strictly in-line implementation from Application that caused problems when many different packages wanted to have header buttons. Instantiating these buttons involves the `window` object and its `controls` property, which is an array of [ApplicationHeaderControlsEntry](https://foundryvtt.com/api/v13/interfaces/foundry.applications.types.ApplicationHeaderControlsEntry.html).

```js
// for proper class definition you'd need to use HandlebarsApplicationMixin
// but it's not used here because these are properties of the base ApplicationV2 class
class MyApplication extends ApplicationV2 {
	static DEFAULT_OPTIONS = {
  	actions: {
    	myAction: MyApplication.myAction
    }
    window: {
      controls: [
    		{
      	  // font awesome icon
       	  icon: 'fa-solid fa-triangle-exclamation',
          // string that will be run through localization
          label: "Bar",
          // string that MUST match one of your `actions`
          action: "myAction",
        },
      ]
    }
  }
}
```


### HandlebarsApplicationMixin

- [MDN docs on Mixins](https://developer.mozilla.org/en-US/docs/Glossary/Mixin)
- [Handlebars Helpers](/en/development/api/helpers)

Unless you are using an external rendering package, every AppV2 instance is going to extend `HandlebarsApplicationMixin`. This function returns a `HandlebarsApplication` class which fully implements the rendering logic required by ApplicationV2.

#### PARTS

The core of HandlebarsApplication is the `static PARTS` property, which is a Record consisting of objects with the following structure:

```jsdoc
/**
 * @typedef {Object} HandlebarsTemplatePart
 * @property {string} template                      The template entry-point for the part
 * @property {string} [id]                          A CSS id to assign to the top-level element of the rendered part.
 *                                                  This id string is automatically prefixed by the application id.
 * @property {string[]} [classes]                   An array of CSS classes to apply to the top-level element of the
 *                                                  rendered part.
 * @property {string[]} [templates]                 An array of templates that are required to render the part.
 *                                                  If omitted, only the entry-point is inferred as required.
 * @property {string[]} [scrollable]                An array of selectors within this part whose scroll positions should
 *                                                  be persisted during a re-render operation. A blank string is used
 *                                                  to denote that the root level of the part is scrollable.
 * @property {Record<string, ApplicationFormConfiguration>} [forms]  A registry of forms selectors and submission handlers.
 */
```

Replicating a v1 Application is fairly simple - just pass a single part!
```js
static PARTS = {
  form: {
    template: "modules/my-module/templates/my-app.hbs"
  }
}
```

However, you may want to have an application that leverages the flexibility of multiple parts. When using multiple parts, it's important to know the following

- Each part must return a single HTML element - that is, only one pair of top-level tags.
- The parts are concatenated in the order of the static property
- All parts are encapsulated by the top-level tag set in `options.tag` - this is a `div` by default in `ApplicationV2`, but `DocumentSheetV2` changes this to `form`. 

Broadly speaking, this means the most straightforward way to structure a multi-part application is to lead with a `header` part, then optionally a distinct `tabs` part, then finally one part for each of your tabs. You can even add a footer at the end!

##### Only Displaying Some Parts

One way to leverage parts is to only show some of them sometimes. The correct place to do this is by extending `_configureRenderOptions`; you *do* want to call `super` here, as some important things happen upstream.

```js
// This isn't DocumentSheet specific, but it's the most common place you'll want this
class MyApplication extends HandlebarsApplicationMixin(DocumentSheetV2) {
  static PARTS = {
  	header: { template: '' },
  	tabs: { template: '' },
  	description: { template: '' },
    foo: { template: '' },
    bar: { template: '' },
  }
  
  /** @override */
  _configureRenderOptions(options) {
    // This fills in `options.parts` with an array of ALL part keys by default
    // So we need to call `super` first
  	super._configureRenderOptions(options);
    // Completely overriding the parts
    options.parts = ['header', 'tabs', 'description']
    // Don't show the other tabs if only limited view
    if (this.document.limited) return;
    // Keep in mind that the order of `parts` *does* matter
    // So you may need to use array manipulation
    switch (this.document.type) {
      case 'typeA':
        options.parts.push('foo')
        break;
      case 'typeB':
        options.parts.push('bar')
        break;
    }
  }

}
```

#### \_prepareContext

The variable-based rendering of handlebars is handled by `_prepareContext`, an asynchronous function that returns a `context` object with whatever data gets fed into the `template`. It has a single argument, `options`, which is the options object passed to the original `render` call, but this can usually be ignored.

In Application V1 terms, this is functionally equivalent to its `getData` call, with the only functional change that this is *always* asynchronous.

Inside your handlebars template, you'll *only* have access to the data setup in `_prepareContext`, so if you need to include information such as `CONFIG.MYSYSTEM` you'll want to include a pointer to it in the returned object.

> **Note**
> 
> The disconnect between the data provided to the template via `_prepareContext` and the way that `DocumentSheetV2` stores data to the document via the `name=""` field can cause some confusion. It's common practice to store the document's system data in a system key in the context, which means that you can usually do `value="{{system.attribute.value}}"` and `name="system.attribute.value"` in an actor/item sheet and stuff works.
>
> However, under the hood, the `{{}}` is pulling stuff from the context object that the `_prepareContext` returns while the `name=""` is storing things based on the data path in the document itself. This means that there are situations where they won't actually line up, because they're not fundamentally pointing at the same thing at the end of the day, they just happen to often line up.
{.is-info}

##### \_preparePartContext

The `HandlebarsApplicationMixin` provides an additional method for handling context that can be useful, especially in conjunction with only rendering some of the parts so only processes that are actually necessary happen. You can cleanly override this method and ignore its addition of `partId` to the context.

```js
    /**
     * Prepare context that is specific to only a single rendered part.
     *
     * It is recommended to augment or mutate the shared context so that downstream methods like _onRender have
     * visibility into the data that was used for rendering. It is acceptable to return a different context object
     * rather than mutating the shared context at the expense of this transparency.
     *
     * @param {string} partId                         The part being rendered
     * @param {ApplicationRenderContext} context      Shared context provided by _prepareContext
     * @returns {Promise<ApplicationRenderContext>}   Context data for a specific part
     * @protected
     */
    async _preparePartContext(partId, context) {
      context.partId = `${this.id}-${partId}`;
      return context;
    }
```

However, a common pattern is to use a [switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) statement on the `partId` argument and then handle part-specific logic in the cases. This can allow you to both contextually override properties (tab info) or only do work if it's necessary (such as a limited sheet that doesn't render actor inventory).

#### `templates`

The `templates` property of a part is used by `HandlebarsApplication#_preFirstRender`; the declared parts are all added to a Set (to filter out duplicates) and then transformed into an array to be passed to `loadTemplates`. In v12, your primary `template` *must* be included in this array if you're using it. 

Two important caveats to using this property
- If you are otherwise overriding `_preFirstRender`, you must call `await super._preFirstRender(context, options);` to preserve this handling
- The `templates` property only accepts a string array, so there's no way to reference these partials as a key-value record for more succinct references in the handlebars. You need to externally call `loadTemplates` if you wish to register templates with an ID.

---
## Specific Use Cases

Below are some specific tricks and techniques to use with ApplicationV2 and its subclasses.

### Adding Event Listeners

The `actions` field, explored above, is usually sufficient for most sheet listeners - however, sometimes you need other, non-click listeners. For example, many systems like to display physical item's quantity as an editable field on the actor sheet, which isn't natively supported by Foundry's form submission and data architecture. The best place to add these is the `_onRender` function.

```js
class MyActorSheet extends HandlebarsApplicationMixin(ActorSheetV2) {
  /**
   * Actions performed after any render of the Application.
   * Post-render steps are not awaited by the render process.
   * @param {ApplicationRenderContext} context      Prepared context data
   * @param {RenderOptions} options                 Provided render options
   * @protected
   */
	_onRender(context, options) {
		// Inputs with class `item-quantity`
    const itemQuantities = this.element.querySelectorAll('.item-quantity')
    for (const input of itemQuantities) {
      // keep in mind that if your callback is a named function instead of an arrow function expression
      // you'll need to use `bind(this)` to maintain context
      input.addEventListener("change", (e) => {
        e.preventDefault();
        e.stopImmediatePropagation();
        const newQuantity = e.currentTarget.value
        // assuming the item's ID is in the input's `data-item-id` attribute
        const itemId = e.currentTarget.dataset.itemId
        const item = this.actor.items.get(itemId)
        // the following is asynchronous and assumes the quantity is in the path `system.quantity`
        item.update({ system: { quantity: newQuantity }});
      })
    }
  }
}
```

There are much less verbose implementations of the above code - the whole thing is theoretically doable in a single line - but for clarity this example does each piece step-by-step.

### Tabs

#### Tabs in V12

ApplicationV2 in V12 includes partial support for tabs with the `changeTab` method and the `tabGroups` record. However, `HandlebarsApplicationMixin` will not automatically re-apply the relevant class adjustments on re-render automatically, meaning that developers are responsible for maintaining that status themselves.

**Tab Navigation**. There's a handy Foundry-provided template for tabs at `templates/generic/tab-navigation.hbs` you may want to use. It expects an array or record of `ApplicationTab` supplied in a field named `tabs`. A record is preferable to an array because it can be more easily used in tab display. (*This is merely a typedef, you must actually construct the object yourself*)

```js
/**
 * @typedef ApplicationTab
 * @property {string} id         The ID of the tab. Unique per group.
 * @property {string} group      The group this tab belongs to.
 * @property {string} icon       An icon to prepend to the tab
 * @property {string} label      Display text, will be run through `game.i18n.localize`
 * @property {boolean} active    If this is the active tab, set with `this.tabGroups[group] === id`
 * @property {string} cssClass   "active" or "" based on the above boolean
 */
```

**Tab Display**. Each element representing one of your tabs *must* have the following attributes
- `data-group`, for the tab's group
- `data-tab`, for the tab's ID
- You'll want to include `cssClass` within your tab's `class` property to track `active` or not.

If each of your tabs is a `part`, then you can store your `tabs` as `Record<partId, ApplicationTab>`. Then, in `_preparePartContext`, set `context.tab = context.tabs[partId]`. A simple example of the target handlebars:

```handlebars
<section class="tab {{tab.cssClass}}" data-group="primary" data-tab="foo">
	{{! stuff }}
</section>
```

#### Tabs in V13

The V13 implementation of ApplicationV2 has better tab support. See [Tabs in AppV2](/en/development/guides/Tabs-and-Templates/Tabs-in-AppV2) for additional information.

### Text Enrichment

API Reference
- [TextEditor.enrichHTML](https://foundryvtt.com/api/v13/classes/foundry.applications.ux.TextEditor.html#enrichhtml)
- [HandlebarsHelpers.editor](https://foundryvtt.com/api/v13/functions/foundry.applications.handlebars.editor.html)
- [EnrichmentOptions](https://foundryvtt.com/api/v13/interfaces/foundry.EnrichmentOptions.html)

Text enrichment is the process of replacing and augmenting input text like `[[/roll 1d6]]` in the final rendered HTML. It's most commonly used with the `{{editor}}` Handlebars helper.

Text enrichment is an *asynchronous* process, which means it needs to happen inside `_prepareContext` before template rendering. The first argument is a path to the raw html string to be enriched, the second argument implements EnrichmentOptions.
```js
// Exact process may differ for non-handlebars mixins
class MyApplication extends HandlebarsApplicationMixin(ApplicationV2) {
  async _prepareContext() {
    const context = {};
    
    // Be mindful of mutating other objects in memory when you enrich
    context.enrichedDescription = await TextEditor.enrichHTML(
      this.document.system.description, 
      { 
        // Only show secret blocks to owner
        secrets: this.document.isOwner,
        // For Actors and Items
        rollData: this.document.getRollData
      }
    );
    
    return context;
  }
}
``` 

The corresponding handlebars helper, as text enrichment is typically paired. The `target` property should match the source of what was enriched, in this case the assumption is that `system.description` of the document was the field run through enrichment. The `editable` value here is inherited from `super.getData`, which is why it's not explicitly declared in `context` above.

```handlebars
{{editor enrichedDescription target="system.description" editable=editable button=true engine="prosemirror" collaborate=false}}
```

If you're just trying to display enriched text without providing an editor input - such as an item's description in an actor sheet - triple braces will render a string as raw HTML.

```handlebars
{{{enrichedDescription}}}
```


### DragDrop

API Reference

- [DragDrop](https://foundryvtt.com/api/v13/classes/foundry.applications.ux.DragDrop.html)
- [DragDropConfiguration](https://foundryvtt.com/api/v13/interfaces/foundry.DragDropConfiguration.html)

The `DragDrop` helper class integrates dragging and dropping across different applications in the Foundry interface. The most common use is dragging and dropping documents from one location to another.

ApplicationV2 does not include an implementation of this handling, but the helper class still works - you just have to write it yourself. The following implementation uses HandlebarsApplicationMixin, but this should work with other rendering engines.

**Step 1: Initialize the DragDrop.** To do this, we need to override the constructor so the DragDrop class is instantiated as part of the application class.

```js
class MyAppV2 extends HandlebarsApplicationMixin(ApplicationV2) {
  constructor(options = {}) {
    super(options);
    this.#dragDrop = this.#createDragDropHandlers();
  }
  
  /**
   * Create drag-and-drop workflow handlers for this Application
   * @returns {DragDrop[]}     An array of DragDrop handlers
   * @private
   */
  #createDragDropHandlers() {
    return this.options.dragDrop.map((d) => {
      d.permissions = {
        dragstart: this._canDragStart.bind(this),
        drop: this._canDragDrop.bind(this),
      };
      d.callbacks = {
        dragstart: this._onDragStart.bind(this),
        dragover: this._onDragOver.bind(this),
        drop: this._onDrop.bind(this),
      };
      return new DragDrop(d);
    });
  }
  
  #dragDrop;
  
  // Optional: Add getter to access the private property
  
  /**
   * Returns an array of DragDrop instances
   * @type {DragDrop[]}
   */
  get dragDrop() {
    return this.#dragDrop;
  }

}
```

**Step 2: Define `options.dragDrop`.** This implementation mimics the [Application](/en/development/api/application) implementation by using `options.dragDrop` to define a class's bound drag handlers. The options object is compiled from the applications `DEFAULT_OPTIONS`, like the following:

```js
class MyAppV2 extends HandlebarsApplicationMixin(ApplicationV2) {
  static DEFAULT_OPTIONS = {
    dragDrop: [{ dragSelector: '[data-drag]', dropSelector: null }],
  ]
}
```

**Step 3:** Define the handlebars templating. Our actual draggable objects need to have the `data-drag` property, but the actual value of the property doesn't matter unless you want it to.
```handlebars
<ol class="foo">
	{{#each someArray}}
  	<li data-drag="true">{{this.label}}</li>
  {{/each}}
</ol>
```

**Step 4: Bind the DragDrop listeners.** In AppV2, event listeners for non-click events are handled inside `_onRender` (Click events should be implemented as Actions, see above for more details).

```js
class MyAppV2 extends HandlebarsApplicationMixin(ApplicationV2) {
  /**
   * Actions performed after any render of the Application.
   * Post-render steps are not awaited by the render process.
   * @param {ApplicationRenderContext} context      Prepared context data
   * @param {RenderOptions} options                 Provided render options
   * @protected
   */
  _onRender(context, options) {
    this.#dragDrop.forEach((d) => d.bind(this.element));
  }
}
```

**Step 5: Define callbacks.** Back in step 1, we defined a number of callbacks during `#createDragDropHandlers`. Now we just need to implement them!

```js
class MyAppV2 extends HandlebarsApplicationMixin(ApplicationV2) {

  /**
   * Define whether a user is able to begin a dragstart workflow for a given drag selector
   * @param {string} selector       The candidate HTML selector for dragging
   * @returns {boolean}             Can the current user drag this selector?
   * @protected
   */
  _canDragStart(selector) {
    // game.user fetches the current user
    return this.isEditable;
  }
  
  
  /**
   * Define whether a user is able to conclude a drag-and-drop workflow for a given drop selector
   * @param {string} selector       The candidate HTML selector for the drop target
   * @returns {boolean}             Can the current user drop on this selector?
   * @protected
   */
  _canDragDrop(selector) {
    // game.user fetches the current user
    return this.isEditable;
  }
  
  
  /**
   * Callback actions which occur at the beginning of a drag start workflow.
   * @param {DragEvent} event       The originating DragEvent
   * @protected
   */
  _onDragStart(event) {
    const el = event.currentTarget;
    if ('link' in event.target.dataset) return;

    // Extract the data you need
    let dragData = null;

    if (!dragData) return;

    // Set data transfer
    event.dataTransfer.setData('text/plain', JSON.stringify(dragData));
  }
  
  
  /**
   * Callback actions which occur when a dragged element is over a drop target.
   * @param {DragEvent} event       The originating DragEvent
   * @protected
   */
  _onDragOver(event) {}
  
  
  /**
   * Callback actions which occur when a dragged element is dropped on a target.
   * @param {DragEvent} event       The originating DragEvent
   * @protected
   */
  async _onDrop(event) {
    const data = TextEditor.getDragEventData(event);
    
    // Handle different data types
    switch (data.type) {
        // write your cases
    }
  }
}
```

There you have it, a basic implementation of DragDrop in ApplicationV2!

### SearchFilter

API Reference
- [SearchFilter](https://foundryvtt.com/api/v13/classes/foundry.applications.ux.SearchFilter.html)
- [SearchFilterConfiguration](https://foundryvtt.com/api/v13/interfaces/foundry.SearchFilterConfiguration.html)

The `SearchFilter` helper class connects a text input box to filtering a list of results. It suppresses other events that might fire on the same input, instead activating the bound callback to modify the targeted HTML.

ApplicationV2 does not implement its own SearchFilter support so you'll have to initialize it in the constructor or as a class property. Then you'll need to call `bind(this.element)` in `_onRender`. The callback parameter, while only referred to as the base `Function` in `SearchFilterConfiguration`, matches the signature of `Application#_onSearchFilter`, provided below.

```js
  /**
   * Handle changes to search filtering controllers which are bound to the Application
   * @param {KeyboardEvent} event   The key-up event from keyboard input
   * @param {string} query          The raw string input to the search field
   * @param {RegExp} rgx            The regular expression to test against
   * @param {HTMLElement} html      The HTML element which should be filtered
   * @protected
   */
  _onSearchFilter(event, query, rgx, html) {}
```

The body of this function must do the actual DOM manipulation; `rgx.test` is probably helpful, as are operations on the provided `html` element to mark elements as `display: hidden` or other ways of removing them from display in the DOM.

### Registering Document Sheets

API Reference
- [DocumentSheetConfig.registerSheet](https://foundryvtt.com/api/v13/classes/foundry.applications.apps.DocumentSheetConfig.html#registersheet)

When you define a new document sheet, you can register it in the `init` hook so it's configurable. 

```js

// You need to separately define your DocumentSheetV2 subclass
class MyActorSheet extends HandlebarsApplicationMixin(ActorSheetV2) {}

Hooks.once("init", () => {
  // The the `config` object in the fourth argument is entirely optional, as are its properties
  DocumentSheetConfig.registerSheet(Actor, "package-id", MyActorSheet, {
    // Any string here will be localized
  	label: "MyPackage.MyDocumentSheet.Label",
    // If the sheet is only usable for some values of the `type` field
    types: ["character, npc"],
    // Generally useful, defaults to false
    makeDefault: true,
    // There are other properties that are rarely needed. See the linked docs for more.
  })
  // `Actors.registerSheet` is semantically equivalent to passing Actor as the first argument
  // This works for all world collections, e.g. Items
  Actors.registerSheet("package-id", MyActorSheet, {})
}
```

### Easy form submission buttons

1) Add the following to your `static PARTS`:
```js
footer: {
	template: "templates/generic/form-footer.hbs",
}
```

2) Add the following to the return value of `_prepareContext`:
```js
buttons: [
	{ type: "submit", icon: "fa-solid fa-save", label: "SETTINGS.Save" },
	// { type: "reset", action: "reset", icon: "fa-solid fa-undo", label: "SETTINGS.Reset" },
]
```
3) Move all your buttons to the buttons array above.
4) Be sure the HTML template for your form declared in `static PARTS` doesn't contain a HTML `<form>` (change them to `<div>`).	Otherwise, your `formData` argument on the submit method will be empty.

### Non-Handlebars Rendering Frameworks

The following are community implementations of non-handlebars rendering frameworks.

- [Vue](https://github.com/mouse0270/fvtt-vue) by Mouse0270
- [Svelte](https://github.com/ForgemasterModules/svelte-fvtt) by ForgemasterModules

---
## Troubleshooting

Here are some common problems people run into with applications in Foundry.

### Using a button triggers full web page refresh

By default, a button will trigger the `submit` process of whatever form it is in. AppV2 will attempt to capture this if you have form handling configured with `tag: "form"` and a registered `handler` in DEFAULT_OPTIONS, however if that is not the case then the default browser behavior is to submit the webpage - causing a full refresh.

To fix this, add `type="button"` to the attributes of any button you don't want to trigger a submission event.

### Arrays in Forms

Foundry only natively handles arrays of primitives in its forms - that is, an array of strings, numbers, or booleans. If you have an array of objects, you have two options
- Override `DocumentSheetV2#_prepareSubmitData`, calling `super` then modifying the `data` it returns. If you're not subclassing DocumentSheetV2, your own form handler is fully in charge of handling the data.
- Implement a [DataModel](/en/development/api/DataModel) for whatever you're returning, allowing the casting in `ArrayField` to handle the transformation.

### Debugging CSS

The following script macro will toggle the color scheme between light and dark.

```js
const uiConfig = game.settings.get('core', 'uiConfig');
const color = uiConfig.colorScheme.applications;
const newColor = color === 'light' ? 'dark' : 'light';
uiConfig.colorScheme.applications = newColor;
uiConfig.colorScheme.interface = newColor;
await game.settings.set('core', 'uiConfig', uiConfig)
```