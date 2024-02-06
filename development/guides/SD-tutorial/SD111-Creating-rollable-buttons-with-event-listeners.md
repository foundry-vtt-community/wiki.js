---
title: 11.1 Creating rollable buttons with event listeners
description: 
published: true
date: 2024-02-06T05:55:09.145Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:36:24.667Z
---


To make things on your character sheet rollable, you'll need to add a class that you can listen for in your sheet's `activateListeners()` method. In the Boilerplate System, ability rolls have an example of this with a `rollable` class. Let's look at the ability modifier line from `actor-sheet.html`:

```handlebars
<span class="ability-mod rollable" data-roll="d20+@abilities.{{key}}.mod" data-label="{{ability.label}}">{{numberFormat ability.mod decimals=0 sign=true}}</span>
```

We've added a `rollable` class along with a few different attributes for `data-roll` and `data-label` that our listener can then use to create the rolls. These data attributes aren't strictly necessary - there's many ways to set things up with a mix of `dataset` properties like here as well as deriving in the javascript event handler.

> **The HTML Dataset property**
> The [MDN article on the dataset property](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset) is helpful for understanding how data is passed between the HTML and your javascript.
{.is-info}


The formula in this example includes `@abilities.{{key}}.mod`, which would compute as `@abilities.str.mod`, for example. This should follow the same format if you were to write out inline rolls in chat or in macros, because in the roll method we're about to make, we're passing it `this.actor.data.data` just like core does for those scenarios.

Finally, the `rollable` class also includes some lightweight CSS for it to turn the mouse into a pointer and add a red text shadow on hover or focus.

### activateListeners()

Next, let's take a look at `actor-sheet.js` where we've overridden the ActorSheet class. We need to add a new click listener inside our ActorSheet's `activateListeners()` method:


```js
// Rollable abilities.
html.on('click', '.rollable', this._onRoll.bind(this));
```

This will call a custom `_onRoll()` method on click, and the call to `bind` ensures that our context of `this` is preserved (otherwise the callback will change `this` to the clicked element). Because that method doesn't exist, we need to go down towards the end of our class and add a new method after the `_onItemCreate()` method:

```js

  /**
   * Handle clickable rolls.
   * @param {Event} event   The originating click event
   * @private
   */
  _onRoll(event) {
    event.preventDefault();
    const element = event.currentTarget;
    const dataset = element.dataset;

    // Handle item rolls.
    if (dataset.rollType) {
      if (dataset.rollType == 'item') {
        const itemId = element.closest('.item').dataset.itemId;
        const item = this.actor.items.get(itemId);
        if (item) return item.roll();
      }
    }

    // Handle rolls that supply the formula directly.
    if (dataset.roll) {
      let label = dataset.label ? `[ability] ${dataset.label}` : '';
      let roll = new Roll(dataset.roll, this.actor.getRollData());
      roll.toMessage({
        speaker: ChatMessage.getSpeaker({ actor: this.actor }),
        flavor: label,
        rollMode: game.settings.get('core', 'rollMode'),
      });
      return roll;
    }
  }
```

First, we're preventing other click behaviors (such as if this were an `<a>` tag), and then we're grabbing any data attributes that were included on that element. 

Our first scenario is when there's a `rollType` attribute, which right now is only used for `item` rolls. We navigate the DOM to find the item, fetch it, and then call the item's `roll` method.

Alternatively, if there is a `dataset.roll` attribute (which we only use if we're also going to provide the formula), we'll use that to build the roll. First thing is getting the label attribute if there is one.

To create a roll, we use `new Roll('formula')`, and we've already written out the formula in our Handlebars template. The second argument of Roll() is the object to use for its data, so in this case we want to pass along the actor's data attributes.

And with that we have everything we need to send it to chat. Creating a new roll doesn't actually roll it, but calling the `toMessage()` method will roll before sending that result to to chat. We want this to reference the actor token as the speaker for the chat message, so we set the speaker and flavor properties to match what we need.

And with that, we've successfully made rollable attributes!

---

* **Prev:** [Extending the ItemSheet class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD10-Extending-the-ItemSheet-class)
* **Next:** [Grouping items by type](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD113-Grouping-items-by-type)