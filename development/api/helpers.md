---
title: Helpers and Utils
description: Independently useful functions in the Foundry API
published: true
date: 2024-05-02T23:23:05.921Z
tags: documentation
editor: markdown
dateCreated: 2024-02-26T16:09:16.281Z
---

![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

Foundry has a LOT of generally useful functions that are not highlighted in other documentation. This page seeks to organize them 

Official documentation
- [Client Utils](https://foundryvtt.com/api/modules/client.html)
- [Common Utils](https://foundryvtt.com/api/modules/foundry.utils.html)
- [Handlebars Helpers](https://foundryvtt.com/api/classes/client.HandlebarsHelpers.html)
- [Primitive Flattens](https://foundryvtt.com/api/modules/primitives.html)
  - [Array](https://foundryvtt.com/api/modules/primitives.Array.html)
  - [Date](https://foundryvtt.com/api/modules/primitives.Date.html)
  - [Math](https://foundryvtt.com/api/modules/primitives.Math.html)
  - [Number](https://foundryvtt.com/api/modules/primitives.Number.html)
  - [Set](https://foundryvtt.com/api/modules/primitives.Set.html)
  - [String](https://foundryvtt.com/api/modules/primitives.String.html)
  - [RegExp](https://foundryvtt.com/api/modules/primitives.RegExp.html)
  - [URL](https://foundryvtt.com/api/modules/primitives.URL.html)
  
**Legend**
```js
Array.fromRange // static method
Array#equals // instance method
```

File paths provided on this page are relative to the `yourFoundryInstall\resources\app` directory.

## Overview

The [MDN Javascript Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript) is very thorough and helpful for new developers, but the functions available in Foundry layer provide a mix of implementations of common gaps in the Javascript Language as well as API-specific helpers and utilities.

## Key Concepts

The functions covered here are ultimately just shortcuts to things you can implement yourself; it can be very useful to look at the client-side code and see how they're implemented for details and inspiration. 

### Utils

Key files: `client\core\utils.js` and `common\utils\helpers.mjs`

These methods cover an enormous range of functions available within the Foundry application; some are niche, like `benchmark`, and others are workhorses that are constantly used, like `mergeObject`. There's no special requirements to using them; they each serve their own purpose.

Note: The distinction between `client` and `common` is that the latter are also used by the Foundry server.

> **Upcoming Deprecations**
> As of v12, global calls to the functions available in [Common Utils](https://foundryvtt.com/api/modules/foundry.utils.html) are deprecated. For future compatibility, use the already-available `foundry.utils` namespace. The [Client Utils](https://foundryvtt.com/api/modules/client.html) are not changing (most importantly: `fromUuid`, `fromUuidSync`, and `getDocumentClass`).
{.is-warning}


### Handlebars Helpers

Key files: `client\apps\templates.js`

Foundry's [Applications](/en/development/api/application) use handlebars for rendering. To help with this, the core software has provided a number of default "Helpers" which you can reference in your `hbs` files via `{{helper arg1 option1="foo" option2=barVar}}`.

You can nest handlebars helpers by using parentheses: `{{localize (concat "foo." barVar}}`.

You can also invoke the HandlebarsHelpers class in your javascript; this is most useful for modules injecting dom elements as part of a render hook. One important note here is that the `options` properties *must* be passed inside of a property named `hash` inside another object, e.g.

```js
let  value = 3
const options: {sign: true}

HandlebarsHelpers.numberFormat(value, {hash: options}) // returns '+3'
```

### Primitive Extensions

Key files: The `common\primitives` directory

Rather than just use the common utility functions, Foundry has also directly modified and extended the core [Javascript primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive). Javascript does not include `Array.fromRange` natively, but you can access that static method anywhere in Foundry. These functions can be extraordinarily useful, but at the cost that their presence can confuse and surprise both new and veteran developers alike.

## API Interactions

The following section highlights some important utility functions every developer should be familiar with. It is not a comprehensive list; for that, see the documentation at the top of the page. 

### Global Methods

These methods are generally useful across the foundry API; functions available in `foundry.utils` are prefaced as such and should be called that way.

### [fromUuid(uuid)](https://foundryvtt.com/api/modules/client.html#fromUuid) and [fromUuidSync(uuid)](https://foundryvtt.com/api/modules/client.html#fromUuidSync)

Primary article: [CompendiumCollection](/en/development/api/CompendiumCollection)

These functions allow you to grab a pointer to any document. `fromUuid` is asynchronous and always returns a pointer, while `fromUuidSync` is synchronous and will only return an index entry if the document is inside a compendium. Both are very useful for tracking down things like a token actor for an unlinked token, since those are nested several layers deep.

#### [foundry.utils.deepClone(object, {strict=false}={})](https://foundryvtt.com/api/modules/foundry.utils.html#deepClone)

Ordinarily, javascript passes objects and arrays by *reference* rather than by *value* - if you edit that object or array, it will *mutate* the original. When this is undesired, deepClone can help, with the caveat that deepClone will *not* help on any advanced data like a Set or another class. This is an important caveat when working with a system that has implemented [Data Models](/en/development/api/DataModel) for its types: the `system` property is a class instantiation and so will still be passed by reference if you call deepClone on the parent object.

Another common use for deepClone is while debugging; `console.log` on an object or array will output a reference to the object, so if updates are performed *after* the log call then those will be included when you inspect in the console. DeepClone can allow you to properly capture a snapshot.

Keep in mind that this kind of operation can be performance intensive and you should carefully evaluate why you need a fresh object rather than mutating the reference.

#### [foundry.utils.mergeObject(original, other, options)](https://foundryvtt.com/api/modules/foundry.utils.html#mergeObject)

Foundry makes heavy use of nested object properties, and combining objects is a frequent need. One basic use of `mergeObject` is updating `CONFIG` in an init hook.

```js
// Object to add to CONFIG
MYPACKAGE = {}

Hooks.once("init", () => {
	foundry.utils.mergeObject(CONFIG, { MYPACKAGE });
}

```

#### [foundry.utils.isNewerVersion(v1, v0)](https://foundryvtt.com/api/modules/foundry.utils.html#isNewerVersion)

This method is helpful when attempting to provide multi-version compatibility across core software and system updates. This supports both strings and numbers and is written with [semantic versioning](https://semver.org/) in mind.

The most common targets for v1 are `game.version` and `game.system.version` for the core software and system versions respectively. It's important to check if a game system uses a leading `v` in their versioning definition so your comparisons can be accurate.

#### [getDocumentClass(documentName)](https://foundryvtt.com/api/modules/client.html#getDocumentClass)

This is the canonical way to find the correct class for a document after configuration has happened.


### Handlebars Helpers

Foundry's helpers augment the [built-in helpers](https://handlebarsjs.com/guide/builtin-helpers.html). In addition to the helpers highlighted below, there's a number of other input helpers like numberInput, rangeInput, etc. that can simplify your rendering logic.

#### [localize](https://foundryvtt.com/api/classes/client.HandlebarsHelpers.html#localize)

Primary article: [Localization](/en/development/api/localization)

The localize helper represents two different functions; `game.i18n.localize` and `game.i18n.format`. If you only pass a single argument, `localize` is called and it's a simple translation. If you pass additional arguments, they get fed into `format`.

```handlebars
<!-- Returns "Actor" -->
{{localize "DOCUMENT.Actor"}}

<!-- Returns "Create New Actor" -->
{{localize "DOCUMENT.Create" type="Actor"}}
```

#### [selectOptions](https://foundryvtt.com/api/classes/client.HandlebarsHelpers.html#selectOptions)

The `selectOptions` Handlebars helper is provided by Foundry for more easily building the list of options in a drop-down list. This is typically used in actor and item sheets, for offering the selection of one option among multiple.

The helper takes an object of values and labels (either labels to use as-is or localization strings) and generates the proper sequence of `<option>` HTML elements for them.

The appropriate object for the options is often defined as a constant in the system, but can also be generated dynamically if needed. The object of choices gets passed into the template in the `getData` function of the application.

#### Undocumented Helpers

The following helpers are not emitted to the foundry API page, but are nevertheless very useful:

```handlebars
<!-- Turns a timestamp into a relative string. -->
<!-- The input can be a Date or string -->
{{timeSince timeStamp}}

<!-- The following helpers return a boolean -->
<!-- They should be used inside an #if or #unless -->
<!-- As such they are presented to be nested, with ( ) instead of {{ }} -->

<!-- Returns v1 === 2 -->
(eq v1 v2)

<!-- Returns v1 !== 2 -->
(ne v1 v2)

<!-- Returns v1 < 2 -->
(lt v1 v2)

<!-- Returns v1 > 2 -->
(gt v1 v2)

<!-- Returns v1 <= 2 -->
(lte v1 v2)

<!-- Returns v1 >= 2 -->
(gte v1 v2)

<!-- Returns !pred -->
(not pred)

<!-- Returns true if every argument is truthy  -->
(and arg1 arg2 arg3 ...)

<!-- Returns true if any argument is truthy -->
(or arg1 arg2 arg3 ...)
```

One warning with these: Your primary application logic should still be occuring within `getData`, you should use these only sparingly.

### Primitive Extensions

> Stub
> This section is a stub, you can help by contributing to it.

## Specific Use Cases

The following are walkthroughs of more complicated helpers or utils in the context of an overall package implementation.

### SelectOptions

This worked example assumes you're using the Boilerplate system development template or some other structure in which you have a `config.mjs` file for your system's constants and use `context` as the object that `getData` returns.

There are multiple files where the various choices get handled to get them into the proper context to feed into the template's context. They're all simple things that logically fit in those files, but it does involve multiple files to place the data in the right spots. The material listed isn't the entire content of the files/methods involved, but it should be enough of the significant context to make the usage clear.

**Localization file (for situations where you're localizing the labels)**
This defines the label strings that actually show up in the dropdown itself.

_lang/en.json_
```json
{
    "MYMODULE.choices": {
        "first": "First choice",
        "second": "Second choice",
        "third": "Third choice"
    }
}
```
**`config.mjs` (the config file where system constants are defined)**
This defines the internal value list and points at the appropriate translation strings from the localization file.

_module/helpers/config.mjs_
```js
export const MYMODULE = {};

MYMODULE.dropdownChoices = {
    "alpha": "MYMODULE.choices.first",
    "bravo": "MYMODULE.choices.second",
    "charlie": "MYMODULE.choices.third",
};
```
**Primary JS file for package (listed in your manfiest file's `esmodules`)**
This code adds the constants object to Foundry's overall CONFIG scope for later use wherever you need it. If you're using Boilerplate, it should be set up in this way already.
```js
import {MYMODULE} from "./helpers/config.mjs";

Hooks.once('init', () => {
    CONFIG.MYMODULE = MYMODULE;
});
```
**The `Application` subclass rendering the dropdown**
This code goes in the `getData` of the `Application` in question (typically an `ActorSheet` or `ItemSheet`, but the technique for using a `selectOptions` is the same regardless of what kind of `Application` is being rendered).
```js
getData() {
    const context = super.getData();
    context.optionObj = CONFIG.MYMODULE.dropdownChoices;
    return context
}
```
**The template Handlebars file for the application**
This is where the `selectOptions` Handlebars helper actually gets used. It is taking the `optionObj` from the Handlebars rendering context, a selected argument with the current value (so that it can have the dropdown initially showing that choice), and an argument indicating that localization should be performed. The `<select name="system.mychoice">` uses the `name=""` to determine where in the data the value should be saved to (assuming the typical use-case of this defining a dropdown for an `ActorSheet`/`ItemSheet` or something similar).
```handlebars
<select name="system.mychoice">
    {{selectOptions optionObj selected=system.mychoice localize=true}}
</select>
```
**Output**
The final HTML generated will be this (assuming the initial value of system.mychoice is bravo, for illustrative purposes). It will save `alpha`, `bravo`, or `charlie` as a string to the document's data in the appropriate spot (if used in a DocumentSheet that has a document to save things to; tweak your name="" and such as-needed for other usages).

**HTML**
```html
<select name="system.mychoice">
    <option value="alpha">First Choice</option>
    <option value="bravo" selected>Second Choice</option>
    <option value="charlie">Third Choice</option>
</select>
```
  
**Output**
  
<select name="system.mychoice" style="border:1px solid black">
    <option value="alpha">First Choice</option>
    <option value="bravo" selected>Second Choice</option>
    <option value="charlie">Third Choice</option>
</select>
  
#### Conclusion
Ultimately, the `selectOptions` helper has the potential to significantly simplify using standardized and localized choices from an object of options. It is possible to define a similar HTML structure manually or through Handlebars `{{#each}}` loops and such, but it's a lot more prone to mistakes than using the helper designed to do it for you.

### SignedString // NumberFormat

You can display signed number that's editable as a number by combining `input type='text'`, `data-dtype='Number'`, and `Number#signedString()` or `{{numberFormat value sign=true}}`. 

- A number input cannot display "+5", but a text input can.
- `data-dtype='Number'` is a special property in Foundry that will cast the input from a string to a number as a FormApplication is submitted. ([FormDataExtended##castType](https://foundryvtt.com/api/classes/client.FormDataExtended.html#_castType))
- Either in your `getData` you can use `Number#signedString()` to derive the display value, or you can use the `numberFormat` helper with `sign=true`; it depends on how exactly you've structured your data which is better.

```handlebars
<!-- The name and value attributes will depend on your getData and form object -->
<input
	type='text'
	data-dtype='Number'
	name='system.attribute.mod'
	value={{numberFormat system.attribute.mod sign=true}}
/>
```

### Branching version logic

Supporting multiple versions of Foundry in the same codebase can be tricky when there's breaking API changes. One way to address this is `foundry.utils.isNewerVersion`. You can always pair this with the core software compatibility controls in `module.json` or `system.json`

```js
// `Game#release.generation` returns just the major version number, e.g. 11
const isV11 = foundry.utils.isNewerVersion(12, game.release.generation)

// `Game#version` returns the full version string, e.g. 11.315
const isV12dev2 = !foundry.utils.isNewerVersion("12.319", game.version)

// You also don't *have* to use the helper function when you just need the major version
const isV12 = game.release.generation >= 12
```

### formInput and formField

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Using the `formInput` and `formField` helpers can be confusing for nested structures, even if they otherwise simplify sheet templating. For them to work properly, you *must* have implemented a [Data Model](/en/development/api/DataModel) for the document subtype. Remember that these are just *helpers* and are in no ways mandatory.

- The main argument, `fields`, takes a pointer to the actual DataField instance it's rendering
  - Your `getData` or `_prepareContext` needs to provide `this.document.schema.fields` for base document properties (e.g. `Actor#name`). 
  - However, this pointer won't be able to traverse any nested data model instances, such as the `system` field; you'll need to provide a separate pointer, e.g. `context.systemFields = this.document.system.schema.fields`.
- Traversing a nested structure of SchemaField requires alternating with the `fields` property; a simple path to `system.details.biography.value` turns into `systemFields.details.fields.biography.fields.value`
- Similar complications arise if you use the `EmbeddedDataField` class - it may be simpler in those cases to just use normal input creation.
- `formInput` optional arguments are an instance of [FormInputConfig](https://foundryvtt.com/api/v12/interfaces/foundry.applications.fields.FormInputConfig.html)
- `formField` optional arguments are a union of FormInputConfig and [FormGroupConfig](https://foundryvtt.com/api/v12/interfaces/foundry.applications.fields.FormGroupConfig.html)

One example of their implementation is the new UserConfig application, available at `resources\app\client-esm\applications\sheets\user-config.mjs` and its template `resources\app\templates\sheets\user-config.hbs`

#### The `widget` option

One way to offload HTML construction from the Handlebars template to a javascript function is the `widget` option, which takes a function with the signature `(FormGroupConfig, FormInputConfig} => HTMLDivElement`\*. This is only available to the `formField` helper, not `formInput`, and its actual utility as compared to defining the structure in the template directly depends on the complxity of the application. 

\*The function could technically return any HTML element with a valid `outerHTML` property, not just a div.

## Troubleshooting

> Stub
> This section is a stub, you can help by contributing to it.