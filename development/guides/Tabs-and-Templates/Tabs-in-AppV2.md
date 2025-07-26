---
title: 4. Tabs in AppV2
description: A short primer on adding tabs to an instance of an ApplicationV2
published: true
date: 2025-07-26T16:00:48.782Z
tags: tabs, appv2
editor: markdown
dateCreated: 2024-08-07T12:26:29.432Z
---

# Adding Tabs to AppV2
There are four elements necessary to add tabs to an instance of AppV2, e.g. an actor sheet.
1. A static PARTS object.
2. A static TABS object.
2. Data in the context 	(constructed in `_prepareContext`) that describes each tab.
3. A `_preparePartContext` that sets context.tab appropriately.
4. The standard tab handlebars template (included in 1.)

In the examples below, the system is represented by **my-system** and **MYSYS**. You'll need to change these to whatever your system is using. This short example will have two sections that are always rendered and two tabs (traits and aptitudes), only one of which will be selected and visible at any given time

## static PARTS
A static variable that has an entry for each separate slice of html that will be rendered by the app. This may include a page header, the standard tab template, and an entry for each tabbed page.

```js
  /** @typedef {import("@client/applications/api/handlebars-application.mjs").HandlebarsTemplatePart} HandlebarsTemplatePart */
  /** @type {Record<string, HandlebarsTemplatePart>} */
  static PARTS = {
    header: {
      template: 'systems/my-system/templates/actor/header.hbs',
    },
    tabs: {
      // Foundry-provided generic template
      template: 'templates/generic/tab-navigation.hbs',
      // classes: ['sysclass'], // Optionally add extra classes to the part for extra customization
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

Note, each part must generate a single html element. The top level tag must include the data-group, & data-tab attributes, it must also add `{{tab.cssClass}}` to the class attribute of the html element as this is how the tab will be made active.
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

## static TABS

A static variable that holds the configurations for each tab group for the sheet.  This includes the tabs themselves as well as supplemental data.

```js
  /** @type {Record<string, foundry.applications.types.ApplicationTabsConfiguration>} */
  static TABS = {
    primary: {
      tabs: [{ id: "traits" }, { id: "aptitudes" }],
      labelPrefix: "MYSYS.tab", // Optional. Prepended to the id to generate a localization key
      initial: "traits", // Set the initial tab
    },
  };
```

Tabs is an array of objects of the basic tab data.  Only `id` is required if `labelPrefix` is set.  If `labelPrefix` is not set, then one of `icon` or `label` are required. The value of label will be localized.

```ts
type Tab = {
  id: string;
  icon?: string; // CSS classes for a FontAwesome icon
  label?: string; // Tab label.  Localized
  tooltip?: string; // Tooltip text when hovered.  Localized
  cssClass?: string;
};
```

This data is used by `ApplicationV2#_prepareTabs` to generate the context data used for rendering the the tabs template and for setting the initial render state for the tabs.

## The data for each tab

In your `_prepareContext`, assign the output of `this._prepareTabs(tabGroup)` to `context.tabs`.  This is an object where the keys are the tab ids and the values are an ApplicationTab object.  

```js
async _prepareContext(options) {
  const context = {
    // ...
    /** @type {Record<string, foundry.applications.types.ApplicationTab} */
    tabs: this._prepareTabs("primary"),
  };
  return context;
}
```

```js
// Available in jsdoc via {foundry.applications.types.ApplicationTab}
/**
 * @typedef ApplicationTab
 * @property {string}  id
 * @property {string}  group
 * @property {boolean} active
 * @property {string}  cssClass
 * @property {string}  [label]
 * @property {string}  [icon]
 * @property {string}  [tooltip]
 */
```

## The prepare context for each part (`_preparePartContext`)
This operation is called as each part in the `static PARTS` objects is rendered. It sets `context.tab` appropriately. If you have data that only relates to a single tab in your application (e.g. an html field that needs enriched), you may wish to do that here, rather than in the main `_prepareContext` 

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
