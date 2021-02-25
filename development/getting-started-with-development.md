---
title: Getting Started with Package Development
description: Some common hurdles facing new Package Developers
published: true
date: 2021-02-25T05:38:04.580Z
tags: 
editor: markdown
dateCreated: 2021-02-05T16:13:36.470Z
---

> ### What is this?
> This is going to be our FAQ and "common hurdles to overcome" guide to help new package developers. It's going to be perpetually updating and changing.
> If you know of some questions or common sticking points for new package developers, please edit this to include those things.
> ### Have a question not answered here?
> The [League of Foundry VTT Developers](https://discord.gg/cudQBu8HKT) is a helpful development-focused discord community which aims to help veterans and new developers alike. Please drop by and ask us your questions, the more questions we get the more likely this document is going to be updated with their answers.
{.is-warning}

> This document is up to date as of 0.7.9
{.is-info}

# FAQs

## Is there a list of hooks?

[There is a community maintained hook typescript definition and documentation file.](https://github.com/League-of-Foundry-Developers/foundry-vtt-types/blob/foundry-0.7.9/types/core/hooks.d.ts) But there's an easier way to discover what hook you can use for a specific piece of functionality:

```js
CONFIG.debug.hooks = true;
```

With this enabled, you'll see every hook that fires as you interact with Foundry's UI and what arguments it is passed.


## How do I delete a value?

This is foundry specific and will delete the `someKey` in the database entry for `someEntity`
```js
someEntity.someKey -= null;
```


## What is a flag and how do I use them?

Flags are the safest way that Modules can store arbitrary data on existing entities. If you are making a module which allows the user to set a data point which isn't supported normally in Foundrdy Core or a System's data structure, you should use a flag.

A flag does not have to be a specific type, anything which can be `JSON.stringify`ed is valid.

### Setting a flag's value
Flags are automatically namespaced within the first parameter given to [`Entity#setFlag`](https://foundryvtt.com/api/Entity.html#setFlag).

```js
const newFlagValue = 'foo';

someEntity.setFlag('myModuleName', 'myFlagName', newFlagValue);
```

#### Can I mutate the value itself?
Setting a flag's value without `setFlag` will not persist that change in the database. This should only be done as part of a larger operation which persists an entity's data in the database, for example as a part of character sheet editing.

### Getting a flag's value
There are two places to get a flag value: On the data model itself, or with [`Entity#getFlag`](https://foundryvtt.com/api/Entity.html#getFlag)

```js
const flagValue = someEntity.getFlag('myModuleName', 'myFlagName');
// flagValue === 'foo'
```

#### On the data model itself
This can be somewhat tricky as it might be different depending on what entity you're dealing with. Somewhere in the entity's data object there is a `flags` key. The object attached is keyed by module `name`, which is itself an object keyed by flag name, as registered in `setFlag`.

### Unset a flag
A safe way to delete your flag's value is with [`Entity#unsetFlag`](https://foundryvtt.com/api/Entity.html#unsetFlag). This will fully delete that key from your module's flags on the provided entity.

```js
someEntity.unsetFlag('myModuleName', 'myFlagName');
```

### How do I use this?
It's arbitrary data that you can safely control on any `Entity`. Because of this, all of the hooks related to that entity are going to have your flag available when they fire.

For example, if I have a flag on a Scene, I can check if that flag exists when the `updateScene` hook fires.

```js
Hooks.on('updateScene', (scene, data) => {
  if (hasProperty(data, 'flags.myModule')) {
    console.log(data);
  }
});
```

## How do I work with settings?

Settings, like flags, are a way for modules to store and persist data. Settings are not tied to a specific entity however, unlike flags. Also unlike flags they are able to leverage the 'scope' field to keep a set of data specific to a user's localStorage (`scope: client`) or put that data in the database (`scope: world`).

For the vast majority of use-cases, settings are intended to be modified by a UI, either a Menu or within the Module Settings panel itself. These settings are intended to be used to modify the functionality of a module or system, rather than store arbitrary data for that module or system.

### Registering a Setting

All settings must be registered before they can be set or accessed. This needs to be done with [`game.settings.register`](https://foundryvtt.com/api/ClientSettings.html#register), with `game.settings` being an instance of `ClientSettings`.

```js
/*
 * Create a custom config setting
 */
await game.settings.register('myModuleName', 'mySettingName', {
  name: 'My Setting',
  hint: 'A description of the registered setting and its behavior.',
  scope: 'world',     // "world" = sync to db, "client" = local storage 
  config: true,       // false if you dont want it to show in module config
  type: Number,       // Number, Boolean, String,  
  default: 0,
  range: {.           // range turns the UI input into a slider input
    min: 0,           // but does not validate the value
    max: 100,
    step: 10
  },
  onChange: value => { // value is the new value of the setting
    console.log(value)
  }
});
```

### Setting a Setting's value
Settings can be set with [`game.settings.set`](https://foundryvtt.com/api/ClientSettings.html#set). It's important to note that a `scope: world` setting can only be set by a Gamemaster user, and that `scope: client` settings will only persist on the user's local machine.

```js
const whateverValue = 'foo';

game.settings.set('myModuleName','myModuleSetting', whateverValue);
```

### Getting a Setting's value

Settings can be read with [`game.settings.get`](https://foundryvtt.com/api/ClientSettings.html#get). 

```js
const someVariable = game.settings.get('myModuleName','myModuleSetting');
console.log(someVariable); // expected to be 'foo'
```

### Setting Menus

Sometimes your use case is more complex than a few settings will allow you to manage. In these cases the best play is to register a settings menu with [`game.settings.registerMenu`](https://foundryvtt.com/api/ClientSettings.html#registerMenu), and manage your settings logic with a [FormApplication](https://foundryvtt.wiki/en/development/guides/understanding-form-applications).

```js
game.settings.registerMenu("myModule", "mySettingsMenu", {
  name: "My Settings Submenu",
  label: "Settings Menu Label",      // The text label used in the button
  hint: "A description of what will occur in the submenu dialog.",
  icon: "fas fa-bars",               // A Font Awesome icon used in the submenu button
  type: MySubmenuApplicationClass,   // A FormApplication subclass
  restricted: true                   // Restrict this submenu to gamemaster only?
});
```

#### Why would I want this?

FormApplications allow you to run any logic you want, which includes setting settings, thus this kind of power could be leveraged to accomplish many things:

1. **Space.** You could easily use this to tidy up a lot of module settings which would otherwise take up a lot of vertical space on the settings list.
2. **Validation.** Since you control the FormApplication's submit logic, you could run validation on user inputs before saving them to the database.
3. **Edit Setting Objects.** If you have a use case for a complex object of data being stored as a setting, a FormApplication menu would let your users manipulate that object directly.


# Common Hurdles and How to Overcome Them


## Storing Module Data

> - Flags
> - Settings
> - Pros and Cons of the two

# Hello World Module Walkthrough 


> ### [Stub](https://www.reddit.com/r/restofthefuckingowl/), should be its own guide article.
> https://foundryvtt.com/article/module-development/
> 1. Create your directory
> 1.a Symlink from a /dist directory
> 2. Common gotchyas with manifest.json and file structure
> 3. Include a script file
> 6. Include localization
> 4. Include a template
> 5. Include CSS
> 6. ???
> 7. Profit


# Appendix 1: Fun Javascript Terminology

## Mutate

Basically means "changing" a piece of data.

```js

let foo = 'bar';

someFunction() {
  // does things...
  foo = 'bat';
  
  console.log({foo}); // 'bat'
}

someOtherFunction() {
  if (foo === 'bar') {
  	// do things...
  }
}
```

In the example above, `someFunction` mutates `foo`. Depending on the order in which it is called (before or after `someOtherFunction`) this might cause `someOtherFunction` to behave unexpectedly.

This isn't necessarily a bad pattern but it can save you some headaches if you avoid mutating things as much as possible. Instead try to make duplicates of your data when you want to change it.

## Promise

> [stub](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
