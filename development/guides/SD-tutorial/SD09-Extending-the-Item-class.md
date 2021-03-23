---
title: SD09 Extending-the-Item-class
description: 
published: true
date: 2020-12-20T21:53:27.298Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:36:12.667Z
---

You can extend the Item class to use your own version, just like we did earlier with the Actor class. Let's start by taking a look a the BoilerplateItem class in <!-- {% raw %} -->`/module/item/item.js`<!-- {% endraw %} -->. As with previous examples, you'll want to rename <!-- {% raw %} -->`BoilerplateItem`<!-- {% endraw %} --> to whatever your system's name is, such as <!-- {% raw %} -->`MySystemNameItem`<!-- {% endraw %} -->.

<!--- {% raw %} --->

```js
/**
 * Extend the basic Item with some very simple modifications.
 * @extends {Item}
 */
export class BoilerplateItem extends Item {
  /**
   * Augment the basic Item data model with additional dynamic data.
   */
  prepareData() {
    super.prepareData();

    // Get the Item's data
    const itemData = this.data;
    const actorData = this.actor ? this.actor.data : {};
    const data = itemData.data;
  }
}
```

<!--- {% endraw %} --->

The Boilerplate system doesn't actually include any other modifications to its Item class, but you could add derived data here, as we did in the Actor class. In addition, you can define methods that can be called when needed, such as a <!-- {% raw %} -->`toRoll()`<!-- {% endraw %} --> method to make a roll from an item and send it to chat. We'll revisit that idea later when we add Macrobar support to items.

---

* **Prev:** [Creating HTML templates for your actor sheets](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD08-Creating-HTML-templates-for-your-actor-sheets)
* **Next:** [Extending the ItemSheet class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD10-Extending-the-ItemSheet-class)