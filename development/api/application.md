---
title: Application
description: The standard application window that is rendered for a large variety of UI elements in Foundry VTT.
published: true
date: 2024-02-27T01:53:23.247Z
tags: documentation
editor: markdown
dateCreated: 2024-02-13T19:36:31.269Z
---

Applications are a core piece of Foundry's API that almost every developer will have to familiarize themselves with.

The [Dialog](/en/development/api/dialog) subclass has its own page detailing its specific options because its configuration is so specific.

Official Documentation
- [Application](https://foundryvtt.com/api/classes/client.Application.html)
- [FormApplication](https://foundryvtt.com/api/classes/client.FormApplication.html)
- [DocumentSheet](https://foundryvtt.com/api/classes/client.DocumentSheet.html)
- [ContextMenu](https://foundryvtt.com/api/classes/client.ContextMenu.html)

**Legend**
```js
Application.defaultOptions // `.` indicates static method or property
Application#getData // `#` indicates instance method or property
```

## Overview

The Application class is the basic building block of Foundry's UI (which, combined with the canvas that represents a scene, makes up everything you see in Foundry). At its fundamental level, it's an HTML element that is positioned somewhere in the browser window.

`Application` and its subclasses are stored in `yourFoundryInstall\resources\app\client\apps`

## Key Concepts

There are two general types of Applications, popout applications and non-popout applications. Popout applications will have a header bar and be movable (all of the pop-up windows for configuration, actor/item sheets, and so on) while non-popout applications are generally fixed UI elements (such as the sidebar, the macro hotbar, the scene navigation, and so on).

Applications are stored in the ui global namespace. So, you'll see stuff like the right sidebar tabs in `ui.sidebar`, the floating popout windows in `ui.windows`, the layer controls on the left in `ui.controls`, and so on.

### API Interactions
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

### FormDataExtended

When working with a `FormApplication`, you can cast the output of `input` elements by adding a `data-dtype` property. In modern foundry this is somewhat redundant if you're using a [Data Model](/en/development/api/DataModel), but it can still be useful for more advanced techniques. 

Valid choices include:

- "String"
- "Boolean"
- "Number"
- "JSON"

It will also generally check for `window[dataType] instanceof Function`, so you can globally define a type by globally defining a function used to cast that type.

## Specific Use Cases

### Automatic re-rendering on Document updates¶
Document classes each have an `apps` property which stores an object of Application references. This object is iterated through and each app is re-rendered whenever the Document changes, so that any open windows will reflect the new state of the document.

DocumentSheet automatically adds itself to the apps when it initially renders, which lets the document easily automatically trigger re-rendering the sheet to reflect new changes in realtime. However, applications can add themselves to a document's apps as-needed, which can be used for any non-DocumentSheet Application subclasses which need to display data about a document and keep the display properly updated.

## Troubleshooting

> Stub
> This section is a stub, you can help by contributing to it.