---
title: 09 Extending the Item class
description: 
published: true
date: 2024-02-06T04:43:39.917Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:36:12.667Z
---

You can extend the Item class to use your own version, just like we did earlier with the Actor class. Let's start by taking a look a the BoilerplateItem class in <!-- {% raw %} -->`/module/item/item.js`<!-- {% endraw %} -->. As with previous examples, you'll want to rename <!-- {% raw %} -->`BoilerplateItem`<!-- {% endraw %} --> to whatever your system's name is, such as <!-- {% raw %} -->`MySystemNameItem`<!-- {% endraw %} -->.

Let's start with the class definition, stripping out all the content of the methods and leaving just the signatures for brevity.

```js
/**
 * Extend the basic Item with some very simple modifications.
 * @extends {Item}
 */
export class BoilerplateItem extends Item {
  /**
   * Augment the basic Item data model with additional dynamic data.
   */
  prepareData() {}

  /**
   * Prepare a data object which is passed to any Roll formulas which are created related to this Item
   * @private
   */
   getRollData() {}

  /**
   * Handle clickable rolls.
   * @param {Event} event   The originating click event
   * @private
   */
  async roll() {}
  }
}

```

Items don't inherently do a lot compared to Actors. An Actor can be instantiated as a token on the canvas, can be added to combats, and generally comes with a lot out of the box. Items, by themselves, do very little - they have sidebar tab, can store active effects, but ultimately exist as a modular way to store data either in a compendia or on an actor. That's useful, but is going to be more dependent on what YOU need.

> **Can you store an item within another item?**
> No, the collections of embeded documents requires a server-side connection for database handling. A given document only has the embedded collections defined at its top level. For an `actor`, that is `items` and `effects`. For an Item, that's only `effects`.
{.is-warning}

### prepareData()

Like actors, items have a `prepareData()` cycle that's called when they're loaded from the database (such as after a page refresh or after a call to `update`). The boilerplate system doesn't do anything here, but you can override the `prepareBaseData` and/or `prepareDerivedData` methods. One thing to note here is that this method is called whether or not the item is embedded in an actor, so you may want to use `this?.parent` to use the optional chaining and avoid errors.

### getRollData() and roll()

This next section deals with two linked functions - `getRollData`, which is overriding a method provided by the `Item` class, as well as `roll`, which we call in our Actor sheet's item listeners with the `.rollable` listener.

```js

  /**
   * Prepare a data object which defines the data schema used by dice roll commands against this Item
   * @override
   */
  getRollData() {
    // Starts off by populating the roll data with `this.system`
    const rollData = { ...super.getRollData() };

    // Quit early if there's no parent actor
    if (!this.actor) return rollData;

    // If present, add the actor's roll data
    rollData.actor = this.actor.getRollData();

    return rollData;
  }

  /**
   * Handle clickable rolls.
   * @param {Event} event   The originating click event
   * @private
   */
  async roll() {
    const item = this;

    // Initialize chat data.
    const speaker = ChatMessage.getSpeaker({ actor: this.actor });
    const rollMode = game.settings.get('core', 'rollMode');
    const label = `[${item.type}] ${item.name}`;

    // If there's no roll data, send a chat message.
    if (!this.system.formula) {
      ChatMessage.create({
        speaker: speaker,
        rollMode: rollMode,
        flavor: label,
        content: item.system.description ?? '',
      });
    }
    // Otherwise, create a roll and send a chat message from it.
    else {
      // Retrieve roll data.
      const rollData = this.getRollData();

      // Invoke the roll and submit it to chat.
      const roll = new Roll(rollData.formula, rollData);
      // If you need to store the value first, uncomment the next line.
      // const result = await roll.evaluate();
      roll.toMessage({
        speaker: speaker,
        rollMode: rollMode,
        flavor: label,
      });
      return roll;
    }
  }
```

In this setup, we've created a way to "roll" an item. `getRollData()` simply returns an object with relevant data we might want to use; `roll` then uses that data.

The second method, `roll`, is one we've defined to help us make rolls from our items. We start by defining some variables we'll need later to construct our chat message - `speaker`, `rollMode`, and then some text we're calling `label` but is passed as `flavor`. 

Our next trick is that if there ISN'T a formula to actually roll we just send a chat message with the item's description, but that may not be the behavior you want - it may be preferable to throw an error with `ui.notifications.warn("No roll formula")` or something along those lines instead.

Finally, we put together our roll. Rolls are not documents and are not stored in any database, so we can use ordinary Javascript class constructors to make our roll and then use its built-in functions. 

Finally, for anyone who might be using our system programatically (e.g. in a macro) we `return roll` so they have easy access to the results of what we've done.

---

* **Prev:** [Creating HTML templates for your actor sheets](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD08-Creating-HTML-templates-for-your-actor-sheets)
* **Next:** [Extending the ItemSheet class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD10-Extending-the-ItemSheet-class)