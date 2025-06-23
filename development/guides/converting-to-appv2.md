---
title: Converting to ApplicationV2
description: A guide to convert an Application to ApplicationV2
published: true
date: 2025-06-23T18:26:41.291Z
tags: 
editor: markdown
dateCreated: 2024-05-28T06:46:59.385Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

# Example
Here is an example of a FormApplication that will be used in this guide.

```javascript
class TemplateApplication extends FormApplication {
	static get defaultOptions() {
		return foundry.utils.mergeObject(super.defaultOptions, {
		  	id: "foo-form",
			title: `My Module: ${game.i18n.localize("FOO.form.title")}`,
			template: "./modules/foo/templates/form.hbs",
			classes: ["form"],
			width: 640,
			height: "auto",
			closeOnSubmit: true,
		});
	}

	getData() {
		const setting = game.settings.get("foo", "config");
		return {
		  setting
		}
	}

	async activateListeners(html) {
		super.activateListeners(html);
		html.find("input[name=something]").on("click", /* ... */);
		html.find("button[name=reset]").on("click", async (event) => {
		  await game.settings.set("foo", "config", {});
		});
	}

  async _updateObject(event, formData) {
    await Promise.all(
      Object.entries(formData)
      	.map(([key, value]) => game.settings.set("foo", key, value));
    );
  }
}
```
# The Basics
## Default Options
`static get defaultOptions` is replaced by `static DEFAULT_OPTIONS = {}`, but not everything that used to be there goes there.

What used to be
```js
static get defaultOptions() {
  return foundry.utils.mergeObject(super.defaultOptions, {
    id: "foo-form",
    title: `My Module: ${game.i18n.localize("FOO.form.title")}`,
    template: "./modules/foo/templates/form.hbs",
    classes: ["form"],
    width: 640,
    height: "auto",
    closeOnSubmit: true,
  });
}
```

Should now become

```js
static DEFAULT_OPTIONS = {
  id: "foo-form",
  form: {
    handler: TemplateApplication.#onSubmit,
    closeOnSubmit: true,
  },
  position: {
    width: 640,
    height: "auto",
  },
  tag: "form", // The default is "div"
  window: {
    icon: "fas fa-gear", // You can now add an icon to the header
    title: "FOO.form.title"
  }
  
}
  
get title() {
  return `My Module: ${game.i18n.localize(this.options.window.title)}`;
}
```

The template is now declared in a separate property: `static PARTS`. Note that it no longer needs to be a monolithic file.

Here you declare the parts that will be loaded. The order matters.

```js
static PARTS = {
  foo: {
    template: "./modules/foo/templates/form.hbs"
  }
}
```

## Data/Context
`getData(options)` is replaced by `_prepareContext(options)`. This is just a name change.

```js
_prepareContext(options) {
    const setting = game.settings.get("foo", "config");
    return {
      setting
    }
  }
```

## Listeners
`activateListeners(html)` is replaced by `_onRender(context, options)`. Here, `context` is the same data you've returned on `_prepareContext(options)`.

Reminder that AppV2 has abandoned jQuery, so, if you still want to use it, you must add the following: `const html = $(this.element)`.

```js
_onRender(context, options) {
  this.element.querySelector("input[name=something]").addEventListener("click", /* ... */);
  // We will deal with reset later
}
```

## Submit
On `static DEFAULT_OPTIONS` we've declared a `handler: TemplateApplication.#onSubmit`, which replaces `Application#_updateObject`.
The main difference is that formData is the whole `FormDataExtended` instead of the its `object`.

```js
static #onSubmit(event, form, formData) {
    const settings = foundry.utils.expandObject(formData.object);
	await Promise.all(
		Object.entries(settings)
			.map(([key, value]) => game.settings.set("foo", key, value))
  );
}
```

# The New Features
## Actions
`Action`s are functions that automatically get bound as `click` listeners to any element that has the appropriate `data-action` in its attributes. Importantly, these should be static functions, but their `this` value will still point to the specific class instance.

They are declared on the `DEFAULT_OPTIONS`.

On this example, we will rewrite the reset listener as an `action`:
```js
static DEFAULT_OPTIONS = {
  actions: {
    reset: TemplateApplication.reset,
  },
}

static async reset() {
  await game.settings.set("foo", "config", {});
}
```

## The Buttons
Right now you have a complete 1:1 conversion of your original Application, but you will notice that the submit button is reloading the page instead of actually triggering the form's `handler`.

Here is how you fix it:
1) On your template's HTML, remove the `submit`-type button.
2) On your AppV2's `PARTS`, add the following:
```js
static PARTS = {
  // ...
  footer: {
    template: "templates/generic/form-footer.hbs",
  },
}
```
3) On your AppV2's `_prepareContext`, add the following:
```js
_prepareContext(options) {
  return {
    // ...
    buttons: [
      { type: "submit", icon: "fa-solid fa-save", label: "SETTINGS.Save" }
    ]
  }
}
```
4) This ensure buttons added this way are nicely styled.
```js
static DEFAULT_OPTIONS = {
  // ...
  window: {
    contentClasses: ["standard-form"]
  }
}
```

Now, test it. If the issue persists, check if any of templates have a `<form>`. If they do, replace it for `<div>`.

I don't know *why* it works like this, but that's how it is.

# Styling Hints
- Add `class="standard-form"` to your HTML template's outer `<div>` so your inner `<div class="form-group">` get proper spacing.
- Replace `<p class="notes">` for `<p class="hint">`.