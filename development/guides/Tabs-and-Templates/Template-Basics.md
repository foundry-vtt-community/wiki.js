---
title: 1. Template Basics
description: Basic guide to using HTML templates in FoundryVTT. 
published: true
date: 2021-09-01T02:41:35.137Z
tags: template, handlebars, html, formapplication
editor: markdown
dateCreated: 2021-06-19T16:20:49.323Z
---

![](https://img.shields.io/badge/Foundry-v0.8.7-informational)

# Using HTML Templates in FoundryVTT

This guide provides some examples of using html templates and handlebars in Foundry VTT dialogs or sheets. 

# What the what?

## What are templates?
Templates are just html files that can be loaded into Foundry, in lieu of hard-coding all the html within your javascript macro, module, or system.

Examples:
- [Midi-Qol templates](https://gitlab.com/tposney/midi-qol/-/tree/master/templates)
- [VanceCole template example](https://github.com/VanceCole/macros/blob/master/handlebars-templates.js)

## What are handlebars?
[Handlebars Guide](https://handlebarsjs.com/guide/#what-is-handlebars)

# Setup

## Storing templates
Create in your Data directory a folder to store templates. Don't call it templates, which appears to confuse the caching mechanism. I called mine `macro_data`.

Store the below html template in `macro_data`. 

You are only allowed to load template files with an extension in [html,handlebars,hbs,vue].

## Loading templates
Somewhere in your code, you will need to load the templates before using them. 
```js
const template_file = "macro_data/TEMPLATE_FILE"
loadTemplates([template_file]);
```

## Resetting the cache
You will need to either restart your game session to pick up changes to templates, or you can force Foundry to re-load the cache. 
```js
const template_file = "macro_data/TEMPLATE_FILE"
delete _templateCache[template_file];
```

## Javascript options
You have various options to use templates in javascript within Foundry. These should function for macros, modules, or systems.

1. Call `renderTemplate`, passing the template and the data. Then do something with the resulting html.
```js
const data_object = {}; // data object to pass to the template 
const template_file = "macro_data/TEMPLATE_FILE"; // file path for the template file, from Data directory
const rendered_html = renderTemplate("macro_data/TEMPLATE_FILE", data_object);
```
Notes:
- `renderTemplate` calls getTemplate to retrieve a template from the server. 
- `getTemplate` in turn compiles the html template using `Handlebars.compile`
- `getTemplate` caches the compiled result from `Handlebars.compile`.
- `getTemplate` registers the path and compiled result as a handlebars partial.
- `renderTemplate` applies the data to the compiled result to return rendered html.
- If you want to delay rendering the data, use `getTemplate`. 

2. Subclass `FormApplication`. You will need to define several methods.
```js
class SelectItemDialog extends FormApplication {
  constructor(object, options) {
    super(object, options)  
  }


  static get defaultOptions() {
    return mergeObject(super.defaultOptions, {
      template: "macro_data/TEMPLATE_FILE"
    });
  }
  
  getData(options = {}) {
    return super.getData();
  }
  
  activateListeners(html) {
    super.activateListeners(html);
  }
    
  async _updateObject(event, formData) {
    return;
  }
}
```

# Example: Basic Template 

Remember, save the following in the foundry Data folder. For this example, I am saving mine at `macro_data/TEMPLATE_FILE`, where `TEMPLATE_FILE` is something like `basic_template.html`.

## HTML
```html
<form>
<b>Fixed header</b><br>
{{header}}
<hr>
Fixed content<br>
{{{content}}}
<hr>
Fixed <em>footer</em><br>
{{footer}}
</form>
```
Notes: 
- Need one and only one `<form>` tag. 
- Use `{{...}}` to pass data; use `{{{...}}}` to pass unescaped data (for example, a string variable with html code).
- Everything between the `<form>` tag is up to you.
- See [Handlebars Guide](https://handlebarsjs.com/guide/#what-is-handlebars) for more advanced handlebars code.

## Javascript Options
### 1. Call `renderTemplate` and then do something with the resulting html
```js
const template_file = "macro_data/TEMPLATE_FILE";
const template_data = { header: "Handlebars header text.",
                        content: "<em>Handlebars</em> <i>content</i>.",
                        footer: "Handlebars footer text."};
const rendered_html = await renderTemplate(template_file, template_data);
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
  });
d.render(true);
```
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
                        content: "<em>Handlebars</em> <i>content</i>.",
                        footer: "Handlebars footer text."};
const my_form = new myFormApplication(template_data, { template: template_file }); // data, options
const res = await my_form.render(true);
```

If you want to resize the dialog box, you can either hard-code options into your class or pass options dynamically. For example, to have resizable width, replace the my_form definition in the above with the following:
```js
const my_form = new myFormApplication(template_data, { template: template_file,
                                                        width: "400",
                                                        height: "auto",
                                                        resizable: true }); 
```