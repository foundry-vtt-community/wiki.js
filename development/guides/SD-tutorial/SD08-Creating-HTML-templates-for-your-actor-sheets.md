---
title: 08. Creating HTML templates for your actor sheets
description: 
published: true
date: 2024-02-20T02:50:43.411Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:36:05.581Z
---

![Foundry v11 Compatible](https://img.shields.io/badge/Foundry-v11%20Compatible-blue)

In addition to the JS classes that define them, actors and items also have HTML templates that define the structure of your character and item sheets. In the Boilerplate System, these are placed in `/templates/actor/` and `/templates/item/`. These paths are not discovered automatically; you have to define specify their full path in your ActorSheet and ItemSheet class' defaultOptions() method. Within those directories, Boilerplate includes sheets for specific document types (such as `actor-character-sheet.html` and `item-spell-sheet.html`) along with template partials like `parts/actor-features.html`. Using partials is optional, but it can be helpful for dividing up complicated templates into more manageable chunks.

HTML templates in Foundry use [Handlebars](https://handlebarsjs.com/guide/) for their templating engine. If you're using a code editor like [Visual Studio Code](https://code.visualstudio.com/), you can set the syntax highlighting mode to Handlebars and get useful color coding to help recognize the various statements and variables in your templates.

Let's start by taking a look at how the `actor-character-sheet.hbs` template is laid out at a high level.

```handlebars
<form class="{{cssClass}} {{actor.type}} flexcol" autocomplete="off">

    {{!-- Sheet Header --}}
    <header class="sheet-header">
        {{!-- Header stuff goes here --}}
        <img class="profile-img" src="{{actor.img}}" data-edit="img" title="{{actor.name}}" height="100" width="100"/>
        <div class="header-fields">
            <h1 class="charname"><input name="name" type="text" value="{{actor.name}}" placeholder="Name"/></h1>
            <div class="resources grid grid-2col">{{!-- resources here --}}</div>
            <div class="abilities grid grid-3col">{{!-- abilities here --}}</div>
        </div>
    </header>

    {{!-- Sheet Tab Navigation --}}
    <nav class="sheet-tabs tabs" data-group="primary">
        <a class="item" data-tab="description">Description</a>
        <a class="item" data-tab="items">Items</a>
    </nav>

    {{!-- Sheet Body --}}
    <section class="sheet-body">
		{{!-- Tab content goes here --}}
    </section>
</form>
```



And here's an annotated version of what that looks like:

![boilerplate character sheet annotated](https://mattsmithin.nyc3.digitaloceanspaces.com/assets/boilerplate-character.png)

## The `<form>`.

The element that surrounds our entire sheet is a `<form>` element with a variable that's outputting `{{cssClass}}` and `{{actor.type}}`. `cssClass` is a combined version of the array of CSS classes we made earlier in the actor-sheet.js file's defaultOptions() method. You can also include classes and other attributes directly in the template, as we did here with `flexcol`. The flexcol class is a helper class provided by Foundry that can be used to layout things vertically without having to write additional CSS. There are several of those included by Foundry, and the Boilerplate System also includes several for grids. Finally, `actor.type` is a variable defined in the actor sheet that lets us distinguish between different types of actors, like `character` and `npc`, which can be useful if your various sheets share some styling while needing variations for each of the different actor types.

> ### Basic sheet layout in Boilerplate System
>
> This system includes a handful of helper CSS classes to help you lay out your sheets if you're not comfortable diving into CSS fully. Those are:
>
> -   `flexcol`: Included by Foundry itself, this lays out the child elements of whatever element you place this on vertically.
> -   `flexrow`: Included by Foundry itself, this lays out the child elements of whatever element you place this on horizontally.
> -   `flex-center`: When used on something that's using flexrow or flexcol, this will center the items and text.
> -   `flex-between`: When used on something that's using flexrow or flexcol, this will attempt to place space between the items. Similar to "justify" in word processors.
> -   `flex-group-center`: Add a border, padding, and center all items.
> -   `flex-group-left`: Add a border, padding, and left align all items.
> -   `flex-group-right`: Add a border, padding, and right align all items.
> -   `grid`: When combined with the `grid-Ncol` classes, this will lay out child elements in a grid.
> -   `grid-Ncol`: Replace `N` with any number from 1-12, such as `grid-3col`. When combined with `grid`, this will layout child elements in a grid with a number of columns equal to the number specified.

## The `<header>`.

The header element is used to create the header section of the form where we put a small amount of important information about our character that should always be visible, like their name, hit points, and ability scores. This will vary heavily depending on your system's needs. Let's step through each section of the header:



```handlebars
    {{!-- Sheet Header --}}
    <header class="sheet-header">
        <img class="profile-img" src="{{actor.img}}" data-edit="img" title="{{actor.name}}" height="100" width="100"/>
        <div class="header-fields">
            <h1 class="charname"><input name="name" type="text" value="{{actor.name}}" placeholder="Name"/></h1>
            {{!-- ...continued... --}}
```



The first element in our header is the actor's image. Aside from adding additional classes or tweaking the height/weight values, your actor images should always follow this pattern if they need to be editable by foundry. We're printing out the path to the image with the `{{actor.img}}` variable, and the `data-edit="img"` tells Foundry that this is an image that should be editable on click.

> ### Variables in Handlebars
> If you're inside an actor-sheet template, the `actor` object will be available for printing things such as the actor name and actor image source with variables such as `actor.name` and `actor.img`. The `actor.system` object is also available at a more convenient `system` variable because of our work in `getData()`, so for properties that are unique to your system you can print values such as `{{system.health.value}}`.

### The resources grid

The next item in our `<div class="header-fields">` div is the resources grid.



```handlebars
	{{!-- ...continued... --}}
  <div class="resources grid grid-2col">
    {{!-- "flex-group-center" is also defined in the _grid.scss file
    and it will add a small amount of padding, a border, and will
    center all of its child elements content and text. --}}
    <div class="resource flex-group-center">
      <label for="system.health.value" class="resource-label">Health</label>
      <div class="resource-content flexrow flex-center flex-between">
      <input type="text" name="system.health.value" value="{{system.health.value}}" data-dtype="Number"/>
      <span> / </span>
      <input type="text" name="system.health.max" value="{{system.health.max}}" data-dtype="Number"/>
      </div>
    </div>

    <div class="resource flex-group-center">
      <label for="system.power.value" class="resource-label">Power</label>
      <div class="resource-content flexrow flex-center flex-between">
      <input type="text" name="system.power.value" value="{{system.power.value}}" data-dtype="Number"/>
      <span> / </span>
      <input type="text" name="system.power.max" value="{{system.power.max}}" data-dtype="Number"/>
      </div>
    </div>

  </div> {{!-- closes the "resources" div --}}
</div> {{!-- closes the "header-fields" div --}}

{{!-- ...continued... --}}
```



In this case, we have a container div with the classes `resources`, `grid`, and `grid-2col`. The later two classes are the ones driving the layout here; any elements immediately inside the resources div (or `resource` divs in this case) will be laid out as a two column grid. We're also using the `flex-group-center` class on the individual `resource` divs to add some padding, a border, and text centering on them.

Each resource has a few fields. There's a label with a special `resource-label` class that makes it bold and uppercase. We also have a `resource-content` div with several different flex classes on it to lay out its contents in a row.

Finally, we have our inputs. Text inputs in Foundry always follow this pattern:



```handlebars
<input type="text" name="system.health.value" value="{{system.health.value}}" data-dtype="Number" />
```



Here's what that's doing:

* **name**: This attribute is used to tell Foundry what field on the actor should be updated when changes are made. This should be plain text, so _don't_ put it inside `{{ variable }}` brackets.
* **value**: This is the current value of the input, and the one the user can change. Because this is the value, we need to put it in `{{ variable }}` brackets so that Foundry outputs the correct value for it.
* **data-dtype**: This is a special attribute that Foundry uses to validate the form input. There are several, but the most common ones are `Number` for numbers, `String` for text, and `Boolean` for true/false checkboxes.

### The abilities `<div>`.

The abilities div is very similar to resources, but in this case we have a group of related abilities that can be looped over.



```handlebars
          {{!-- ...continued... --}}
          <div class="abilities grid grid-3col">
            {{#each system.abilities as |ability key|}}
              <div class="ability flexrow flex-group-center">
                <label for="system.abilities.{{key}}.value" class="resource-label">{{key}}</label>
                <input type="text" name="system.abilities.{{key}}.value" value="{{ability.value}}" data-dtype="Number"/>
                <span class="ability-mod">{{numberFormat ability.mod decimals=0 sign=true}}</span>
              </div>
            {{/each}}
          </div>
        </div>
    </header>
    {{!-- ...continued... --}}
```



First, we're using the same `grid` classes as before, except now we're doing `grid-3col` for a three column layout.

The big difference is that we're running a loop with `{{#each}}`. Let's look at that statement in more detail:



```handlebars
{{#each system.abilities as |ability key|}}
  {{!-- stuff --}}
{{/each}}
```



An each block starts with `#each` and ends with `/each`. The first thing after the opening `{{#each` is the object or array we want to iterate over, or in this case `system.abilities`.

You can either iterate over just the values, which would be `system.abilities as ability` or over the values with the keys available as well, which is what we're doing with `system.abilities as |ability key|`. By doing that we can access the index/key using the `{{key}}` variable, which would evaluate to something like "str", "dex" or "con". The `{{ability}}` variable will give us the object for each ability, which will allow us to print out the current value with `{{ability.value}}`.

Here's what the actual ability input looks like:



```handlebars
<input type="text" name="system.abilities.{{key}}.value" value="{{ability.value}}" data-dtype="Number"/>
```



The first thing to notice is that **name** and **value** are much different than they were in our earlier example! The name attribute needs to be the exact the set of properties that Foundry would use to update the value, so we're mostly printing it out as plain text except for the part that would be "str" or "dex". For that we have to print out the current `{{key}}`. So in our loop we're using `system.abilities.{{key}}.value` for the name, which will get rendered as something like `system.abilities.str.value`.

The value is a bit different though. Because we already have the current ability object in the `{{ability}}` variable, we can just print out `{{ability.value}}` without worrying about the full `system.abilities` structure.

Finally, let's take a look at the ability modifier:



```handlebars
<span class="ability-mod">{{numberFormat ability.mod decimals=0 sign=true}}</span>
```



The user doesn't need to edit this, so we're just outputting the value as content inside a span element rather than the value on an input. Why is it a span? If you just need an element that doesn't have semantic meaning the way a `<p>` tag does for paragraphs, the `<div>` and `<span>` tags are good catchalls for grouping content. Div will typically stretch out and take up a whole row horizontally (unless you change that with CSS) while spans are inline and will only take up as much space as their text requires.

Something we're doing different for the modifier is that we're using the `numberFormat` Handlebars helper. This is a special helper that Foundry includes and it allows you to format numbers with `+` and `-` signs depending on their value, along with rounding decimals. In this case we're using numberFormat on `ability.mod`, which we derived earlier, and setting the decimals and `+/-` sign to true.

## The `<nav>` tabs.

The navigation (nav) element is used to create our sheet's tabs for navigation to multiple pages.



```handlebars
    {{!-- ...continued... --}}
    {{!-- Sheet Tab Navigation --}}
    <nav class="sheet-tabs tabs" data-group="primary">
        <a class="item" data-tab="description">Description</a>
        <a class="item" data-tab="items">Items</a>
    </nav>
    {{!-- ...continued... --}}
```



Tabs are created as a `<nav>` element with a `tabs` class and a `data-group`. The exact data-group name doesn't really matter, as long as it also matches up tab content divs we'll make later. For instance, you could make `data-group="primary"` for one set of tabs and `data-group="secondary"` for a completely unrelated set of tabs (or even nested tabs).

Each tab inside the name if an `<a>` link with a `data-tab` attribute. Much like `data-group`, the contents of `data-tab` don't matter as long as they exactly match a `data-tab` attribute on your tab content div that gets created later.

## The `<section>` body.

Finally, we also have a `<section>` element that's used to create the body of sheet where the various pages (which are navigated to via tabs) will live. This doesn't have to be a section element; it could have also been something else like a `<div>`, but sections can be used to tell the browser "this is a discreet chunk of related content", such as multiple paragraphs in a book that are all under the same headline.

Because we're using tabs on this sheet, each item inside sheet-body is a tab that also needs `data-group` and `data-tab` attributes. Here's the markup for that in the Boilerplate System:



```handlebars

  {{!-- Sheet Body --}}
  <section class="sheet-body">

    {{!-- Owned Features Tab --}}
    <div class="tab features" data-group="primary" data-tab="features">
      <section class="grid grid-3col">
        <aside class="sidebar">

          {{!-- The grid classes are defined in scss/global/_grid.scss. To use,
          use both the "grid" and "grid-Ncol" class where "N" can be any number
          from 1 to 12 and will create that number of columns.  --}}
          <div class="abilities flexcol">
            {{#each system.abilities as |ability key|}}
            <div class="ability flexrow flex-group-center">
              <label for="system.abilities.{{key}}.value" class="resource-label rollable flexlarge align-left" data-roll="d20+@abilities.{{key}}.mod" data-label="{{ability.label}}">{{ability.label}}</label>
              <input type="text" name="system.abilities.{{key}}.value" value="{{ability.value}}" data-dtype="Number"/>
              <span class="ability-mod rollable" data-roll="d20+@abilities.{{key}}.mod" data-label="{{ability.label}}">{{numberFormat ability.mod decimals=0 sign=true}}</span>
            </div>
            {{/each}}
          </div>
        </aside>

        {{!-- For the main features list, span the right two columns --}}
        <section class="main grid-span-2">
          {{!-- This is a Handlebars partial. They're stored in the `/parts` folder next to this sheet, and defined in module/helpers/templates.mjs --}}
          {{> "systems/boilerplate/templates/actor/parts/actor-features.hbs"}}
        </section>

      </section>
    </div>

    {{!-- Biography Tab --}}
    <div class="tab biography" data-group="primary" data-tab="description">
      {{!-- If you want TinyMCE editors to output inline rolls when rendered, you need to pass the actor's roll data to the rollData property. --}}
      {{editor system.biography target="system.biography" rollData=rollData button=true owner=owner editable=editable}}
    </div>

    {{!-- Owned Items Tab --}}
    <div class="tab items" data-group="primary" data-tab="items">
       {{> "systems/boilerplate/templates/actor/parts/actor-items.hbs"}}
    </div>

    {{!-- Owned Spells Tab --}}
    <div class="tab spells" data-group="primary" data-tab="spells">
      {{> "systems/boilerplate/templates/actor/parts/actor-spells.hbs"}}
    </div>

    {{!-- Active Effects Tab --}}
    <div class="tab effects flexcol" data-group="primary" data-tab="effects">
      {{> "systems/boilerplate/templates/actor/parts/actor-effects.hbs"}}
    </div>

  </section>
    {{!-- ...continued... --}}
```

Many of these tabs are stored in handlebar partials - this both reduces the sheer length of a given file to comprehensible levels as well as makes these tabs reusable on other sheets as needed. A handlebars partial looks like the following - note that the FULL path from the foundry `Data` folder must be provided.
```handlebars
{{> "systems/boilerplate/templates/actor/parts/actor-features.hbs"}}
```


### The Biography tab

Skipping ahead slightly, the biography tab in the Boilerplate System is both an example of how to make a tab and how to use a TinyMCE editor.

```handlebars
    {{!-- Biography Tab --}}
    <div class="tab biography" data-group="primary" data-tab="description">
      {{!-- If you want TinyMCE editors to output inline rolls when rendered, you need to pass the actor's roll data to the rollData property. --}}
      {{editor system.biography target="system.biography" rollData=rollData button=true owner=owner editable=editable}}
    </div>
```

First, we have the `tab` class to establish that this is a tab, and a `biography` class that we could use for more specific CSS styling if needed. The `data-group` attribute lets us associate with the primary tabs group, and the `data-tab` attribute tells Foundry that this is specifically the description tab (which we defined earlier in the `<nav>` element).

The `{{editor}}` helper is a special Handlebars helper that will return a TinyMCE text editor. You'll usually want to format it very similar to this example, substituting the `content` and `target` properties with whatever data property you want the editor to make changes to.

### The Owned Features Tab

Now going back, let's look at the Owned Features tab as an example of how to display an array of embedded documents:

```handlebars
    {{!-- Owned Features Tab --}}
    <div class="tab features" data-group="primary" data-tab="features">
      <section class="grid grid-3col">
        <aside class="sidebar">

          {{!-- The grid classes are defined in scss/global/_grid.scss. To use,
          use both the "grid" and "grid-Ncol" class where "N" can be any number
          from 1 to 12 and will create that number of columns.  --}}
          <div class="abilities flexcol">
            {{#each system.abilities as |ability key|}}
            <div class="ability flexrow flex-group-center">
              <label for="system.abilities.{{key}}.value" class="resource-label rollable flexlarge align-left" data-roll="d20+@abilities.{{key}}.mod" data-label="{{ability.label}}">{{ability.label}}</label>
              <input type="text" name="system.abilities.{{key}}.value" value="{{ability.value}}" data-dtype="Number"/>
              <span class="ability-mod rollable" data-roll="d20+@abilities.{{key}}.mod" data-label="{{ability.label}}">{{numberFormat ability.mod decimals=0 sign=true}}</span>
            </div>
            {{/each}}
          </div>
        </aside>

        {{!-- For the main features list, span the right two columns --}}
        <section class="main grid-span-2">
          {{!-- This is a Handlebars partial. They're stored in the `/parts` folder next to this sheet, and defined in module/helpers/templates.mjs --}}
          {{> "systems/boilerplate/templates/actor/parts/actor-features.hbs"}}
        </section>

      </section>
    </div>
```

One key thing here is the use of the `#each` [built-in handlebars helper](https://handlebarsjs.com/guide/builtin-helpers.html#each) - this allows you to loop through data, with the `as` keyword letting you assign variable names like a `for` loop. 

As for the main features list, the partial calls this template:

```handlebars
<ol class='items-list'>
  <li class='item flexrow items-header'>
    <div class='item-name'>{{localize 'Name'}}</div>
    <div class='item-controls'>
      <a
        class='item-control item-create'
        title='Create item'
        data-type='feature'
      >
        <i class='fas fa-plus'></i>
        {{localize 'DOCUMENT.New' type='feature'}}
      </a>
    </div>
  </li>
  {{#each features as |item id|}}
    <li class='item flexrow' data-item-id='{{item._id}}'>
      <div class='item-name'>
        <div class='item-image'>
          <a class='rollable' data-roll-type='item'>
          	<img
              src='{{item.img}}'
              title='{{item.name}}'
              width='24'
              height='24'
            />
           </a>
        </div>
        <h4>{{item.name}}</h4>
      </div>
      <div class='item-controls'>
        <a
          class='item-control item-edit'
          title='{{localize "DOCUMENT.Edit" type='feature'}}'
        >
          <i class='fas fa-edit'></i>
        </a>
        <a
          class='item-control item-delete'
          title='{{localize "DOCUMENT.Delete" type='feature'}}'
        >
          <i class='fas fa-trash'></i>
        </a>
      </div>
    </li>
  {{/each}}
</ol>
```

This creates a table-like view, with the first row setup as a header while the `#each` loops through to display the features (which we set up back in the `getData()` method of our ActorSheet).

For the most part, the `<li>` tags in this `#each` loop are very similar to the ones we made for the header row, but there are a few differences. First, we have a special `data-item-id="{{item._id}}"` attribute on the list item. Having this is very important to making your life easier, because by having it on the parent list item, any links that we make on one of the elements nested in the list item will be able to grab that ID for usage in our Javascript.

Next, we're outputting the `item.img` and `item.name` variables as needed. We're also using `<a>` tags to utilize native browser styling that gives us a pointer cursor when a user mouses over something that we want to be interactible, but the actual interaction is going to be handled in `activateListeners()`.

**Interactivity.** Notice that the controls have `item-edit` and `item-delete` classes, and the header has an `item-create` class? These classes have click listeners defined in `actor-sheet.js`. Let's step back to `actor-sheet.js` and take a look at the `activateListeners()` method:

```js

  /** @override */
  activateListeners(html) {
    super.activateListeners(html);

    // Render the item sheet for viewing/editing prior to the editable check.
    html.on('click', '.item-edit', (ev) => {
      const li = $(ev.currentTarget).parents('.item');
      const item = this.actor.items.get(li.data('itemId'));
      item.sheet.render(true);
    });

    // -------------------------------------------------------------
    // Everything below here is only needed if the sheet is editable
    if (!this.isEditable) return;

    // Add Inventory Item
    html.on('click', '.item-create', this._onItemCreate.bind(this));

    // Delete Inventory Item
    html.on('click', '.item-delete', (ev) => {
      const li = $(ev.currentTarget).parents('.item');
      const item = this.actor.items.get(li.data('itemId'));
      item.delete();
      li.slideUp(200, () => this.render(false));
    });

    // Active Effect management
    html.on('click', '.effect-control', (ev) => {
      const row = ev.currentTarget.closest('li');
      const document =
        row.dataset.parentId === this.actor.id
          ? this.actor
          : this.actor.items.get(row.dataset.parentId);
      onManageActiveEffect(ev, document);
    });

    // Rollable abilities.
    html.on('click', '.rollable', this._onRoll.bind(this));

    // Drag events for macros.
    if (this.actor.isOwner) {
      let handler = (ev) => this._onDragStart(ev);
      html.find('li.item').each((i, li) => {
        if (li.classList.contains('inventory-header')) return;
        li.setAttribute('draggable', true);
        li.addEventListener('dragstart', handler, false);
      });
    }
  }

```

What's happening here? First, we're activating any listeners that Foundry itself defines with `super.activateListeners(html)`. This includes making sure our input fields update the document, make our editors interactive, and enable any file pickers we have.

Our `item-edit` listener happens first because it works by rendering the item sheet, which would work for both users who can only view the item's sheet and owners who can actually edit it as well. To actually retrieve the item, we're finding the `<li>` tag based on the click event target and then reading the `item-id` data attribute (which gets converted to camel case `itemId` in this context). Once we have the item from the item ID, we can render its sheet.

Second, we're returning early if the sheet isn't editable (which usually means an observer has it open rather than the sheet owner).

Third, we're defining a click listener for `item-create`. We went into detail in the previous tutorial about what the `_onItemCreate()` method did, so we'll skip over that here.

Fourth, we're defining a click listener for `item-delete`. This is similar to `item-edit` from before. First we grab the element for the list item that's a parent of the button being clicked, which is what `$(ev.currentTarget).parents('.item')` does. Since we have the `data-item-id` attribute on the list items, we can than use the actor's methods to either get the owned item and render it's item sheet or delete the owned item, depending on which listener we're in. If we're deleting the item, we also do a small animation with `li.slideUp()` to animate getting rid of the item. Like most things in javascript, there's many ways to perform this kind of operation; when you look at other existing systems like `crucible`, `dnd5e`, or `swade` they may implement this kind of logic differently and with different accessors.

There are examples of additional listeners in here as well for more advanced usage.

- The active effect management uses the `onManageActiveEffect()` method from `helpers/effects.mjs`, which we'll get into a later topic. 
- The rollable abilities logic uses the `rollable` class we added to some parts of the sheet to make them clickable, and then passes that along to an `_onRoll()` method that's elsewhere in the sheet class.
- The drag events logic adds special `draggable` and `dragstart` attributes/listeners to items on the sheet so that they can be dragged and dropped to Foundry's macrobar. We'll investigate that in more detail in a later topic.

## Wrapping up

The rest of the sheet performs similar operations, leveraging the same listeners and just providing data to contextualize what is supposed to happen at each step.

> **Active Effect Display**
> As of v11, effects use the `name` attribute like items but their image is stored in the `icon` property. This will change in v12, which will unify their image property as `img` like other documents.
{.is-info}


---

* **Prev:** [Extending the ActorSheet class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD07-Extending-the-ActorSheet-class)
* **Next:** [Extending the Item class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD09-Extending-the-Item-class)