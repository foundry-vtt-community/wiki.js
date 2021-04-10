---
title: Understanding FormApplication
description: 
published: true
date: 2021-04-10T17:07:09.765Z
tags: 
editor: markdown
dateCreated: 2021-02-22T16:19:21.040Z
---

[`FormApplication`](https://foundryvtt.com/api/FormApplication.html) is an abstract subclass of `Application` designed specifically for the case where you have some underlying data object and you want to present a UI to configure that data object.

# FormApplication vs Dialog
Use a **dialog** if...
- You are writing a macro. Defining and instanciating a FormApplication class within the limitations of a macro is not practical
- You need only relatively simple input, or only a confirmation or similar
- You are simply displaying simple output and not taking complex user input

Use **Application** if...
- You need only a blank window in which to render your own components and have no need of FormApplication's advanced features

Use **FormApplication** if...
- You need data binding to a complex object that resembles foundry entity data
- You need a TinyMCE editor or other input type that requires support from the FormApplication class


# What does a FormApplication do?
When you create an instance of a `FormApplication` subclass, you must provide the underlying object which the `FormApplication` will modify. When the `FormApplication` is submitted (and in other configurable scenarios), the `FormApplication` will apply the changes to the form to the provided data object.

`FormApplication` also provides some useful default functionality for things you might want your form to do, including:

 - Making it relatively easy to include TinyMCE editors.
 - Disabling the form, so that users can view the data contained within the object, but not make modifications.
 - Adding File Picker inputs.
 - Handle color picker and range type inputs, so that the value chosen in these inputs is displayed in an associated input field (i.e. show the hex code of a chosen color in a textbox next to the color picker).
   - Using this requires your form HTML to be set up in the way that the `FormApplication` expects.

# How does it do it?
When a `FormApplication` is submitted, it gathers all of the input fields in the form and puts them in an object (with the help of [`FormDataExtended`](https://foundryvtt.com/api/FormDataExtended.html)). This object is then passed to the abstract method `FormApplication#_updateObject`, which updates the underlying object according to whatever logic is defined there by the concrete subclass you are using.

# How do I use it?

## Creating a subclass of `FormApplication`
Since `FormApplication` is an abstract class, you must define a subclass for it which meets some default assumptions. Some of these assumptions are listed in the documentation for the class, but a detailed list is included here for clarity:

1. `FormApplication`s are intended to target/edit one object at a time.
2. As with all `Application` subclasses, you must override `Application.defaultOptions` in your subclass and at least provide a Handlebars template to be displayed as the content of the `Application`. `FormApplication` adds an additonal requirement that your template should have a single `FORM` element as its outermost element.
   - TODO: include notes about valid default option properties (see [`Application` docs](https://foundryvtt.com/api/alpha/Application.html)).
   - TODO: include notes about `FormApplication` specific options: `closeOnSubmit`, `submitOnChange`, `submitOnClose`, and `editable`.
3. Your subclass *must* override the abstract method `FormApplication#_updateObject`. `FormApplication` does not have a default implementation for this method, which is responsible for applying the form data to the underlying data object. It is up to you to determine how to interpret the form data and apply it.

## Using your subclass

When you create an instance of your `FormApplication` subclass, you must provide a reference to the object that the form is modifying, like so: `new MyFormApplication(myObject).render(true)`. So long as you have implemented your subclass according to the requirements listed above, `FormApplication` should handle the rest.

## Example
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

# 0.7.x vs 0.8.x
TODO