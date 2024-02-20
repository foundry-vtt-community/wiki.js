---
title: 12. Localization
description: 
published: true
date: 2024-02-20T02:53:25.440Z
tags: 
editor: markdown
dateCreated: 2020-12-20T22:06:29.592Z
---

![Foundry v11 Compatible](https://img.shields.io/badge/Foundry-v11%20Compatible-blue)

Supporting localization in your new system is critical to allow the maximum number of GMs and players to use your system. Localization provides others with the ability to translate your work into their language. It is easier to build into a system from the start when compared to coming back and adding it. Foundry uses the browser's language settings to determine which localization to use when displaying text. 

> For more detailed information on localization, head to the [Localization Best Practices](/en/development/guides/localization/localization-best-practices) page.
{.is-info}


## What do I localize?
Every string that you display to a user should be localizable. A general rule is to allow localization of any strings that you display in your system. You don't need to localize text that the GM or players enter in Foundry. Typical areas of localization are:
- Headings 
- Body text
- Form labels
- Button labels
- Dialog box titles
- Error messages
## Adding Languages to system.json
Adding the following code to your system.json file tells Foundry that there is an English language file in the folder lang called en.json. 
```json
"languages": [
    {
     "lang": "en",
      "name": "English",
      "path": "lang/en.json"
    }
  ]
```
The lang variable is the two-letter [ISO 639-1 code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) for the language that the path file is supporting.  
Let's tell Foundry about a Spanish language file along with our English language file. Add the following object code to the languages array in your system.json file. 
```json
{
  "lang":"es",
  "name":"Spanish",
  "path":"lang/es.json"
}
```
## Language JSON file
The language file is a simple single-level JSON object. Each string has a variable name and the string that Foundry will display when you tell it to translate the variable. In the example en.json file below, we have the variable MYSYSTEM.ActorName which has the value Name in English
```json
{
  "MYSYSTEM.ActorName": "Actor's Name"
}
```

You can also create nested properties, or even mix and match as you prefer. The above would be equivalent to

```json
{
  "MYSYSTEM": {
    "ActorName": "Actor's Name"
  }
}
```

One common localization need is document types in their create dialog. These paths are defined by foundry; the boilerplate system comes with the following translations, but these should be adjusted whenever you add or remove types from your `template.json`

```json
  "TYPES": {
    "Actor": {
      "character": "Character",
      "npc": "NPC"
    },
    "Item": {
      "item": "Item",
      "feature": "Feature",
      "spell": "Spell"
    }
  }
```

## HTML localization using Handlebars
Foundry uses a Handlebar `{{localize "variable.name"}}` to translate text in HTML files. Let's replace the placeholder for the actor's name in the actor-sheet.html file with a localization. In actor-sheet.html replace `placeholder="name"` with `placeholder="{{localize 'MYSYSTEM.ActorName'}}"`. Your code should look like:
```html
<h1 class="charname"><input name="name" type="text" value="{{actor.name}}" placeholder="{{localize 'MYSYSTEM.ActorName'}}"/></h1>
```
When you create a new character, you will now see Actor's Name in the Name field instead of Name. 

> **Mind your quotes**
> One common error when using localized property values is incorrectly handling nested quotes; you'll need to alternate between " and ' for your handlebars to parse properly.
{.is-warning}


## JavaScript localization
Foundry also provides a localize function as part of the game object `game.i18n.localize()`. Localize takes one parameter the stringID that you are localizing; if it fails it will try to fallback to an English translation, and if that fails it will simply return the input string.

### Using variables in JavaScript localizations
Sometimes you want to change a string of text dynamically. Foundry provides the `game.i18n.format()` function to perform this task. The format function takes two parameters stringId and data. StringId is the string Foundry will lookup and replaces in the language file. Data is an object of variable names with the text that you want to replace in the localized string. In the language file, you denote the variable to be replaced with single curly brackets `{}`. For example, adding the variable itemType you would add the text `{itemType}` to your string in the language file.

Let's add a string with a variable to our en.json file and replace the constant name in actor-sheet.js's \_onItemCreate function with a localized string. Add a variable named MYSYSTEM.NewItem with a string value or "New {itemType}".
```json
{
  "MYSYSTEM.ActorName": "Actor's Name",
  "MYSYSTEM.NewItem": "New {itemType}"
}
```

In actor-sheet.js change the line:
```js
const name = `New ${type.capitalize()}`;
```
with the `game.i18n.format()` function and the MYSYSTEM.NewItem passing type.capitalize() as a variable to it.
```js
const name = game.i18n.format("MYSYSTEM.NewItem", {itemType: type.capitalize()})
```
When you create a new item, the text should be the same, but it will translate if a user has their browser set to a different language.

### Localizing in Handlebars

You can also perform localizations in handlebars - the `{{localize}}` helper will intelligently switch between `localize` and `format` wheter you've provided additional arguments. One example is the spell level headers - `"BOILERPLATE.Item.Spell.SpellLVL": "Level {level} Spells"` gets used in `actor-spells.hbs` like so:

```handlebars
{{localize 'BOILERPLATE.Item.Spell.SpellLVL' level=spellLevel}}
```

## Localizing Arrays of Labels
Possibly the most complex localization of data in Foundry is localizing arrays of data like the display of abilities that come from your character template in template.json. By localizing the array of abilities, you can use the power of arrays and localization. This is also exactly the type of work that `getData` is for, since this is only relevant to the display of the data.

To start, we need to add a configuration variable to the system. Create a file named config.js in the module directory. In this file, we add a variable named MYSYSTEM, and then we add an object with names of our abilities and their translation strings.
```js
export const MYSYSTEM = {}

MYSYSTEM.abilities = {
  "str": "MYSYSTEM.AbilityStr",
  "dex": "MYSYSTEM.AbilityDex",
  "con": "MYSYSTEM.AbilityCon",
  "int": "MYSYSTEM.AbilityInt",
  "wis": "MYSYSTEM.AbilityWis",
  "cha": "MYSYSTEM.AbilityCha"
}
```
Then we add each of the translation variables to the en.json file:
```json
{
  "MYSYSTEM.AbilityCha": "Charisma",
  "MYSYSTEM.AbilityCon": "Constitution",
  "MYSYSTEM.AbilityDex": "Dexterity",
  "MYSYSTEM.AbilityInt": "Intelligence",
  "MYSYSTEM.AbilityStr": "Strength",
  "MYSYSTEM.AbilityWis": "Wisdom",
  "MYSYSTEM.ActorName": "Actor's Name",
  "MYSYSTEM.NewItem": "New {itemType}"
}
```
Now we will replace the abbreviation in the actor-sheet.html with the full version of the ability name. In the actor-sheet.js file add the following as the first line in the file:
```js
import { MYSYSTEM } from '../config.js'
```
This will import the MYSYSTEM constant into the file and allow us to access the abilities variable. Let's add the full name to the data.data.abilities variable for each ability as a key. In actor-0sheet.js in the getData function below the for loop for attributes:
```js
    for (let attr of Object.values(data.data.attributes)) {
      attr.isCheckbox = attr.dtype === "Boolean";
    }
```
Add the following for loop:
```js
    for (let [key, abil] of Object.entries(data.data.abilities)){
      abil.label = game.i18n.localize(MYSYSTEM.abilities[key])
    }
```
The getData function should look like:
```js
  getData() {
    const data = super.getData();
    data.dtypes = ["String", "Number", "Boolean"];
    for (let attr of Object.values(data.data.attributes)) {
      attr.isCheckbox = attr.dtype === "Boolean";
    }
    for (let [key, abil] of Object.entries(data.data.abilities)){
      abil.label = game.i18n.localize(MYSYSTEM.abilities[key])
    }
    console.log(data.data.abilities)
    return data;
  }
```
We need to change the actor-sheet.html file to display the labels instead of the abbreviations. In the ability's, Handlebar loop change the label element to use the ability's label variable as the text. The line:
```html
<label for="data.abilities.{{key}}.value" class="resource-label">{{key}}</label>
```
Should now look like:
```html
<label for="data.abilities.{{key}}.value" class="resource-label">{{ability.label}}</label>
```
When you reload the web page and view an actor, the abbreviations should now read as full text like:

![Full Ability Names in Actor Sheet](https://drive.google.com/uc?export=view&id=1vkKmCo_YaB2eEROpIfDbSGLHGcIpWItd)

---

* **Prev:** [Separating item types into tabs](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD114-Separating-item-types-into-tabs)
* **Next:** [Adding macrobar support to your items](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD16-Adding-macrobar-support-to-your-Items)