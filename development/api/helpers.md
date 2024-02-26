---
title: Helpers and Utils
description: Independently useful functions in the Foundry API
published: true
date: 2024-02-26T19:05:38.864Z
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

The following section highlights some important utility functions every developer should be familiar with.

### Global Methods



#### foundry.utils.mergeObject()

### Handlebars Helpers

Foundry's helpers augment the [built-in helpers](https://handlebarsjs.com/guide/builtin-helpers.html).

#### Localize

Primary article: [Localization](/en/development/api/localization)

The localize helper represents two different functions; `game.i18n.localize` and `game.i18n.format`. If you only pass a single argument, `localize` is called and it's a simple translation. If you pass additional arguments, they get fed into `format`.

```handlebars
<!-- Returns "Actor" -->
{{localize "DOCUMENT.Actor"}}

<!-- Returns "Create New Actor" -->
{{localize "DOCUMENT.Create" type="Actor"}}
```

#### SelectOptions

The `selectOptions` Handlebars helper is provided by Foundry for more easily building the list of options in a drop-down list. This is typically used in actor and item sheets, for offering the selection of one option among multiple.

The helper takes an object of values and labels (either labels to use as-is or localization strings) and generates the proper sequence of `<option>` HTML elements for them.

The appropriate object for the options is often defined as a constant in the system, but can also be generated dynamically if needed. The object of choices gets passed into the template in the `getData` function of the application.

#### Undocumented Helpers

The following helpers are not emitted to the foundry API page, but are nevertheless very useful:

```handlebars
{{timeSince}}

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
(lt v1 v2)

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
```hbs
<select name="system.mychoice">
    {{selectOptions optionObj selected=system.mychoice localize=true}}
</select>
```
**Output**
The final HTML generated will be this (assuming the initial value of system.mychoice is bravo, for illustrative purposes). It will save `alpha`, `bravo`, or `charlie` as a string to the document's data in the appropriate spot (if used in a DocumentSheet that has a document to save things to; tweak your name="" and such as-needed for other usages).

**HTML**
```
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

## Troubleshooting

> Stub
> This section is a stub, you can help by contributing to it.