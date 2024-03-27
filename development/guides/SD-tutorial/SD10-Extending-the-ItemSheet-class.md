---
title: 10. Extending the ItemSheet class
description: 
published: true
date: 2024-02-20T02:52:21.565Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:36:18.600Z
---

![Foundry v11 Compatible](https://img.shields.io/badge/Foundry-v11%20Compatible-blue)

The ItemSheet class is the class associated with our item sheets. Let's take a look at what Boilerplate System does:

## defaultOptions()

Very similar to the actor sheet, we start out by defining our default options for this sheet. Our width and height tend to be smaller on item sheets so that they don't fully cover up the character sheet when they're opened. One thing that we did differently from the actor sheet is that this defaultOptions() method doesn't have a template property. That's intentional, and it could have been done on the actor sheet as well if we needed to. If you don't include a template property, you should create a `template()` getter method to return the correct template.

```js

/**
 * Extend the basic ItemSheet with some very simple modifications
 * @extends {ItemSheet}
 */
export class BoilerplateItemSheet extends ItemSheet {
  /** @override */
  static get defaultOptions() {
    return foundry.utils.mergeObject(super.defaultOptions, {
      classes: ['boilerplate', 'sheet', 'item'],
      width: 520,
      height: 480,
      tabs: [
        {
          navSelector: '.sheet-tabs',
          contentSelector: '.sheet-body',
          initial: 'description',
        },
      ],
    });
  }
```

## get template()

The `template()` method has the `get` keyword placed before it to signal that this is a getter method for a property. Like the actor sheet, we're returning different item sheets based on the item type, but you could have a single item sheet that uses handlebar helpers like `{{#if (eq type "spell")}}` to conditionally show sections of the sheet instead.


```js

  /** @override */
  get template() {
    const path = 'systems/boilerplate/templates/item';
    // Return a single sheet for all item types.
    // return `${path}/item-sheet.hbs`;

    // Alternatively, you could use the following return statement to do a
    // unique item sheet by type, like `weapon-sheet.hbs`.
    return `${path}/item-${this.item.type}-sheet.hbs`;
  }
```



## getData()

As with the actor sheet, you can use this to create display data for your item sheet if needed. Values created here would only be available within this class or on the sheet's HTML template; it would not be available accessing the item document elsewhere. 

```js
  /** @override */
  getData() {
    // Retrieve base data structure.
    const context = super.getData();

    // Use a safe clone of the item data for further operations.
    const itemData = context.data;

    // Retrieve the roll data for TinyMCE editors.
    context.rollData = this.item.getRollData();

    // Add the item's data to context.data for easier access, as well as flags.
    context.system = itemData.system;
    context.flags = itemData.flags;

    // Prepare active effects for easier access
    context.effects = prepareActiveEffectCategories(this.item.effects);

    return context;
  }
```

Unlike the actor sheet class, we don't need to use a generator to parse the effects - we can just directly pass the item's `effects` collection. (This is only displayed on the Features item by default, but you can do whatever you want with your system).

## activateListeners()

As with the actor sheet, this is where you would activate listeners and events for your item sheet. For example, if you had logic that happened on click or edit, or if you had a roll button within the item sheet, that could go in here. `html` is a jQuery object that you can use to find other elements, like `html.find('.rollable')`.

```js
  /** @override */
  activateListeners(html) {
    super.activateListeners(html);

    // Everything below here is only needed if the sheet is editable
    if (!this.isEditable) return;

    // Roll handlers, click handlers, etc. would go here.

    // Active Effect management
    html.on('click', '.effect-control', (ev) =>
      onManageActiveEffect(ev, this.item)
    );
  }
```

## Wrapping up

Don't forget closing brackets!

```js
}
```



---

* **Prev:** [Extending the Item class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD09-Extending-the-Item-class)
* **Next:** [Creating rollable buttons with event listeners](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD111-Creating-rollable-buttons-with-event-listeners)