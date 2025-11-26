---
title: DialogV2
description: A lightweight Application that renders a dialog containing a form with arbitrary content, and some buttons.
published: true
date: 2025-11-26T22:33:48.885Z
tags: documentation, docs
editor: markdown
dateCreated: 2024-06-12T23:19:13.654Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

The DialogV2 class, introduced in v12 alongside [AppV2](/en/development/api/applicationv2), is a responsive and modern way to present basic choices to users.

Official documentation
- [DialogV2](https://foundryvtt.com/api/classes/foundry.applications.api.DialogV2.html)
- [DialogV2Configuration](https://foundryvtt.com/api/interfaces/foundry.DialogV2Configuration.html)
- [DialogV2Button](https://foundryvtt.com/api/interfaces/foundry.DialogV2Button.html)

**Legend**
```js
DialogV2.confirm // `.` indicates static method or property
DialogV2#render // `#` indicates instance method or property
```

## Overview

The DialogV2 class is a quick and easy way to render a simple application window with a handful of buttons. It is built upon AppV2, so it inherits the dynamic styling for both light and dark mode.

Use DialogV2 if:
- You need the user to make a simple choice
- You don't expect to re-render the application based on user actions
- The application represents a pause in a workflow that should prevent other actions

It's better to extend ApplicationV2 yourself, e.g. with `HandlebarsApplicationMixin`, if:
- You expect to handle complex form data
- You want your application to have tabs
- There isn't a clear "I'm done here" state in your process
- You need to re-render the application at some point

The DialogV2 class can be accessed via `foundry.applications.api.DialogV2`, and the code can be found in `yourFoundryInstallPath\resources\app\client-esm\applications\api\dialog.mjs`

## Key Concepts

Any usage of DialogV2 should keep the following in mind.

### Options

DialogV2 inherits all the options from [ApplicationConfiguration](https://foundryvtt.com/api/interfaces/foundry.applications.types.ApplicationConfiguration.html), which has the `window` property that includes a field for `title` you should always set. You may also want to alter the the `classes` array which can be helpful for specifying the styling of your dialogs. 

In addition, the options from [DialogV2Configuration](https://foundryvtt.com/api/interfaces/foundry.DialogV2Configuration.html) are all important to implement; `buttons` and `content` especially are where you usually do the most work to configure the dialog. The only automatic [localization](/en/development/api/localization) for these properties is the `label` of any button; use template strings and calls to `game.i18n.localize` for defining your `content`.

One option in particular that should be used with care is `modal`, which causes the dialog to disable the rest of the Foundry UI while it is active. This can be good for simple pauses in a workflow, but any higher complexity dialog should avoid using this property.

### Button Callbacks

API Reference

- [DialogV2Button](https://foundryvtt.com/api/interfaces/foundry.DialogV2Button.html)
- [DialogV2ButtonCallback](https://foundryvtt.com/api/types/foundry.DialogV2ButtonCallback.html)

The `callback` property of a DialogV2Button determines the return of that button when using the provided static methods - `confirm`, `prompt`, and `wait`. If no callback is defined or the callback returns a nullish result (`null` or `undefined`), it will return the value of the mandatory `action` property (a string). If the callback returns a value, then that value is the return of the button. 

Frequently you may want to grab the value of an input in the dialog - to do so in the callback, `button.form.elements` is a Record of all of the input elements with the key being the element's name. So to access an input with `name="foo"` you could go `button.form.elements.foo.value`. All dialogs wrap the `content` provided in a form, that tag does not need to be provided in your html.

This behavior can be modified by passing a function to the `submit` property of the `options` when constructing the dialog or by overriding the `_onSubmit` method in an extension of the class.

## API Interactions

There are a few basic ways of invoking DialogV2 that can simplify your code. These are all asynchronous operations. Also keep in mind that any strings should probably be template strings that make calls to `game.i18n.localize` or `game.i18n.format` as needed; the below examples use static strings for readability, but actual implementations should take advantage of Foundry's [localization](/en/development/api/localization) system.

### confirm

API Reference
- [DialogV2.confirm](https://foundryvtt.com/api/classes/foundry.applications.api.DialogV2.html#confirm)

The `confirm` static method provides a simple way to get a yes/no response. Yes returns true, no returns false. 

```js
const likesIceCream = await foundry.applications.api.DialogV2.confirm({
  window: { title: "Sweet Treat Check" },
  content: "<p>Do you like ice cream?</p>"
})
```

If needed, the default behavior of the `yes` and `no` buttons can be overridden by providing properties matching their name in argument object, e.g. `yes: { class: "mymodule yes"}`. 

### prompt

API Reference
- [DialogV2.prompt](https://foundryvtt.com/api/classes/foundry.applications.api.DialogV2.html#prompt)

The `prompt` static method gives the user a simple input that can be properly `await`ed before proceeding.

```js
const proceed = await foundry.applications.api.DialogV2.prompt({
  window: { title: "Proceed" },
  content: "<p>Do you wish to continue?</p>"
})
```

If needed, the default behavior of the `ok` button can be overridden by providing properties matching their name in argument object, e.g. `ok: { class: "mymodule ok"}`. 

### wait

API Reference
- [DialogV2.wait](https://foundryvtt.com/api/classes/foundry.applications.api.DialogV2.html#wait)

The `wait` static method is the most flexible of the three and covers a wide range of uses. While the prior two methods *do* accept additional buttons, the `wait` method *requires* them. 

```js
const method = await foundry.applications.api.DialogV2.wait({
  window: { title: "D20 Roll" },
  content: "<p>Roll Method?</p>"
  // This example does not use i18n strings for the button labels, 
  // but they are automatically localized.
  buttons: [
  {
    label: "Advantage",
    action: "advantage",
  },
  {
    label: "Standard",
    action: "standard",
  },
  {
    label: "Disadvantage",
    action: "disadvantage",
  },
  ]
})
```

This sample dialog will return the value `advantage`, `standard`, or `disadvantage` to the `method` constant - the values of each button's `action` property. If you provide a `callback` function, then the return of that function will be used in place of the `action`.

### input 

API Reference
- [DialogV2.input](https://foundryvtt.com/api/classes/foundry.applications.api.DialogV2.html#input)

A simple variant of `prompt` that returns the form data by default. You can adjust the button's label or icon via the `ok` property.

```js
const data = await foundry.applications.api.DialogV2.input({
  window: { title: "Favorite Color" },
  content: `<input type="text" name="color">`,
  ok: {
    label: "Save",
    icon: "fa-solid fa-floppy-disk",
  }
})

// data.color will have the input value
```

When working with many inputs, this will give a flat object pairing all of the input `name` properties with the values submitted. If you need to transform the object to a nested structure, the [`foundry.utils.expandObject`](https://foundryvtt.com/api/functions/foundry.utils.expandObject.html) function can help.

### rejectClose

The `rejectClose` property, by default `true` in v12 and `false` in v13, causes a dialog to throw an error if it is closed, halting all execution. If you would prefer to throw if the user closes the dialog, pass `rejectClose: true`. 


### query

API Reference
- [DialogV2.query](https://foundryvtt.com/api/classes/foundry.applications.api.DialogV2.html#query)

The most complex of the static methods is `query`, which is a way for one user to prompt another user with a dialog, with proper handling of the inter-client communication via sockets and promises.

```js
// Assuming you have some other way of designating the `user`
// Asks permission to proceed
const dialogOptions = {
  window: { title: "Proceed" },
  content: "<p>Do you wish to continue?</p>"
}

const proceedA = await foundry.applications.api.DialogV2.query(user, "confirm", dialogOptions)

// This is mostly equivalent to directly invoking User#query
// with the exception of not having any way to specify the `timeout` option
// which takes a number of milliseconds to wait before guaranteeing a return
const proceedB = await user.query("dialog", { type: "confirm", config: dialogOptions }, { timeout: 30 * 1000 });
```

A particularly powerful use is combining `DialogV2.query` with the `"input"` type, which allows basically arbitrary data to be transferred between clients. This can be very useful for coordinating rolls and other similarly advanced info.

## Specific Use Cases

Here are some common tips and tricks while working with DialogV2

### Creating inputs with JS

By default, applications only return based on their buttons inputs. However, a common desire is taking simple inputs, such as situational bonuses for a dice roll or picking an option in a Select. The `foundry.applications.fields` [namespace](https://foundryvtt.com/api/modules/foundry.applications.fields.html) provides a number of functions to generate `input` and `select` elements that can be fed into your `content`.

```js
const fields = foundry.applications.fields;

const textInput = fields.createNumberInput({ 
  name: 'foo',
  value: 'Starting Value'
});

const textGroup = fields.createFormGroup({
  input: textInput,
  label: "My text input",
  hint: "Optional hint"
});

const selectInput = fields.createSelectInput({
  options: [
    {
      label: "Option 1",
      value: 'one'
    }, { 
      label: "Option 2",
      value: 'two'
    }
  ],
  name: 'fizz'
})

const selectGroup = fields.createFormGroup({
  input: selectInput,
  label: "My Select Input",
  hint: "Another Hint"
})

const content = `${textGroup.outerHTML} ${selectGroup.outerHTML}` 
```

Alternatively, if you are working with data models, the [`toInput`](https://foundryvtt.com/api/classes/foundry.data.fields.DataField.html#toInput) and [`toFormGroup`](https://foundryvtt.com/api/classes/foundry.data.fields.DataField.html#toFormGroup) functions can help.

```js
const prop = 'foo.bar';

const field = doc.system.schema.getField(prop);

// If the field doesn't have a `label` and `hint` property,
// or you want to customize the label and hint,
// you can pass them into the first parameter of the function.
const group = field.toFormGroup({}, { value: foundry.utils.getProperty(doc.system, prop) });

const content = group.outerHTML;
```

However you construct it, that `content` can then be passed into a DialogV2 static function.

### Using renderTemplate to generate the `content`

If you have a complex chunk of HTML you want to render that doesn't need re-rendering and can be handled as a dialog, outsourcing the HTML construction to handlebars can be an effective tactic. To do that, use the `renderTemplate(path, data)` function which is globally available. The `path` argument is a string representing a handlebars file on the server, while `data` is an object of properties used to fill in the handlebars. This function is asynchronous to allow fetching the file from the server if it's not in the cache.

```js
const content = await renderTemplate('path/to/template.hbs', data)

const response = await foundry.applications.api.DialogV2.prompt({
  window: { title: "Proceed" },
	content,
  modal: true
})
```

### The `render` option

API Reference
- [DialogV2RenderCallback](https://foundryvtt.com/api/types/foundry.DialogV2RenderCallback.html)

When using `confirm`, `prompt`, or `wait`, you can pass a function to the `render` property to trigger when the dialog finishes rendering. This can be useful for adding event listeners to the rendered HTML. 

### Adding actions

Sometimes a Dialog needs extra click listeners. This can be accomplished by leveraging the AppV2 actions framework.

**In the HTML**: Add a `button` element with `type="button"` and  `data-action`; other attributes such as the inner content are up to you.

**In the JS**: Pass an `actions` object with method callbacks matching the values of each `data-action` you want to use. An important note here is that arrow methods will *not* get you `this` as the Dialog reference; prefer anonymous functions or the like.

```js
const response = await foundry.applications.api.DialogV2.prompt({
  window: { title: "Proceed" },
  content: '<button type="button" data-action="clickMe">Click Me!</button>',
  actions: {
    clickMe: function (event, target) {
			console.log(this, event, target);
    }
  }
})
```

### Extending DialogV2

One way to ensure common styling throughout your own use of DialogV2 is to have a consistent class you apply to it, such as the id for your package. You can extend DialogV2 and include information in the DEFAULT_OPTIONS that will then be used as additional defaults. You will then have to use this class instead of DialogV2, but that may be preferable to always invoking DialogV2's deeply nested namespace.

```js
class MyPackageDialog extends foundry.applications.api.DialogV2 {
  static DEFAULT_OPTIONS = {
    classes: ["my-package"]
  }
}
```

## Troubleshooting

Here are some of the common problems when working with DialogV2

### Asynchronicity

All three of the primary DialogV2 static methods are asynchronous, which means they return a Promise. Handling that promise requires the use of `then` or `await`. It also means that modules cannot use them in a `preCreate` hook, which operates synchronously, but systems *can* use them in the `_preCreate` method on Documents or TypeDataModel, as that function is asynchronous.

### Re-rendering

DialogV2 does *not* support re-rendering. If you need to re-render your application, use [ApplicationV2](/en/development/api/applicationv2) instead.