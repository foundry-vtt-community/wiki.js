---
title: 2. Extending FormApplication with Tabbed Template
description: Guide to extending FormApplication to use a tabbed template
published: true
date: 2021-06-19T22:02:02.682Z
tags: template, html, formapplication, tabs
editor: markdown
dateCreated: 2021-06-19T16:53:47.366Z
---

![](https://img.shields.io/badge/Foundry-v0.8.7-informational)

# Extend FormApplication Class using HTML Tabbed Templates

This guide provides some examples of using html templates, tabs, and handlebars in Foundry VTT dialogs or sheets. In this guide, we will extend the FormApplication class for a basic tabbed version.

See [Template Basics](/en/development/guides/Tabs-and-Templates/Template-Basics) for an overview of templates in Foundry VTT.

See [Understanding Form Applications](/en/development/guides/understanding-form-applications) for more details on how the FormApplication class works.

# Example: All the tabs
The following code creates a tabbed display. Using handlebars, we are able to dynamically set the number of tabs by passing an array of tab properties, one for each tab. 

## HTML
Save the following in the foundry Data folder. For this example, I am saving mine at `macro_data/TEMPLATE_FILE`, where `TEMPLATE_FILE` is something like `tab_template.html`.

```html
<form>
<nav class="tabs" data-group="primary-tabs">
  {{#each tabs}}
  <a class="item" data-tab="{{label}}"><i class="fas fa-dice-d20"></i> {{title}}</a>
  {{/each}}
</nav>

<b>Fixed header</b><br>
{{header}}
<hr>

<section class="content">
  {{#each tabs}}
  <div class="tab" data-tab="{{label}}" data-group="primary-tabs">
    Fixed content for tab {{title}}<br>
    {{{content}}}
  </div>
  {{/each}}
</section>

<hr>
Fixed <em>footer</em><br>
{{footer}}
</form>
```
## Javascript
Run this from a macro or in module code. Two options are provided below: 
1. Use renderTemplate.
2. Subclass FormApplication. 

### 1. Call `renderTemplate` and then do something with the resulting html. The example here uses a Dialog but you have many options:
```js
const template_file = "macro_data/TEMPLATE_FILE";
const template_data = { header: "Handlebars header text.",
                        tabs: [{ label: "Tab1",
                                 title: "My First Tab",
                                 content: "<em>Fancy tab1 content.</em>"},
                                 
                               { label: "Tab2",
                                 title: "My Second Tab",
                                 content: "<em>Fancy tab2 content.</em>"}],
                        footer: "Handlebars footer text."};
const rendered_html = await renderTemplate(template_file, template_data);
console.log(rendered_html); 

// Works as of Foundry 0.8.7; did not work in Foundry 0.7.9
let d = new Dialog({
    title: "MyDialogTitle",
    content: rendered_html,
    buttons: {
        toggle: {
            icon: '<i class="fas fa-check"></i>',
            label: "Okay",
            callback: () => console.log("Okay")
        },
    },
    default: "toggle",
    close: html => {
        console.log(html);
    },
  }, { tabs: [{ navSelector: ".tabs", contentSelector: ".content", initial: "tab1" }]} );
d.render(true);
```
Notes:
- Passing `{ tabs: [{ navSelector: ".tabs", contentSelector: ".content", initial: "tab1" }]}` is (basically) telling the Application class how to track when you select different tabs.
- See [Extending Dialog with Tabs](/en/development/guides/Tabs-and-Templates/Extending-Dialog-with-Tabs) for an example of extending the Dialog class.


### 2. Subclass `FormApplication`.
```js
class myFormApplication extends FormApplication {
  constructor(object, options) {
    super(object, options);  
  }

  static get defaultOptions() {
    return super.defaultOptions;
  }
  
  getData(options = {}) {
    return super.getData().object; // the object from the constructor is where we are storing the data 
  }
  
  activateListeners(html) {
    super.activateListeners(html);
  }
    
  async _updateObject(event, formData) {
    return;
  }
}

const template_file = "macro_data/TEMPLATE_FILE";
const template_data = { header: "Handlebars header text.",
                        tabs: [{ label: "tab1",
                                 title: "My First Tab",
                                 content: "<em>Fancy tab1 content.</em>"},
                                 
                               { label: "tab2",
                                 title: "My Second Tab",
                                 content: "<em>Fancy tab2 content.</em>"}],
                        footer: "Handlebars footer text."};
const my_form = new myFormApplication(template_data, { template: template_file,
                             tabs: [{navSelector: ".tabs", contentSelector: ".content", initial: "tab1"}] }); // data, options
const res = await my_form.render(true);
```
Note:
- Make sure the initial tab name is the same as the label for the tab you want.
- These options could be built into myFormApplication definition. 
- If you want buttons like with Dialog, you will need to add them yourself.

# Example: Handlebars with Partials

Instead of dynamically creating tab content, another option is to load one or more handlebars partials files for the tab content. For this example, we will use a single partial file but provide it dynamic content. Name the partial file whatever you want; here I use `tab_partial.html`.``

## HTML
Main file:
```html
<form>
<nav class="tabs" data-group="primary-tabs">
  {{#each tabs}}
  <a class="item" data-tab="{{label}}"><i class="fas fa-dice-d20"></i> {{title}}</a>
  {{/each}}
</nav>

<b>Fixed header</b><br>
{{header}}
<hr>

<section class="content">
  {{#each tabs}}
  <div class="tab" data-tab="{{label}}" data-group="primary-tabs">
    Fixed content for tab {{title}}<br>
    {{> 'macro_data/tab_partial.html' this}}
  </div>
  {{/each}}
</section>

<hr>
Fixed <em>footer</em><br>
{{footer}}
</form>

```
Note that we are providing the `tab_partial.html` file location above. Here is the html for that `tab_partial.html` file:
```html
Fixed tab content.<br>
This is tab {{title}}.<br>
{{content}}
```

## Javascript
```js
class myFormApplication extends FormApplication {
  constructor(object, options) {
    super(object, options);  
  }

  static get defaultOptions() {
    return super.defaultOptions;
  }
  
  getData(options = {}) {
    return super.getData().object; // the object from the constructor is where we are storing the data 
  }
  
  activateListeners(html) {
    super.activateListeners(html);
  }
    
  async _updateObject(event, formData) {
    return;
  }
}

const template_file = "macro_data/TEMPLATE_FILE";
loadTemplates(["macro_data/tab_partial.html"]);
const template_data = { header: "Handlebars header text.",
                        tabs: [{ label: "tab1",
                                 title: "My First Tab",
                                 content: "<em>Fancy tab1 content.</em>"},
                                 
                               { label: "tab2",
                                 title: "My Second Tab",
                                 content: "<em>Fancy tab2 content.</em>"}],
                        footer: "Handlebars footer text."};
const my_form = new myFormApplication(template_data, { template: template_file,
                             tabs: [{navSelector: ".tabs", contentSelector: ".content", initial: "tab1"}] }); // data, options
const res = await my_form.render(true);
```

Notes:
- You must load the partial template either in your main code or when instantiating the class. 
- The `<form>` tag is important. 

# Example: Search Tab
This is an example of a tab that relies on a `<script>` tag. Here, we define a function to filter through a table by a specific column in that table. (Useful for lists of items, tokens, etc.) We will use partials and create a special partial for the search table, named `tab_search_partial.html`. 

The following is overkill, in that it dynamically chooses the search tag with handlebars (this allows us to see how such if/then switches might work). The simpler version would just define the tabs in advance with no dynamic switching. 

## HTML
Main file:
```html
<form>
<nav class="tabs" data-group="primary-tabs">
  {{#each tabs}}
  <a class="item" data-tab="{{label}}"><i class="fas fa-dice-d20"></i> {{title}}</a>
  {{/each}}
</nav>

<b>Fixed header</b><br>
{{header}}
<hr>

<section class="content">
  {{#each tabs}}
  Fixed content for tab {{title}}<br>
  <div class="tab" data-tab="{{label}}" data-group="primary-tabs">
    {{#if search}}
      {{> 'macro_data/tab_search_partial.html' this}}
    {{else}}
      {{> 'macro_data/tab_partial.html' this}}
    {{/if}}
  </div>
  {{/each}}
</section>

<hr>
Fixed <em>footer</em><br>
{{footer}}
</form>

```
Partial:
```html
Fixed tab content.<br>
This is tab {{title}}.<br>
{{{content}}}
```

Search partial:
```html
Fixed tab content.<br>
This is tab {{title}}.<br>
{{{content}}}
<input type="text" id="filter_field" onkeyup="filterFn()" placeholder="{{search_text}}">
<table id="list_table" class="table table-striped">
<tbody>
  {{#each rows}}
  <tr class="item-row">
    {{#each columns}}
      <td> {{{this}}} </td>
    {{/each}}
  </tr>
  {{/each}}
</tbody>
</table>

<script>

function filterFn() {
        const input = document.getElementById("filter_field");
        const filter = input.value.toUpperCase();
        const table = document.getElementById("list_table");
        let tr = table.getElementsByTagName("tr");

        // Loop through all table rows, and hide those who don't match the search query
        for (let i = 0; i < tr.length; i++) {
                const td = tr[i].getElementsByTagName("td")[0]; // column to search
                if (td) {
                        const txtValue = td.textContent || td.innerText;
                        if (txtValue.toUpperCase().indexOf(filter) > -1) {
                                tr[i].style.display = "";
                        } else {
                                tr[i].style.display = "none";
                        }
                }
        }
}

</script>

<style type="text/css">
  img { border-style: none; }
</style>
```
Notes:
- `filterFn` searches through a single column, as noted in the above code. So make sure you have chosen the correct column given the data you are using (or create a more advanced version to search multiple columns)
- The css style code is simply to remove the border around any images. See below image example.
- We anticipate passing html code as each row. For example, to provide an image. Thus `{{{this}}}` uses triple-moustache.
- `{{{this}}}` refers to the entire column object contained in the columns array. 


## Javascript
```js
class myFormApplication extends FormApplication {
  constructor(object, options) {
    super(object, options);  
  }

  static get defaultOptions() {
    return super.defaultOptions;
  }
  
  getData(options = {}) {
    return super.getData().object; // the object from the constructor is where we are storing the data 
  }
  
  activateListeners(html) {
    super.activateListeners(html);
  }
    
  async _updateObject(event, formData) {
    return;
  }
}

const template_file = "macro_data/TEMPLATE_FILE";
loadTemplates(["macro_data/tab_partial.html"]);
loadTemplates(["macro_data/tab_search_partial.html"]);

// build up some content for the search table
let rows = [];
let columns = [];

const row_names = ["Acid", "Angel", "Barrel"];
const images = ["icons/svg/acid.svg",
                "icons/svg/angel.svg",
                "icons/svg/barrel.svg"];

for(let r = 0; r < 3; r++) {
  columns = [];
  columns.push(row_names[r]); // We set filterFn above to search the first column, so this first column should be the name
  columns.push(`<img src="${images[r]}" width="30" height="30" />`);
  columns.push(`<input type="checkbox" id="row${r}" class="GroupSelection"/>`);
  rows.push({columns: columns});
}

console.log(rows);

const template_data = { header: "Handlebars header text.",
                        tabs: [{ label: "tab1",
                                 title: "My First Tab",
                                 content: "<em>Fancy tab1 content.</em>"},
                                 
                               { label: "tab2",
                                 title: "My Second Tab",
                                 content: "<em>Fancy tab2 content.</em>",
                                 search: true,
                                 search_text: "Search by name...",
                                 rows: rows}],
                        footer: "Handlebars footer text."};
const my_form = new myFormApplication(template_data, { template: template_file,
                             tabs: [{navSelector: ".tabs", contentSelector: ".content", initial: "tab1"}],
                             resizable: true }); // data, options
const res = await my_form.render(true);
```
