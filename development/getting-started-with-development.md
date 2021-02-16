---
title: Getting Started with Package Development
description: Some common hurdles facing new Package Developers
published: true
date: 2021-02-16T14:39:02.712Z
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

> [stub](https://github.com/VanceCole/macros/blob/master/flags.js)


### Unset a flag

A safe way to delete your flag's value is with [`Entity#unsetFlag`](https://foundryvtt.com/api/Entity.html#unsetFlag). This will fully delete that key from your module's flags on the provided entity.

```js
someEntity.unsetFlag('myModuleName', 'myFlagName');
```


## How do I work with settings?

> [stub](https://github.com/VanceCole/macros/blob/master/settings.js)

## How do I delete a value?

This is foundry specific and will delete the `someKey` in the database entry for `someEntity`
```js
someEntity.someKey -= null;
```

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
