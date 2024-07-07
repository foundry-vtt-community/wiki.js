---
title: Application
description: The standard application window that is rendered for a large variety of UI elements in Foundry VTT.
published: true
date: 2024-07-07T09:23:43.700Z
tags: documentation
editor: markdown
dateCreated: 2024-02-13T19:36:31.269Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Applications are a core piece of Foundry's API that almost every developer will have to familiarize themselves with. As of V12, the Application class has begun a (long) transition out of use - instead, [ApplicationV2](/en/development/api/applicationv2) should be preferred for new applications. See also [this guide](/en/development/guides/converting-to-appv2) for help changing over.

The [Dialog](/en/development/api/dialog) subclass has its own page detailing its specific options because its configuration is so specific. It has an AppV2 equivalent in [DialogV2](/en/development/api/dialogv2).

*Official Documentation*
- [Application](https://foundryvtt.com/api/classes/client.Application.html)
- [FormApplication](https://foundryvtt.com/api/classes/client.FormApplication.html)
- [DocumentSheet](https://foundryvtt.com/api/classes/client.DocumentSheet.html)
- [Built-in Helpers](https://handlebarsjs.com/guide/builtin-helpers.html)
- [Foundry Handlebar Helpers](https://foundryvtt.com/api/classes/client.HandlebarsHelpers.html)

**Legend**
```js
Application.defaultOptions // `.` indicates static method or property
Application#getData // `#` indicates instance method or property
```

## Overview

The Application class is the basic building block of Foundry's UI (which, combined with the canvas that represents a scene, makes up everything you see in Foundry). At its fundamental level, it's an HTML element that is positioned somewhere in the browser window. 

`Application` and its subclasses are stored in `yourFoundryInstall\resources\app\client\apps`

---
## Key Concepts

There are two general types of Applications, popout applications and non-popout applications. Popout applications will have a header bar and be movable (all of the pop-up windows for configuration, actor/item sheets, and so on) while non-popout applications are generally fixed UI elements (such as the sidebar, the macro hotbar, the scene navigation, and so on).

Applications are stored in the ui global namespace. So, you'll see stuff like the right sidebar tabs in `ui.sidebar`, the floating popout windows in `ui.windows`, the layer controls on the left in `ui.controls`, and so on.

### Which class to extend

`Application` subclasses are not mandatory but are intended to make things easier for you, as a developer. With the exception of `Dialog`, they're rarely directly invoked; instead, you declare a new class that *extends* one of these (sub-)classes.

- Application: If truly anything beyond minimal window rendering would be an imposition
- FormApplication: General form handling
- DocumentSheet: If your application has an attached document and should have header buttons for thinks like grabbing the document ID/UUID as well as a configurable sheet.

---
## API Interactions
The core functionality of Applications is provided by `render`, `getData`, and `activateListeners`.

### `Application#render`
The `render` method triggers rendering of the application. If called with no arguments, as in `app.render()`, it will re-render the application if it alredy exists; if called with `true` as an argument, as in `app.render(true)` it will force the app to render even if it hasn't been rendered yet. This lets you distinguish between a "pop it up and show this thing" rendering and a "refresh it if it's already there" rendering.

At an extremely simplified high-level, rendering does these important things:

1. Use `this.getData` to get the prepared context for the Handlebars template
1. Use the `this.template` getter to determine what Handlebars template file to use
1. Render the template with the context to make the HTML for the application
1. Inject the HTML into the proper place in the webpage as a whole
1. Use `this.activateListeners` to bind any needed JS listeners to the HTML of the application
1. Call the `render` hook on the Application subclass itself and all parent classes (so, rendering the `MyActorSheet` Application will call `renderMyActorSheet`, `renderActorSheet`, `renderDocumentSheet`, `renderFormApplication`, and `renderApplication`)

### `Application#getData`
The `getData` method is what pulls together whatever data the template the Handlebars template will need for rendering and compiling them into an object.

The context object that the Handlebars template gets has no relation to the data in Foundry itself. Anything you want the Handlebars template to have to use for rendering needs to be defined in the object you're passing it.

Because of the disconnect between Foundry's data itself and the context object being passed to Handlebars, you're free to build whatever context data you need to in the `getData`. Lay the data out in whatever way is ideal for the template being rendered, without needing to worry about how it relates to Foundry's data as a whole.

> **Note**
> 
> The disconnect between the data provided to the template via `getData` and the way that `DocumentSheet` stores data to the document via the `name=""` field can cause some confusion. It's common practice to store the document's system data in a system key in the context, which means that you can usually do `value="{{system.attribute.value}}"` and `name="system.attribute.value"` in an actor/item sheet and stuff works.
>
> However, under the hood, the `{{}}` is pulling stuff from the context object that the `getData` returns while the `name=""` is storing things based on the data path in the document itself. This means that there are situations where they won't actually line up, because they're not fundamentally pointing at the same thing at the end of the day, they just happen to often line up.
{.is-info}

### `Application#activateListeners`
The `activateListeners` method handles binding JS listeners to the HTML of the application. It takes one argument, which is the jQuery HTML object, and you can bind any JS interaction listeners you need in it.

This is where any click handlers or other interactivity should be implemented.

### ContextMenu

API Reference
- [ContextMenu](https://foundryvtt.com/api/classes/client.ContextMenu.html)
- [ContextMenuEntry](https://foundryvtt.com/api/interfaces/client.ContextMenuEntry.html)

The `ContextMenu` provides a standard UI for context menus in FoundryVTT applications, and are triggered by right clicking. The normal way to create them is with `ContextMenu.create` in your application's `activateListeners` method, providing a name, icon, and a callback.

The `name` option will be localized if its a locatilization variable.

```js
  activateListeners(html) {
  	super.activateListeners(html)
  	// other listener handling
    ContextMenu.create(this, html, '.css-selector-class', [
      {
      	name: "SIDEBAR.Edit",
        icon: '<i class="fa-solid fa-trash"></i>',
        callback: this.myFunction.bind(this),
        condition: this.myCondition.bind(this)
      }
    ]);
  }

	/**
   *	@param {JQuery} jq - The element that the ContextMenu was attached to
   */
  myFunction(jq) {
  	// does something
  }

	/**
   *	@param {JQuery} jq - The element that the ContextMenu was attached to
   *  @returns {boolean} Whether or not to render this option
   */
  myCondition(jq) {
  	// You don't have to use the provided JQuery object
    // You could just have it be conditional on say `this.isEditable`
  }
```

Note that the `icon` property uses the internal string for a [FontAwesome](https://fontawesome.com/search?o=r&m=free) icon, rather than the full html element.

### DragDrop

API Reference
- [DragDrop](https://foundryvtt.com/api/classes/client.DragDrop.html)
- [DragDropConfiguration](https://foundryvtt.com/api/interfaces/client.DragDropConfiguration.html)

The `DragDrop` helper class integrates dragging and dropping across different applications in the Foundry interface. The most common use is dragging and dropping documents from one location to another.

The Application class and its subclasses have native support with the `dragDrop` option — usually in the `defaultOptions` static getter — that is an array of `DragDropConfiguration`. If you use this option you only need to provide the `dragSelector` and `dropSelector` properties, as the native handling automatically binds the following functions:
- *Permissions*
  - `_canDragStart` (default: `game.user.isGM`)
  - `_canDragDrop` (default: `game.user.isGM`)
- *Callbacks*
  - `_onDragStart`
  - `_onDragOver`
  - `_onDrop`

The most common interaction with drag events is on subclasses of ActorSheets, where there's already some native handling; for example, the `dragDrop` option defaults to `[{dragSelector: ".item-list .item", dropSelector: null}]`. It also replaces the default permission controls with `this.isEditable`.

The `ActorSheet#_onDragStart` function natively supports dragging from `li.item` elements with `data-item-id` and `data-effect-id` properties, but only checks if those ids are directly in the embedded collections of the actor; there is no native support for effects embedded in items, for systems that have `CONFIG.ActiveEffect.legacyTransferral = false`.

`ActorSheet#_onDrop` is more flexible, offering protected functions for `_onDropActiveEffect`, `_onDropActor`, `_onDropItem`, and `_onDropFolder`. These functions serve as sensible defaults, but can be overridden as needed (e.g. `_onDropActor` simply returns `false` if the current user is not an owner of the actor; you can extend it to do whatever linkages between actors are necessary).

### FormDataExtended

API Reference
- [FormDataExtended](https://foundryvtt.com/api/classes/client.FormDataExtended.html)

When working with a `FormApplication`, you can cast the output of `input` elements by adding a `data-dtype` property. In modern foundry this is somewhat redundant if you're using a [Data Model](/en/development/api/DataModel), but it can still be useful for more advanced techniques. 

Valid choices include:

- "String"
- "Boolean"
- "Number"
- "JSON"

It will also generally check for `window[dataType] instanceof Function`, so you can globally define a type by globally defining a function used to cast that type.

### SearchFilter

API Reference
- [SearchFilter](https://foundryvtt.com/api/classes/client.SearchFilter.html)
- [SearchFilterConfiguration](https://foundryvtt.com/api/interfaces/client.SearchFilterConfiguration.html)

The `SearchFilter` helper class connects a text input box to filtering a list of results. It suppresses other events that might fire on the same input, instead activating the bound callback to modify the targeted HTML.

The Application class and its subclasses have native support with the `filters` option — usually in the `defaultOptions` static getter — that is an array of `SearchFilterConfiguration`. If you use this option you only need to provide the `inputSelector` and `contentSelector` properties, as the native handling automatically binds [`_onSearchFilter`](https://foundryvtt.com/api/classes/client.Application.html#_onSearchFilter) to the callback. The implementation of this function is entirely up to you; the implementation in `PackageConfiguration` is probably the most approachable. The inherited jsdoc is provided for clarity.

```js
	// defaultOptions includes the following configuration for the search field:
  // filters: [{inputSelector: 'input[name="filter"]', contentSelector: ".categories"}],

  /**
   * Handle changes to search filtering controllers which are bound to the Application
   * @param {KeyboardEvent} event   The key-up event from keyboard input
   * @param {string} query          The raw string input to the search field
   * @param {RegExp} rgx            The regular expression to test against
   * @param {HTMLElement} html      The HTML element which should be filtered
   * @protected
   */
  _onSearchFilter(event, query, rgx, html) {
    const visibleCategories = new Set();

    // Hide entries
    for ( const entry of html.querySelectorAll(".form-group") ) {
      if ( !query ) {
        entry.classList.remove("hidden");
        continue;
      }
      const label = entry.querySelector("label")?.textContent;
      const notes = entry.querySelector(".notes")?.textContent;
      const match = (label && rgx.test(SearchFilter.cleanQuery(label)))
        || (notes && rgx.test(SearchFilter.cleanQuery(notes)));
      entry.classList.toggle("hidden", !match);
      if ( match ) visibleCategories.add(entry.parentElement.dataset.category);
    }

    // Hide categories which have no visible children
    for ( const category of html.querySelectorAll(".category") ) {
      category.classList.toggle("hidden", query && !visibleCategories.has(category.dataset.category));
    }
  }
```

### Tabs

API Reference
- [Tabs](https://foundryvtt.com/api/classes/client.Tabs.html)
- [TabsConfiguration](https://foundryvtt.com/api/interfaces/client.TabsConfiguration.html)

The base Foundry application provides handling for tabs. This requires handling in both the Application's `defaultOptions` as well as your .hbs file.

In the application class, you need to provide a `tabs` property structured as an array of TabsConfiguration objects. For example:
```js
MyApplicationClass extends Application {
  /** @override */
  static get defaultOptions() {
    foundry.utils.mergeObject(super.defaultOptions, {
      tabs: [
       {
          group: 'primary-tabs',
          navSelector: '.tabs',
          contentSelector: '.content',
          initial: 'tab1',
        },
      ]
    }
  }
}
```

Then in your template `.hbs` file:

```handlebars
<nav class="tabs" data-group="primary-tabs">
  <a class="item" data-tab="tab1" data-group="primary-tabs">Tab 1</li>
  <a class="item" data-tab="tab2" data-group="primary-tabs">Tab 2</li>
</nav>

<section class="content">
  <div class="tab" data-tab="tab1" data-group="primary-tabs">Content 1</div>
  <div class="tab" data-tab="tab2" data-group="primary-tabs">Content 2</div>
</section>
```

The `group` property is optional if you only intend to have one set of tabs, but if you plan to have two or more you must include the `group` property on each TabsConfiguration object.

### Header Buttons

The `_getHeaderButtons` method is responsible for managing the buttons in the header of the application. By default, all applications have a `close` button, and DocumentSheet adds buttons to import from compendium and configure the choice of sheet. Calling `const buttons = super._getHeaderButtons` is important because that's how you have the inherited buttons. By convention, subclasses use `unshift` to modify the array rather than `push` to ensure that the `close` button remains the right-most. The buttons array is typed as an array of [ApplicationHeaderButton](https://foundryvtt.com/api/modules/hookEvents.html#ApplicationHeaderButton). The `onClick` function can be asynchronous or not, as your application needs, but the `_getHeaderButtons` method itself is strictly synchronous.

A simple example:

```js
class MyApplication extends Application {
	_getHeaderButtons() {
  	const buttons = super._getHeaderButtons();
    // You can use conditional logic here like any other function
    buttons.unshift({
      // there's no auto-localization so you probably want to use game.i18n.localize
    	label: "Foo",
      // class applied to the button for styling
      class: 'foo',
      // Free fontawesome icons
      icon: 'fa-solid fa-triangle-exclamation',
      // the callback when clicked
      onclick: () => ui.notifications.info("Foo!")
    })
    return buttons;
  }
}
```

Modules can inject header buttons with the [getApplicationHeaderButtons](https://foundryvtt.com/api/modules/hookEvents.html#getApplicationHeaderButtons) hook; like render hooks, this is called for each class in an application's inheritance chain (e.g. both `getApplicationHeaderButtons` and `getDocumentSheetHeaderButtons` are called)

```js
// app is the application subclass that's calling the hook
Hooks.on("getApplicationHeaderButtons", (app, buttons) => {
  // You can use conditional logic here like any other function
  buttons.unshift({
    label: "Bar",
    class: 'bar',
    icon: 'fa-solid fa-triangle-exclamation',
    onclick: () => ui.notifications.info("Bar!")
  })
  // You're mutating the `buttons` array so no return here
}}

```

> **Use Header Buttons Sparingly**
> By default, the Application class does not have any protections for large numbers of header buttons, which can crowd out the application's title and cause other UI issues.
{.is-warning}


---
## Specific Use Cases

Here are some tips and tricks when working with Applications.

### Automatic re-rendering on Document updates
Document classes each have an `apps` property which stores an object of Application references. This object is iterated through and each app is re-rendered whenever the Document changes, so that any open windows will reflect the new state of the document.

DocumentSheet automatically adds itself to the apps when it initially renders, which lets the document easily automatically trigger re-rendering the sheet to reflect new changes in realtime. However, applications can add themselves to a document's apps as-needed, which can be used for any non-DocumentSheet Application subclasses which need to display data about a document and keep the display properly updated.

### Registering Document Sheets

API Reference
- [DocumentSheetConfig.registerSheet](https://foundryvtt.com/api/classes/client.DocumentSheetConfig.html#registerSheet)

When you define a new document sheet, you can register it in the `init` hook so it's configurable. 

```js
// You need to separately define your DocumentSheet subclass
class MyActorSheet extends ActorSheet {}

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

### Text Enrichment

API Reference
- [TextEditor.enrichHTML](https://foundryvtt.com/api/classes/client.TextEditor.html#enrichHTML)
- [HandlebarsHelpers.editor](https://foundryvtt.com/api/classes/client.HandlebarsHelpers.html#editor)
- [EnrichmentOptions](https://foundryvtt.com/api/interfaces/client.EnrichmentOptions.html)

Text enrichment is the process of replacing and augmenting input text like `[[/roll 1d6]]` in the final rendered HTML. It's most commonly used with the `{{editor}}` Handlebars helper.

As of v10, text enrichment is an *asynchronous* process, which means it needs to happen inside `getData` before template rendering. The second argument implements EnrichmentOptions; make sure to pass `async: true` to avoid triggering a deprecation warning.
```js
// Text enrichment is available for all applications
// it's just most commonly needed by custom DocumentSheets
class MyApplication extends DocumentSheet {
  async getData() {
    const context = super.getData();
    // htmlContent might be `this.document.system.description` or some other similar path
    context.enrichedDescription = await TextEditor.enrichHTML(
      this.document.system.description, 
      { 
        // Only show secret blocks to owner
        secrets: this.document.isOwner,
        async: true,
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
{{editor enrichedDescription target="system.description" editable=editable button=true engine=prosemirror" collaborate=false}}
```

If you're just trying to display enriched text without providing an editor input - such as an item's description in an actor sheet - triple braces will render a string as raw HTML.

```handlebars
{{{enrichedDescription}}}
```

#### Defining your own enrichers

API Reference
- [TextEditorEnricherConfig](https://foundryvtt.com/api/interfaces/client.TextEditorEnricherConfig.html)
- [TextEditorEnricher](https://foundryvtt.com/api/modules/client.html#TextEditorEnricher)

By default, Foundry includes enrichers for the following:
- Content links like `@UUID[uuid]{name}`
- Hyperlinks
- Inline rolls like `[[/roll]]`

If you wish to add more, add them to the array at `CONFIG.TextEditor.enrichers` during the `init` hook. The following demonstrates an enricher that replaces all instances of `<p>Foo</p>` with `<p>Bar</p>`.

```js
/**
 * @param {RegExpMatchArray} match    - The regular expression match result
 * @param {EnrichmentOptions _options - Options provided to customize text enrichment
 *                                      (can usually be ignored for custom enrichers)
 * @returns {HTMLElement|null} An HTML element to insert in place of the matched text or null to
 *                             indicate that no replacement should be made.
 */ 
async function myEnricher(match, _options) {
  // You usually want to do something with the matched string
  // Depending on your regex you'll also have capture groups
  // const matchString = match[0];
  const p = document.createElement(p)
  p.textContent = "Bar"
  return p;
}

Hooks.once("init", () => {
  CONFIG.TextEditor.enrichers.push({pattern: /<p>Foo<\/p>/g, enricher: myEnricher})
}

```

---
## Troubleshooting

Below are some of the common foibles in Foundry application development

### Using a button triggers full web page refresh

The default HTML behavior for a `button` element is to submit the whole webpage as a form and refresh the page. To avoid this, you need to use `<button type="button">` so it only performs your click listener.

### The data provided to my Handlebars template isn't what I expect

First, review the above section on `Application#getData`. Second, read [this guide](https://foundryvtt.wiki/en/development/guides/from-load-to-render) on how data flows through the various classes.

### Arrays in Forms

Foundry only natively handles arrays of primitives in its forms - that is, an array of strings, numbers, or booleans. If you have an array of objects, you have two options
- Override the `_getSubmitData` method, calling `super` then modifying the `data` it returns.
- Implement a [DataModel](/en/development/api/DataModel) for whatever you're returning, allowing the casting in `ArrayField` to handle the transformation.