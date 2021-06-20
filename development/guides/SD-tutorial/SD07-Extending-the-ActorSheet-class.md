---
title: SD07 Extending the ActorSheet class
description: 
published: true
date: 2021-06-20T03:21:12.287Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:35:58.947Z
---

The ActorSheet class is the class associated with our actor's character sheets. Let's take a look at what Boilerplate System does:

## Class definition and defaultOptions()

Every sheet needs to define its default options.

```js
/**
 * Extend the basic ActorSheet with some very simple modifications
 * @extends {ActorSheet}
 */
export class BoilerplateActorSheet extends ActorSheet {

  /** @override */
  static get defaultOptions() {
    return mergeObject(super.defaultOptions, {
      classes: ["boilerplate", "sheet", "actor"],
      template: "systems/boilerplate/templates/actor/actor-sheet.html",
      width: 600,
      height: 600,
      tabs: [{ navSelector: ".sheet-tabs", contentSelector: ".sheet-body", initial: "features" }]
    });
  }

  /** @override */
  get template() {
    return `systems/boilerplate/templates/actor/actor-${this.actor.data.type}-sheet.html`;
  }
```

First, we're exporting the class (don't forget to rename yours!) and defining the default options for the sheet. In the `defaultOptions()` method we need to return a mergedObject of the default options from the main ActorSheet class and our customizations. Those customizations are:

- **classes**: an array of CSS classes to apply to the character sheet.
* **template**: the path to the Handlebars HTML template that we'll use for this character sheet. This needs to be relative to Foundry's root, so you need to include `systems/MYSYSTEMNAME` in the start of the path. If you have multiple actor or sheet types, you can also use a `get template()` method to compute the template name dynamically.
* **width**: default window width.
* **height**: default window height.
* **tabs**: Use this to define your tabs on the character sheet. This should match up with what you've created in your HTML template, but you must have a nav selector CSS class, content selector CSS class, and choose a default/initial tab.

### get template()

Document sheets also support a special `template()` getter method (see the `get` prefix before it) that can be used to retrieve the template name dynamically. Normally you don't have access to the actor data in the `defaultOptions()` getter method, so you have to use this method if you want to do something like including the actor type in the template name. For example, the following would allow for templates named `actor-character-sheet.html` and `actor-npc-sheet.html`:

```js
  /** @override */
  get template() {
    return `systems/boilerplate/templates/actor/actor-${this.actor.data.type}-sheet.html`;
  }
```

## getData()

Much like the Actor class' `prepareData()` method, we can use the `getData()` method to derive new data for the character sheet. The main difference is that values created here will only be available within this class and on the character sheet's HTML template. If you were to use your browser's inspector to take a look at an actor's available data, you wouldn't see these values in the list, unlike those created in prepareData().

> **Actor#prepareData() or ActorSheet#getData()?**
> Both of these methods are capable of calculating derived data that you can display on your sheet. The major difference is that data in the actor sheet's `getData()` method is _only_ available to the sheet itself. That means that `getData()` is great for derived data that's useful on the sheet (such as the width of an XP progress bar) or for making more convenient data structures (such as filtering the items into groups based on item type), but should be avoided for data that's useful in other contexts (such as calculated ability score modifiers).
{.is-info}

```js
  /* -------------------------------------------- */

  /** @override */
  getData() {
    // Retrieve the data structure from the base sheet. You can inspect or log
    // the context variable to see the structure, but some key properties for
    // sheets are the actor object, the data object, whether or not it's
    // editable, the items array, and the effects array.
    const context = super.getData();

    // Use a safe clone of the actor data for further operations.
    const actorData = context.actor.data;

    // Add the actor's data to context.data for easier access, as well as flags.
    context.data = actorData.data;
    context.flags = actorData.flags;

    // Prepare character data and items.
    if (actorData.type == 'character') {
      this._prepareItems(context);
      this._prepareCharacterData(context);
    }

    // Prepare NPC data and items.
    if (actorData.type == 'npc') {
      this._prepareItems(context);
    }

    // Add roll data for TinyMCE editors.
    context.rollData = context.actor.getRollData();

    // Prepare active effects
    context.effects = prepareActiveEffectCategories(this.actor.effects);

    return context;
  }
```

The first thing we're doing here is setting a new constant called `context` that's equal to `super.getData()`. We're using `context` for the variable name so that we can avoid the `data.data` changes mentioned in the previous step of the tutorial and can instead use references like `context.data.attributes.level.value`.

