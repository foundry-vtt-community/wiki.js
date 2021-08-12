---
title: Understanding FormApplication
description: 
published: true
date: 2021-08-12T16:05:30.539Z
tags: 
editor: markdown
dateCreated: 2021-02-22T16:19:21.040Z
---

> This is known to be accurate as of Foundry Core 0.8.x
{.is-info}


[`FormApplication`](https://foundryvtt.com/api/FormApplication.html) is an abstract subclass of `Application` designed specifically for the case where you have some underlying data object and you want to present a UI to configure that data object.

## FormApplication vs Dialog vs Application
Use **Application** if...
- You need only a blank window in which to render your own components and have no need of FormApplication's advanced features
- You have no interactive elements

Use a **Dialog** if...
- You are writing a macro. Defining and instanciating a `FormApplication` class within the limitations of a macro is not practical
- You need only relatively simple input, e.g. a confirmation or similar
- You are simply displaying simple output and not taking complex user input

Use **FormApplication** if...
- You need data binding to a complex object that resembles foundry entity data
- You need a TinyMCE editor or other input type that requires support from the FormApplication class


## What does a FormApplication do?
When you create an instance of a `FormApplication` subclass, you must provide the underlying object which the `FormApplication` will modify. When the `FormApplication` is submitted (and in other configurable scenarios), the `FormApplication` will apply the changes to the form to the provided data object.

`FormApplication` also provides some useful default functionality for things you might want your form to do, including:

 - Making it relatively easy to include TinyMCE editors.
 - Disabling the form, so that users can view the data contained within the object, but not make modifications.
 - Adding File Picker inputs.
 - Handle color picker and range type inputs, so that the value chosen in these inputs is displayed in an associated input field (i.e. show the hex code of a chosen color in a textbox next to the color picker).
   - Using this requires your form HTML to be set up in the way that the `FormApplication` expects.

## How does it do it?
When a `FormApplication`'s `<form>` element is submitted, it gathers all of the input fields in the form and puts them in an object (with the help of [`FormDataExtended`](https://foundryvtt.com/api/FormDataExtended.html)). This object is then passed to the abstract method `FormApplication#_updateObject`, which updates the underlying object according to whatever logic is defined there by the concrete subclass you are using.

## How do I use it?

### Defining a subclass of `FormApplication`
Since `FormApplication` is an abstract class, you must define a subclass for it which meets some default assumptions. Some of these assumptions are listed in the documentation for the class, but a detailed list is included here for clarity:

1. `FormApplication`s are intended to target/edit one object at a time.

2. As with all `Application` subclasses, you must override `Application.defaultOptions` in your subclass and at least provide a Handlebars template to be displayed as the content of the `Application`. `FormApplication` adds an additonal requirement that your template should have a single `<form>` element as its outermost element.
   - TODO: include notes about valid default option properties (see [`Application` docs](https://foundryvtt.com/api/alpha/Application.html)).
   - TODO: include notes about `FormApplication` specific options: `closeOnSubmit`, `submitOnChange`, `submitOnClose`, and `editable`.

3. Your subclass *must* override the abstract method `FormApplication#_updateObject`. `FormApplication` does not have a default implementation for this method, which is responsible for applying the form data to the underlying data object. It is up to you to determine how to interpret the form data and apply it.

### Using your subclass

When you create an instance of your `FormApplication` subclass, you must provide a reference to the object that the form is modifying, like so: `new MyFormApplication(myObject).render(true)`. So long as you have implemented your subclass according to the requirements listed above, `FormApplication` should handle the rest.

> :warning: This example does not seem to match up with the statement above as it does not require an object be present. We should add that providing an object is not necessary.
{.is-warning}

### Example
```js
/**
 * Define your class that extends FormApplication
 */
class MyFormApplication extends FormApplication {
  constructor(exampleOption) {
    super();
    this.exampleOption = exampleOption;
  }

  static get defaultOptions() {
    return mergeObject(super.defaultOptions, {
      classes: ['form'],
      popOut: true,
      template: `myFormApplication.html`,
      id: 'my-form-application',
      title: 'My FormApplication',
    });
  }

  getData() {
    // Send data to the template
    return {
      msg: this.exampleOption,
      color: 'red',
    };
  }

  activateListeners(html) {
    super.activateListeners(html);
  }

  async _updateObject(event, formData) {
    console.log(formData.exampleInput);
  }
}

window.MyFormApplication = MyFormApplication;
```

```js
/**
 * To open your application
 */

new MyFormApplication('example').render(true);
```

```html
/**
 * myFormApplication.html
 */
<form class="flexcol">
  <div class="form-group">
    <label for="exampleInput">Example Input</label>
    <input type="text" name="exampleInput" style="color: {{color}}" value="{{msg}}">
  </div>
  <footer class="sheet-footer flexrow">
    <button type="submit" name="submit">
        <i class="fa fa-check"></i> OK
    </button>
  </footer>
</form>
```

## The `options` object.
All `Application`s including all `FormApplication`s have an `options` object which sets certain parameters for the function of the app.

The following options are available:
```js
(From foundry.js)

Application:

 * @param {string} options.baseApplication      A named "base application" which generates an additional hook
 * @param {number} options.width                The default pixel width for the rendered HTML
 * @param {number} options.height               The default pixel height for the rendered HTML
 * @param {number} options.top                  The default offset-top position for the rendered HTML
 * @param {number} options.left                 The default offset-left position for the rendered HTML
 * @param {boolean} options.popOut              Whether to display the application as a pop-out container
 * @param {boolean} options.minimizable         Whether the rendered application can be minimized (popOut only)
 * @param {boolean} options.resizable           Whether the rendered application can be drag-resized (popOut only)
 * @param {string} options.id                   The default CSS id to assign to the rendered HTML
 * @param {Array.<string>} options.classes      An array of CSS string classes to apply to the rendered HTML
 * @param {string} options.title                A default window title string (popOut only)
 * @param {string} options.template             The default HTML template path to render for this Application
 * @param {Array.<string>} options.scrollY      A list of unique CSS selectors which target containers that should
 *                                              have their vertical scroll positions preserved during a re-render.|

FormApplication: (inherits all the above and adds)

* @returns {Object} options                    The default options for this FormApplication class, see Application
* @returns {boolean} options.closeOnSubmit     Whether to automatically close the application when it's contained
*                                              form is submitted. Default is true.
* @returns {boolean} options.submitOnChange    Whether to automatically submit the contained HTML form when an input
*                                              or select element is changed. Default is false.
* @returns {boolean} options.submitOnClose     Whether to automatically submit the contained HTML form when the
*                                              application window is manually closed. Default is false.
* @returns {boolean} options.editable          Whether the application form is editable - if true, it's fields will
*                                              be unlocked and the form can be submitted. If false, all form fields
*                                              will be disabled and the form cannot be submitted. Default is true.
```

The `options` object (`this.options` in your code) is created when the `Application` class is instantiated. The `options` are initialized to values found in another object `defaultOptions`.

When you create your application, you may want to provide both *default* settings for all instances of your app to share, and specific settings that a given instance of the app sets.

For `defaultOptions` you will provide an override of the `static get defaultOptions()` getter. Here is an example from `class ActorSheet`:

```js
  /** @override */
  static get defaultOptions() {
    return mergeObject(super.defaultOptions, {
      height: 720,
      width: 800,
      template: "templates/sheets/actor-sheet.html",
      closeOnSubmit: false,
      submitOnClose: true,
      submitOnChange: true,
      resizable: true,
      baseApplication: "ActorSheet",
      dragDrop: [{dragSelector: ".item-list .item", dropSelector: null}]
    });
  }
```

Notice that this getter is `static` meaning that it belongs to the *class* `ActorSheet` and not to any one instance of an actor sheet. That means that within this getter, you *do not have acess* to any of the data about a given actor! So if for instance you wanted to set the `title` option to the `name` of the actor, you could not do so here.

Rather, you will need to modify `this.options` at some other point. You might do this in your constructor, or elsewhere in your code. But `defaultOptions` is for *defaults* these are the options that all instances of your appliation should share. Common options here would be the Handlebars template, the width/height of the app, and the behaviour of options like `submitOnClose`.

In addition to modifying `this.options`, it is possible to pass options directly to the `FormApplication` constructor from yours. For example:

```js
constructor(myObject) { // myObject is the object your app modifies, such as an Actor or Item
  super(myObject, { title: myObject.name });
	// The rest of your constructor
}
```
Since your application `extends FormApplication` the method `super` calls the constructor for form app, which takes two arguments: `object` (the object your form modifies/displays) and `options`, you can pass any of the options you like to this. The `Application` class will handle creating `this.options`, setting all the options to `defaultOptions` and then setting any values you passed to the `options` argument to the values you gave.

Note also, that if you are creating an application, you can add your own options! All you must do to define a new option, is add it to `defaultOptions` so that it has some initial value.

## Reacting to Input Changes

To force your FormApplication to rerender when any inputs are changed, add `this.render()` at the end of your `FormApplication#_updateObject` method.

```js
async _updateObject(event, formData) {
  // normal updateObject stuff
  
  this.render(); // rerenders the FormApp with the new data.
}
```

## The `render` method

`Application#render` itself is not `async`, which means it cannot be awaited to ensure that the rendering process is finished e.g. before manipulating the resulting HTML element. The `async` method actually rendering the `Application` is `Application#_render`, for which `render` is a thin wrapper adding some error handling in the form of a console message and setting `this._state = Application.RENDER_STATES.ERROR` in case `_render` throws an error.

## 0.7.x vs 0.8.x
TODO