---
title: 4. Tabs in AppV2
description: A short primer on adding tabs to an instance of an ApplicationV2
published: false
date: 2024-08-07T12:35:45.538Z
tags: appv2 tabs
editor: markdown
dateCreated: 2024-08-07T12:26:29.432Z
---

# Adding Tabs to AppV2
There are four elements necessary to add tabs to an instance of AppV2, e.g. an actor sheet.
1. A static PARTS object.
2. Data in the context 	(constructed in \_prepareContext) that describes each tab.
3. A \_preparePartContext that sets context.tab appropriately.
4. The standard tab handlebars template (included in 1.)

In the examples below, the system is represented by **my-system** and **MYSYS**. You'll need to change these to whatever your system is using.

## static PARTS
A static variable that has an entry for each separate slice of html that will be rendered by the app. This may include a page header, the standard tab template, and an entry for each tabbed page.

```js
  static PARTS = {
    header: {
      template: 'systems/my-system/templates/actor/header.hbs',
    },
    tabs: {
      // Foundry-provided generic template
      template: 'templates/generic/tab-navigation.hbs',
    },
    traits: {
      template: 'systems/my-system/templates/actor/traits.hbs',
      scrollable: [''],
    },
    aptitudes: {
      template: 'systems/my-system/templates/actor/aptitudes.hbs',
      scrollable: [''],
    },
  };
```

If you want the individual tabbed pages to be scrollable, add the scrollable property here.

Note, each part must generate a single html element, and the top level tag must include the data-group and data-tab attributes. It must also add `{{tab.cssClass}}` to the class of the html element and this is how the tab will be made active.
### Example part

```html
<section
  class='tab aptitudes {{tab.cssClass}}'
  data-group='primary'
  data-tab='aptitudes'
>
  <h1>Aptitudes</h1>
  
  {{#each this.aptitudes}} {{#if this.show}}
    <div class="aptitude-entry">
      <div class="aptitude-label">{{this.label}}</div>
      <div class="aptitude-trait">{{this.trait}}</div>
      <div class="aptitude-die">{{this.totalRanks}}d{{this.die}}</div>
    </div>
  {{/if}} {{/each}}

</section>
````

## The data for each tab

In your `\_prepareContext`, construct a tabs field.  This is an object with keys representing your tabs, where the values associated with the keys are objects that contain the configuration for the tab. 

```js
  context.tabs = {
    traits: {
      cssClass: '',
      group: tabGroup,
      id: 'traits',
      icon: '',
      label: 'MYSYS.tab.traits',
    },
    aptitudes: {
      cssClass: '',
      group: tabGroup,
      id: 'aptitudes',
      icon: '',
      label: 'MYSYS.tab.aptitudes',
    },
  }
```

This construction can be automated somwhat and you may wish to delegate this to a `\_getTabs` method.

```js
  async _prepareContext(options) {
    let context = await super._prepareContext(options);

    context = foundry.utils.mergeObject(context, {
      tabs: this._getTabs(options.parts),
    });

    return context;
  }
```

```js
  _getTabs(parts) {
    const tabGroup = 'primary';

    // Default tab for first time it's rendered this session
    if (!this.tabGroups[tabGroup]) this.tabGroups[tabGroup] = 'aptitudes';

    return parts.reduce((tabs, partId) => {
      const tab = {
        cssClass: '',
        group: tabGroup,
        // Matches tab property to
        id: '',
        // FontAwesome Icon, if you so choose
        icon: '',
        // Run through localization
        label: 'MYSYS.tab.',
      };

      switch (partId) {
        case 'header':
        case 'tabs':
          return tabs;
        case 'aptitudes':
          tab.id = 'aptitudes';
          tab.label += 'aptitudes';
          break;
        case 'traits':
          tab.id = 'traits';
          tab.label += 'traits';
          break;
        default:
      }

      // This is what turns on a single tab
      if (this.tabGroups[tabGroup] === tab.id) tab.cssClass = 'active';

      tabs[partId] = tab;
      return tabs;
    }, {});
  }
```

## The prepare context for each part (\_preparePartContext)
This operation is called as each part in the `static PARTS` objects is rendered. It sets `context.tab` appropriately. If you have data that only relates to a single tab in your application (e.g. an html field that needs enriched), you may wish to do that here, rather than in the main `\prepareContext` 

```js
  async _preparePartContext(partId, context) {
    switch (partId) {
      case 'aptitudes':
      case 'traits':
        context.tab = context.tabs[partId];
        break;
      default:
    }
    return context;
  }
```