> **What is super.getData()?**
>
> Calling `super.getData()` will execute the `getData()` method in the `ActorSheet` class that we extended for this, so it's helpful to be aware of what exactly that gives when we execute it. As of Foundry 0.8.6, it returns an object structured as:
>
> `data`: A safe duplicate of the actor's data usable in sheets
> `actor`: The actor document
> `items`: Items on the actor document
> `effects`: Active Effects on the actor document
> `cssClass`: This will be either `editable` or `locked` based on whether the actor is editable
> `editable`: A boolean for whether or not this actor sheet should be editable
> `document`: A reference to `this.document`, as with the actor document earlier
> `limited`: Whether or not the document should have limited permissions
> `options`: Options passed to the `getData()` call
> `owner`: A boolean for if this user is either the document's owner or a GM.
> `title`: The sheet applications' title
{.is-info}

After grabbing an initial data object for the sheet and storing it in the `context` variable, we then grab the actor data like so:

```js
// Use a safe clone of the actor data for further operations.
const actorData = this.actor.data.toObject(false);

// Add the actor's data to context.data for easier access, as well as flags.
context.data = actorData.data;
context.flags = actorData.flags;
```

The first line is the most important one, as that's what retrieves a safe copy of the actor's data for sheet manipulation purposes. It uses the document data's built in `toObject()` method and gives it the `false` parameter, which instructs Foundry to not just convert this to a plain object but to also run a deep clone on nested objects/arrays. Just using `this.actor.data` can work, but if you don't use `this.actor.data.toObject(false)`, you can run into difficult to debug issues related to the original object.

Afterwards, we set up new properties for both `context.data` and `context.flags` based on the actorData that we just retrieved. The `context.data` property is the one that will be used frequently in your Handlebars templates later, as its essentially the cleanest and most direct set of the actor's data. The flags are useful to go ahead and include the structure for, but they tend to be more useful for modules than systems (flags are used for arbitrary data structures that don't have to fit the system's template.json).



## activateListeners()

If you want your sheet to be interactive, this is where that needs to happen. The `activateListeners()` method is where you can execute jQuery on your sheet to do things like create rollable skills and powers, add new items, or delete items. This method is passed an `html` object that behaves much like `$('.sheet')` would if you were trying to run jQuery logic on your sheet in your browser's console.



```js
  /** @override */
  activateListeners(html) {
    super.activateListeners(html);

    // Everything below here is only needed if the sheet is editable
    if (!this.options.editable) return;

    // Add Inventory Item
    html.find('.item-create').click(this._onItemCreate.bind(this));

    // Update Inventory Item
    html.find('.item-edit').click(ev => {
      const li = $(ev.currentTarget).parents(".item");
      const item = this.actor.getOwnedItem(li.data("itemId"));
      item.sheet.render(true);
    });

    // Delete Inventory Item
    html.find('.item-delete').click(ev => {
      const li = $(ev.currentTarget).parents(".item");
      this.actor.deleteOwnedItem(li.data("itemId"));
      li.slideUp(200, () => this.render(false));
    });
  }
```



The Boilerplate System includes three examples of click listeners, one to create new items, one to edit existing items, and one to delete items. We'll revisit this later in the tutorial to add a listener for rollable attributes.

The first click listener we added was to create new items, but notice that it uses `this._onItemCreate.bind(this)` rather than calling its code directly like the edit and delete listeners do. You can follow that code pattern to break your listeners into custom methods to make your code more organized as it grows over time. For now, let's take a closer look at the `_onItemCreate()` custom method:



```js
  /* -------------------------------------------- */

  /**
   * Handle creating a new Owned Item for the actor using initial data defined in the HTML dataset
   * @param {Event} event   The originating click event
   * @private
   */
  _onItemCreate(event) {
    event.preventDefault();
    const header = event.currentTarget;
    // Get the type of item to create.
    const type = header.dataset.type;
    // Grab any data associated with this control.
    const data = duplicate(header.dataset);
    // Initialize a default name.
    const name = `New ${type.capitalize()}`;
    // Prepare the item object.
    const itemData = {
      name: name,
      type: type,
      data: data
    };
    // Remove the type from the dataset since it's in the itemData.type prop.
    delete itemData.data["type"];

    // Finally, create the item!
    return this.actor.createOwnedItem(itemData);
  }
```



We're doing a few different things here. First, we're getting the element (header) that was clicked, and then we're finding out what type of item it was. In this case that type is `item`, but it could also be something like `feature` or `spell`.  After that, we're grabbing any custom data attributes on the element that was clicked and using them to create a new `itemData` object. Finally, we're passing all of that over to `this.actor.createdOwnedItem(itemData)` to create the item on this actor.

And since these examples have all been the individual sections, don't forget your closing bracket for the class itself!



```js
}
```



---

* **Prev:** [Extending the Actor class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD06-Extending-the-Actor-class)
* **Next:** [Creating HTML templates for your actor sheets](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD08-Creating-HTML-templates-for-your-actor-sheets)