---
title: 3. Extending Dialog with Tabs
description: Create a tabbed version of the Dialog class using Handlebars and a template. 
published: true
date: 2021-06-19T17:08:52.350Z
tags: template, dialog, handlebars, html
editor: markdown
dateCreated: 2021-06-19T17:08:52.350Z
---

![](https://img.shields.io/badge/Foundry-v0.8.7-informational)

# Extend Dialog Class to utilize tabs 

This guide provides some examples of using html templates, tabs, and handlebars in Foundry VTT dialogs or sheets. In this guide, we will extend the Dialog class for a basic tabbed version.

If you have not yet, you might want to check out the [Template Basics guide](/en/development/guides/Tabs-and-Templates/Template-Basics) or the [Tabs and FormApplication guide](/en/development/guides/Tabs-and-Templates/Tabs-FormApplication).

# Dialog Basics

## Dialog HTML template
The original dialog HTML template looks like:
```html
<!--- dialog template --->

<div class="dialog-content">
    {{{content}}}
</div>
<div class="dialog-buttons">
    {{#each buttons as |button id|}}
    <button class="dialog-button {{cssClass}}" data-button="{{id}}">
        {{{button.icon}}}
        {{{button.label}}}
    </button>
    {{/each}}
</div>
```
Dialog displays whatever is in `content`. The `{{{...}}}` means that Handlebars will interpret `content` literally, without quoting. See [HTML Escaping](https://handlebarsjs.com/guide/#html-escaping). This permits `content` to have full html. Basically, Handlebars is just pasting in whatever string is in `content` sight-unseen. The catch is that Dialog has no mechanism for interpreting any Handlebars that might be in the `content` string—you have to pass it html only. 

Most of the real work of the Dialog Class is adding one or more buttons to the bottom. In the template, Handlebars is cycling through `buttons`. For each button provided to Dialog, the icon and the label of the button is displayed using an html `<button>` tag. 

## Dialog Class (Javascript)

Looking at the Dialog class definition (Foundry 0.8.6), we can see how options are added to reference the Dialog html template:

```js
class Dialog extends Application {

  /** @inheritdoc */
	static get defaultOptions() {
	  return foundry.utils.mergeObject(super.defaultOptions, {
	    template: "templates/hud/dialog.html",
      classes: ["dialog"],
      width: 400,
      jQuery: true
    });
  }
```

A lot of the rest of the magic of Dialog is just making the user define the buttons used in the template. For example, the Dialog class defines for us a basic yes/no prompt with two buttons:
```js
  /**
   * A helper factory method to create simple confirmation dialog windows which consist of simple yes/no prompts.
   * If you require more flexibility, a custom Dialog instance is preferred.
   *
   * @param {object} config             Confirmation dialog configuration
   * @param {string} config.title          The confirmation window title
   * @param {string} config.content        The confirmation message
   * @param {Function} [config.yes]        Callback function upon yes
   * @param {Function} [config.no]         Callback function upon no
   * @param {Function} [config.render]     A function to call when the dialog is rendered
   * @param {boolean} [config.defaultYes=true]  Make "yes" the default choice?
   * @param {boolean} [config.rejectClose=false] Reject the Promise if the Dialog is closed without making a choice.
   * @param {Object} [config.options={}]   Additional rendering options passed to the Dialog
   *
   * @return {Promise<*>}               A promise which resolves once the user makes a choice or closes the window
   *
   * @example
   * let d = Dialog.confirm({
   *  title: "A Yes or No Question",
   *  content: "<p>Choose wisely.</p>",
   *  yes: () => console.log("You chose ... wisely"),
   *  no: () => console.log("You chose ... poorly"),
   *  defaultYes: false
   * });
   */
  static async confirm({title, content, yes, no, render, defaultYes=true, rejectClose=false, options={}}={}) {
    return new Promise((resolve, reject) => {
      const dialog = new this({
        title: title,
        content: content,
        buttons: {
          yes: {
            icon: '<i class="fas fa-check"></i>',
            label: game.i18n.localize("Yes"),
            callback: html => {
              const result = yes ? yes(html) : true;
              resolve(result);
            }
          },
          no: {
            icon: '<i class="fas fa-times"></i>',
            label: game.i18n.localize("No"),
            callback: html => {
              const result = no ? no(html) : false;
              resolve(result);
            }
          }
        },
        default: defaultYes ? "yes" : "no",
        render: render,
        close: () => {
          if ( rejectClose ) reject("The confirmation Dialog was closed without a choice being made");
          else resolve(null);
        },
      }, options);
      dialog.render(true);
    });
  }
```

The Dialog class, as seen in the above example, also takes a bunch of functions (callbacks) that tell it what to do once either button is pushed, or if the user simply closes the dialog using the `X` button in the window. 


# Example: Tabbed Dialog Class

We are going to extend the Dialog class to allow for some basic tabs, following a similar structure to how the Dialog class lets the user define buttons. We assume that the user may want a fixed header across all tabs, and a fixed footer that includes any buttons defined by the user. The number of tabs will be defined by the user. 

## HTML
Save the following in the foundry Data folder. For this example, I am saving mine at `macro_data/TEMPLATE_FILE`, where `TEMPLATE_FILE` is something like `tab_template.html`.

```html
<div class="dialog-content">
<nav class="tabs" data-group="primary-tabs">
  {{#each tabs}}
  <a class="item" data-tab="{{id}}"><i class="{{icon}}"></i> {{title}}</a>
  {{/each}}
</nav>

<div class="dialog-header">
{{{header}}}
</div>

<section class="tab-content">
  {{#each tabs}}
  <div class="tab" data-tab="{{id}}" data-group="primary-tabs">
    {{{content}}}
  </div>
  {{/each}}
</section>

<div class="dialog-footer">
{{{footer}}}
</div>
</div>

<div class="dialog-buttons">
    {{#each buttons as |button id|}}
    <button class="dialog-button {{cssClass}}" data-button="{{id}}">
        {{{button.icon}}}
        {{{button.label}}}
    </button>
    {{/each}}
</div>
```

At the top, we have added a `<nav>` tag to tell Foundry that we have one or more tabs to navigate. Each of the tabs listed here corresponds to tabs in the `tab-content` below. Specifically, the `data-tab` ids match, as do the `data-group`. Note how we use `{{#each tabs}}` in both parts to cycle through the tabs provided.

The template is then split into the `{{{header}}}`, `{{{content}}}` per tab, `{{{footer}}}`, and buttons. We keep the same button html as in the original Dialog template.

## Javascript
Run this from a macro or in module code. Remember that you will need both the extended TabbedDialog class definition below as well as code from one of the below examples of TabbedDialog in action.

### Extend Dialog

First, extend the Dialog class. The main "trick" is telling Foundry, through the `options.tabs` array, that we would like tabs to be used. This is needed so that the base Application class will correctly activate the listener (see `activateListeners` in the Application class) that will allow us to click on different tabs. In order to allow the user to pass through a default initial tab to display without having to pass through all the other `options.tab` array information, we define `options.tabs` early, in the constructor.

In order to use the additional Handlebars in our template, we need to pass some additional information to Handlebars in `getData`. The `getData` function will then pass the data provided when rendering the html template. Here, we construct an array of tabs based on user input or reasonable defaults, along with providing the user-defined header and footer.

We also choose to increment tab ids from 1. This is so the user can specify the default tab as "tab1", "tab2", etc., instead of having to refer to "tab0".


```js
class TabbedDialog extends Dialog {
  constructor(data, options = {}) {  
    // setting up tabs here instead of in defaultOptions so that we can easily set the initial tab
    options.tabs = [{navSelector: ".tabs", contentSelector: ".tab-content", initial: options.initial_tab || "tab1"}];
    super(data, options)    
  }

  static get defaultOptions() {
    return mergeObject(super.defaultOptions, {
      template: "macro_data/tabbedDialogTemplate.html",
    });
  }
  
  getData() {
    console.log("getData", this);
    // no super to Application
    const data = super.getData();

    data.tabs = this.data.tabs.map((t, idx) => {
      return {
        id: t.id || `tab${idx + 1}`,
        title: t.title || `Tab ${idx + 1}`,
        icon: t.icon || "fas fa-dice-d20",
        content: t.content || ""
      }
    });
    
    data.header = this.data.header;
    data.footer = this.data.footer;
    
    console.log(data);
    
    return data;
  }
  
  // In Foundry 0.7.9 we would have needed to call Application.prototype.activateListeners directly. Can remove for 0.8.7.
  /* 
  activateListeners(html) {
    super.activateListeners(html);
    Application.prototype.activateListeners.call(this, html);  
  } 
  */     
}
```

For Foundry 0.7.9, what were we doing with activateListeners? `Dialog` class alone did not work with tabs, because tab switching did not work. So we called the Application activateListeners here as a work-around. We can ignore for Foundry 0.8.7, which switches tabs as expected without overriding `activateListeners`.

### Example: Three tabs, no content

Below is an example of a basic tabbed dialog, in which we ask for three tabs but provide no data about those tabs. This lets us see that our defaults are reasonable.

```js
let d = new TabbedDialog(
{
  title: "Test Tabbed Dialog",
  header: "Test <em>header</em>",
  footer: "Test <i>footer</i>",
  tabs: [ { },
          { },
          { }
        ],
  buttons: {
   one: {
    icon: '<i class="fas fa-check"></i>',
    label: "Option One",
    callback: () => console.log("Chose One")
   },
   two: {
    icon: '<i class="fas fa-times"></i>',
    label: "Option Two",
    callback: () => console.log("Chose Two")
   }
 },
 default: "two",
 render: html => console.log("Register interactivity in the rendered dialog"),
 close: html => console.log("This always is logged no matter which option is chosen")

},
{ resizable: true }

);

d.render(true);
```

### Example: Three tabs, varied options

Below is an example that is closer to what we might expect a user to provide. Three tabs with titles and varied content. Default to tab 3 initially. We also test that Handlebars is passing through html tab content.   

```js
let d = new TabbedDialog(
{
  title: "Test Tabbed Dialog",
  header: "Test <em>header</em>",
  footer: "Test <i>footer</i>",
  tabs: [ { title: "Tab 1",
            content: "Tab 1 content" },
          { title: "Tab 2",
            icon: "fas fa-cogs",
            content: "<b>Tab 2 content</b>" },
          { content: "<i>Tab 3 content</i>" }
        ],
  buttons: {
   one: {
    icon: '<i class="fas fa-check"></i>',
    label: "Option One",
    callback: () => console.log("Chose One")
   },
   two: {
    icon: '<i class="fas fa-times"></i>',
    label: "Option Two",
    callback: () => console.log("Chose Two")
   }
 },
 default: "two",
 render: html => console.log("Register interactivity in the rendered dialog"),
 close: html => console.log("This always is logged no matter which option is chosen")

},
{ resizable: true, initial_tab: "tab3" }

);

d.render(true);
```
