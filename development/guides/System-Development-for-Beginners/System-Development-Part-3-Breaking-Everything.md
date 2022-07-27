---
title: System Development-Part-3-Breaking-Everything
description: 
published: true
date: 2022-05-19T13:19:58.699Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:37:01.635Z
---

![](https://lh5.googleusercontent.com/Jbky61hs2_yrsXWmf7T5dve6BR7Xh9IeukK60zOHZdQjQd7dWAIvSBmwEvND7hVWRiXDfGyL8AWzJ67Xhn5n1mg2GDXCDbFLrsluzRNntUF2wa_msKcGyPlzPtqEmmRwOq2BPVIY)

  
  

Now that we know Foundry can see our system and we’ve got a development world created, we can start the preliminaries of building our character sheet. It is best to do these steps with a copy of a character sheet for your system in front of you, so if you’re planning on making Paranoia, best get your clone sheet ready.

  

## 3a. Open both the template.json and the ./templates/actor-sheet.html files in your editor.

(in VSCode you can conveniently right click one of these files at the top of the screen after opening it and choose ‘split right’ for a split-screen view.)

![](https://lh3.googleusercontent.com/IUym1frwBawJe8KKZferP2Bq26is2DkHeCJCzl6VX4KOn8eOoBYN6I_rxqgS6XEvEpsxx-GjJkNKgfWNzODljXrCNHL5_y_HKTz0zNyLYmCc0ispIudkU4tmXpC7GS2E861X6bAh)

  

The window on the left is our template.json file containing all our data-to-be-used, the window on our right is an html file that uses that data.

  
  

## 3b. Read and understand some HTML

Some of you will already be able to piece together what’s happening here, but for the sake of thoroughness, we’ll look closer at lines 7, 11, and 13 in our html file.

Line 7:
```html
<h1 class="charname">
    <input name="name" type="text" value="{{data.name}}" placeholder="Name" />
</h1>
```

Line 11:
```html
<input type="number" name="data.health.value" value="{{systemData.health.value}}"/>
```

Line 13:
```html
<input type="number" name="data.health.max" value="{{systemData.health.max}}"/>
```

Each of these lines creates an `<input>` text box on our character sheet, and each one is linked to something different. Greatly simplified: whenever Foundry makes a character, it creates a database object for that character called actor and gives actor a sub-entry called data that contains everything from our template.json file that we told it an actor needs.

  

Foundry comes with some predefined Handlebars. These are effectively shortcuts we can use that tell Foundry to read `{{data.health.value}}` from data related to the actor.

  

In short: whenever you see something wrapped in `{{}}` in html for Foundry, it’s a shortcut that (likely) references some data from a JSON object.

  

We can use this exact structure to make our own entries. Name always refers to the path to where we want our data  stored. Value always refers to the actual information we’re storing.

  
So if our maximum health is 10, that information is stored at `data.health.max`, which in our template looks like this:

```json
"health": {
  "value": 10,
  "min": 0,
  "max": 10
},
```

But we don’t get the value to show up in the box unless we tell Foundry to give us the value using `{{data.health.max}}`.

  

**Name** tells Foundry where to store the information.

**Value** tells Foundry to get the information for us.

  
  
  
  

## 3c. Break Everything

Now that we can see how foundry references our data, let’s give it some data to reference. The “Actor” section of our template.json contains only one type, which suits us just fine. We only care about characters and only really want to make a character sheet anyway.

  

We don’t plan on using the freeform attributes system from foundry, so let’s get rid of `attributes` and `groups`.

  
  

Before

```json
"character": {
  "biography": "",
  "health": {...},
  "power": {...},
  "attributes": {},
  "groups": {}
}
```

After

```json
"character": {
  "biography": "",
  "health": {...},
  "power": {...}
}
```

A quick detour

But, since we have a bunch of stuff that references that data, we need to make sure that we’re not just getting rid of it in the data but the HTML as well. If we don’t, we won’t be able to open the character sheet because of errors telling us that Foundry can’t find some data it was told to look for.

  

So in our **actor-sheet.html** we’ll just heartlessly delete the really nice free-form attributes system that Atropos made for us so we can put in our own boring text input boxes.

  

Before

```handlebars
{{!-- Attributes Tab --}}
<div class="tab attributes" data-group="primary" data-tab="attributes">
    <header class="attributes-header flexrow">
        <span class="attribute-key">{{localize "SIMPLE.AttributeKey"}}</span>
        <span class="attribute-value">{{localize "SIMPLE.AttributeValue"}}</span>
        <span class="attribute-label">{{localize "SIMPLE.AttributeLabel"}}</span>
        <span class="attribute-dtype">{{localize "SIMPLE.AttributeDtype"}}</span>
        <a class="attribute-control" data-action="create" data-group="{{group}}"><i class="fas fa-plus"></i></a>
    </header>

    {{!-- Render the attribute list partial. --}}
    {{> "systems/worldbuilding/templates/parts/sheet-attributes.html" attributes=systemData.ungroupedAttributes dtypes=dtypes}}

    {{!-- Render the grouped attributes partial and control. --}}
    <div class="groups">
        {{> "systems/worldbuilding/templates/parts/sheet-groups.html" attributes=systemData.groupedAttributes groups=systemData.groups dtypes=dtypes}}

        <div class="group-controls flexrow">
            <input class="group-prefix" type="text" value=""/>
            <a class="button group-control" data-action="create-group"><i class="fas fa-plus"></i>Add Attribute Group</a>
        </div>
    </div>
</div>
```

After

```handlebars
{{!-- Attributes Tab --}}
<div class="tab attributes" data-group="primary" data-tab="attributes">
    <header class="attributes-header flexrow">
        <span class="attribute-key">{{localize "SIMPLE.AttributeKey"}}</span>
        <span class="attribute-value">{{localize "SIMPLE.AttributeValue"}}</span>
        <span class="attribute-label">{{localize "SIMPLE.AttributeLabel"}}</span>
        <span class="attribute-dtype">{{localize "SIMPLE.AttributeDtype"}}</span>
        <a class="attribute-control" data-action="create" data-group="{{group}}"><i class="fas fa-plus"></i></a>
    </header>
</div>
```
  

(yes, we could remove the header from the attributes tab as well if we wanted to.)

  

## 3d. Prepare to clean up code

Open **./module/actor-sheet.js** and take a minute to convince yourself that JavaScript isn’t overwhelming and that you won’t immediately break everything in existence by editing this file.

  

Now we’re going to delete chunks out of this file arbitrarily until our character sheet works again.

## 3e. There can’t be a problem in the code if we delete the code

Let’s start with anything referencing attributes. Inside this function there's a helper function call that that reads each one of our data.attributes and turns anything that’s a checkbox into a boolean… whatever the hell that is. Let’s get rid of that.

  
Before
```js
getData() {
  const context = super.getData();
  EntitySheetHelper.getAttributeData(context.data);
  context.shorthand = !!game.settings.get("worldbuilding", "macroShorthand");
  context.systemData = context.data.data;
  context.dtypes = ATTRIBUTE_TYPES;
  return context;
}
```
  
After
```js
getData() {
  const context = super.getData();
  context.shorthand = !!game.settings.get("worldbuilding", "macroShorthand");
  context.systemData = context.data.data;
  context.dtypes = ATTRIBUTE_TYPES;
  return context;
}
```
  
  

But wait, we’re not done here. In the `activateListeners` function there is some code to handle click events for creating/editing/deleting attributes. Since we don't have those buttons anymore, let's delete the code that does that. Find the following lines in that function and delete them (starting at line 44):

```js
// Attribute Management
html.find(".attributes").on("click", ".attribute-control", EntitySheetHelper.onClickAttributeControl.bind(this));
html.find(".groups").on("click", ".group-control", EntitySheetHelper.onClickAttributeGroupControl.bind(this));
html.find(".attributes").on("click", "a.attribute-roll", EntitySheetHelper.onAttributeRoll.bind(this));
```

## 3f. One last thing

While we’re in here, the character sheet is still pointing at the worldbuilding system. If you've wondered why you can't see your changes yet, this is why So let’s fix that real quick. In the `defaultOptions` function on line 14:

Before
```js
template: "systems/worldbuilding/templates/actor-sheet.html",
```

After
```js
template: "systems/yoursystemname/templates/actor-sheet.html",
```      

Now that we’ve given the Simple Worldbuilding System a bit of a lobotomy, we can get back to creating our sheet.
