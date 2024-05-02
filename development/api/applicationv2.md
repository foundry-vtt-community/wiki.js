---
title: ApplicationV2
description: The Application class is responsible for rendering an HTMLElement into the Foundry Virtual Tabletop user interface.
published: true
date: 2024-05-02T23:09:53.931Z
tags: documentation
editor: markdown
dateCreated: 2024-04-18T15:30:54.955Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Applications are a core piece of Foundry's API that almost every developer will have to familiarize themselves with. The ApplicationV2 class and its subclasses were introduced in Foundry V12, with the long-term goal of transitioning all applications to the new framework. The original [Application](/en/development/api/application) class isn't going away, but it is no longer the subject of active development.


*Official Documentation*

- [ApplicationV2](https://foundryvtt.com/api/v12/classes/foundry.applications.api.ApplicationV2.html)
- [DocumentSheetV2](https://foundryvtt.com/api/v12/classes/foundry.applications.api.DocumentSheetV2.html)

**Legend**

```js
ApplicationV2.DEFAULT_OPTIONS // `.` indicates static method or property
Application#render // `#` indicates instance method or property
```

## Overview

ApplicationV2 is an entirely new set of classes that serve as a complete alternative to the original Application class. If you are building a new UI element in Foundry and otherwise only plan to support Version 12 and later, it is strongly recommended you use ApplicationV2 instead of the original Application class.

Code for ApplicationV2 and its related classes can be found at `yourFoundryInstallPath\resources\app\client-esm\applications`.

---
## Key Concepts

Here are the core things to know about ApplicationV2, including comparisons to the original Application class.

### Advantages of ApplicationV2

- Native light/dark mode support
- Better application window frames
- Architecture supports non-Handlebars rendering engines much more easily
- Support for partial re-rendering in Handlebars
- Better lifecycle events
- Improved a11y handling
- Overall simpler and cleaner to implement

Another major change is there's no more JQuery-by-default in AppV2; all internal functions work exclusively with base javascript DOM manipulation. JQuery is still fully included in Foundry, so developers who prefer it can call `const html = $(this.element)` to get a jquery representation of the application's rendered HTML.

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

### DEFAULT_OPTIONS

One property that's important to include is `static DEFAULT_OPTIONS`, which is an instance of  the [ApplicationConfiguration](https://foundryvtt.com/api/v12/interfaces/foundry.applications.types.ApplicationConfiguration.html) type. You can override or extend these options in individual instances of your application by passing an object into the constructor, e.g. `new MyApplication({ position: { width: 600 }})`.

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

For those used to ApplicationV2, this largely replaces the role `activateListeners` played. If you have other event listeners to add, you can use `_onRender`, which is explored in the "Specific Use Cases" section.


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

Broadly speaking, this means the most straightforward way to structure a multi-part application is to lead with a `header` part, then optionally a distinct `tabs` part, then finally one part for each of your tabs. 

#### \_prepareContext

The variable-based rendering of handlebars is handled by `_prepareContext`, an asynchronous function that returns a `context` object with whatever data gets fed into the `template`. It has a single argument, `options`, which is the options object passed to the original `render` call, but this can usually be ignored.

In Application V1 terms, this is functionally equivalent to its `getData` call, with the only functional change that this is *always* asynchronous.

Inside your handlebars template, you'll *only* have access to the data setup in `_prepareContext`, so if you need to include information such as `CONFIG.MYSYSTEM` you'll want to include a pointer to it in the returned object.

> **Note**
> 
> The disconnect between the data provided to the template via `_prepareContext` and the way that `DocumentSheetV2` stores data to the document via the `name=""` field can cause some confusion. It's common practice to store the document's system data in a system key in the context, which means that you can usually do `value="{{system.attribute.value}}"` and `name="system.attribute.value"` in an actor/item sheet and stuff works.
>
> However, under the hood, the `{{}}` is pulling stuff from the context object that the `getData` returns while the `name=""` is storing things based on the data path in the document itself. This means that there are situations where they won't actually line up, because they're not fundamentally pointing at the same thing at the end of the day, they just happen to often line up.
{.is-info}

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
      itemQuantities.addEventListener("change", (e) => {
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

### Non-Handlebars Rendering Frameworks

> Stub
> This section is a stub, you can help by contributing to it.

---
## Troubleshooting

> Stub
> This section is a stub, you can help by contributing to it.