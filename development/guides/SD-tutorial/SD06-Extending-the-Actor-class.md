---
title: 06. Extending the Actor class
description: 
published: true
date: 2024-02-08T05:50:45.962Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:35:52.934Z
---

Let's start by taking a look a the BoilerplateActor class in `/module/documents/actor.js`. As with previous examples, you'll want to rename `BoilerplateActor` to whatever your system's name is, such as `MySystemNameActor`. Whenever you have calculations to your actor's data, you'll typically want to place them in the actor class itself like in the examples below, rather than in the actor sheet class (which we'll review in the next page of the tutorial). This file is a large one, so we'll review each section in detail.

## Actor#prepareData()

```js
/**
 * Extend the base Actor document by defining a custom roll data structure which is ideal for the Simple system.
 * @extends {Actor}
 */
export class BoilerplateActor extends Actor {

  /** @override */
  prepareData() {
    // Prepare data for the actor. Calling the super version of this executes
    // the following, in order: data reset (to clear active effects),
    // prepareBaseData(), prepareEmbeddedDocuments() (including active effects),
    // prepareDerivedData().
    super.prepareData();
  }

  /** @override */
  prepareBaseData() {
    // Data modifications in this step occur before processing embedded
    // documents or derived data.
  }

  /**
   * @override
   * Augment the actor source data with additional dynamic data. Typically,
   * you'll want to handle most of your calculated/derived data in this step.
   * Data calculated in this step should generally not exist in template.json
   * (such as ability modifiers rather than ability scores) and should be
   * available both inside and outside of character sheets (such as if an actor
   * is queried and has a roll executed directly from it).
   */
  prepareDerivedData() {
    const actorData = this;
    const systemData = actorData.system;
    const flags = actorData.flags.boilerplate || {};

    // Make separate methods for each Actor type (character, npc, etc.) to keep
    // things organized.
    this._prepareCharacterData(actorData);
    this._prepareNpcData(actorData);
  }
```

We're doing a few things in here. First, we're using `export` on this class so that it's available to our main ES module file for importing. Secondly, we're extending the base `Actor` class that Foundry provides so that we get all of the logic that comes along with that by default.

We can override any method in the [Actor class](https://foundryvtt.com/api/Actor.html), but in this case we're just overriding the `prepareData()` method, along with its related methods `prepareBaseData()` and `prepareDerivedData()`.

## Source data vs. derived data

So, what's the difference between source data and derived data? Source data are things that you define in your `template.json` file, should be editable on the character sheet, and are what's stored in the database. For example, ability scores in D&D would be source data. Additionally, if a specific piece of data is not in your template.json, it will not be persisted when running duplication actions (such as dropping an item from the sidebar onto an actor's sheet) as it's assumed those attributes are calculated.

Derived data is the kind of data that you don't actually store and instead calculate it when you need it. For example, ability modifiers are based on applying a formula to ability scores, so we have no reason to make the user enter those manually. Because of that, we need to create those values in the `prepareData()` method, and more specifically in the `prepareDerivedData()`.


### prepareData()

> **Do not call update() in data prep**
> Elsewhere, when working with documents, you want to use the `update()` method to ensure that your changes are persisted to the database. When working with derived data, use standard javascript in-memory assignment with the `=` operator. If you try to use `update()`, you will likely cause an infinite loop, as `update()` will re-trigger `prepareData()`, which will then call `update()`, etc.
{.is-danger}

The `prepareData()` method usually doesn't have to overwritten since the version provided by default covers most use cases. You'll only typically need to modify it if you need to reorder how its submethods are called, which by default are:

1. `this.data.reset();` - Resets actor data back to its unmodified state (equivalent to how it is stored in the database).
2. `this.prepareBaseData();` - Prepare any data related to the Document itself, before any embedded Documents or derived data is computed.
3. `this.prepareEmbeddedDocuments();` Prepare all embedded Documents within the parent Document, such as owned Items or Active Effects. Additionally, Active Effects are called in a submethod of this called `this.applyActiveEffects();` which can be overridden.
4. `this.prepareDerivedData();` Apply all transformations to data that will adjust its derived values. **Most calculations and derived data should occur during this step.**

If the above order works for your system, you can either leave out the `prepareData()` method entirely or just call `super.getData()` in it, such as in the below example:

```js
  /** @override */
  prepareData() {
    // Prepare data for the actor. Calling the super version of this executes
    // the following, in order: data reset (to clear active effects),
    // prepareBaseData(), prepareEmbeddedDocuments() (including active effects),
    // prepareDerivedData().
    super.prepareData();
  }
```

### prepareBaseData()

The `prepareBaseData()` method is the first method called that will modify your actor's data. In many cases, this method will be blank, like in Boilerplate's example:

```js
  /** @override */
  prepareBaseData() {
    // Data modifications in this step occur before processing embedded
    // documents or derived data.
  }
```

However, there are cases where it can be useful. For example, if you need an Active Effect to modify a derived value, you can initialize or calculate the derived value in this step (such as an ability score modifier) so that the Active Effect can modify it later.

### prepareDerivedData()

The `prepareDerivedData()` method is where most of your data modifications will normally occur. Let's take a look at Boilerplate's example of how it handles this method:

```js
  /**
   * @override
   * Augment the actor source data with additional dynamic data. Typically,
   * you'll want to handle most of your calculated/derived data in this step.
   * Data calculated in this step should generally not exist in template.json
   * (such as ability modifiers rather than ability scores) and should be
   * available both inside and outside of character sheets (such as if an actor
   * is queried and has a roll executed directly from it).
   */
  prepareDerivedData() {
    const actorData = this;
    const systemData = actorData.system;
    const flags = actorData.flags.boilerplate || {};

    // Make separate methods for each Actor type (character, npc, etc.) to keep
    // things organized.
    this._prepareCharacterData(actorData);
    this._prepareNpcData(actorData);
  }
```

First, we're making a few convenience variables related to the actor. Those are `actorData`, `systemData`, and `flags`. These are optional, but without them you'll write out long variables like `this.system.abilities.value`. For the `flags` line in particular, we're setting it to either the actor's `boilerplate` flags, if any, or an empty object otherwise. If you have unreliable data that may or may not exist, it's always a good idea to have a fallback similar to that.

> **Flags 101**
> Flags are a namespaced record of key-value pairs. As a system developer, you don't usually need to interact with them - you have full control over the `system` property - but occasionally it makes sense to put something there. The full path will look like `actor.flags.boilerplate.flagKey: flagValue`, which can be accessed with the method `actor.getFlag("boilerplate", "flagKey")`
> The flags object is namespaced by package ID - so where this tutorial says `flags.boilerplate`, you should replace `boilerplate` with whatever you change the ID of your system to. The core software uses `flags.core` for its flags, such as to store which sheet a document uses (e.g. if there's multiple character sheets available). Modules your users install can use their own IDs to store data if they need to store information not tracked by your `template.json`. 
{.is-info}

Second, we're running custom methods for `this._prepareCharacterData(actorData);` and `this._prepareNpcData(actorData);`. Because we've named them with an underscore as a prefix, we're also signifying to any modules that extend our system that these methods are internal and generally shouldn't be overridden.

If you have large amounts of data that get calculated or derived, it's helpful to break them out into additional methods to keep things organized.

> If you implement a system data model, you can use their `prepareDerivedData()` methods in place of these next two type-specific data preparation methods.
{.is-info}

### \_prepareCharacterData()

Now that we've segmented off a custom method that can prepare character data without also affecting NPCs, let's calculate ability modifiers from ability scores.

```js
  /**
   * Prepare Character type specific data
   */
  _prepareCharacterData(actorData) {
    if (actorData.type !== 'character') return;

    // Make modifications to data here. For example:
    const systemData = actorData.system;

    // Loop through ability scores, and add their modifiers to our sheet output.
    for (let [key, ability] of Object.entries(systemData.abilities)) {
      // Calculate the modifier using d20 rules.
      ability.mod = Math.floor((ability.value - 10) / 2);
    }
  }
```

Because we know this method is intended to only run on character actors, we're starting it out with an if statement to check the actor's type and return early if it's not a character. (This is called a typeguard).

You can perform any sort of data modifications in this step. In this case, we want to generate d20-style ability modifiers. The Boilerplate system has an `abilities` property in its data model, so we're looping through it. The syntax is a bit tricky for the actual loop, but `Object.entries` will return pairs of key/value items for whatever apply it to, and when we loop through that each loop will have a `key` that would be the label such as `str` or `dex` and also an `ability` which would be the ability object.

In this case the ability object only has a `value` property, so we then use some math based on the d20 rules to calculate what the ability modifier should be and assign it to `ability.mod`.

And with that, we're done deriving new values! We don't have to return anything; because we're working with objects and we didn't clone or copy them, the changes we made to `ability.mod` will automatically work their way back up to the original `actorData` object.

### \_prepareNpcData()

As a second example, let's take a look at what preparing NPC data would look like:

```js
  /**
   * Prepare NPC type specific data.
   */
  _prepareNpcData(actorData) {
    if (actorData.type !== 'npc') return;

    // Make modifications to data here. For example:
    const systemData = actorData.system;
    systemData.xp = (systemData.cr * systemData.cr) * 100;
  }
```

As with the previous method, we're starting out by checking the actor type and exiting early if needed. From there, we're doing a simple calculation to create a new `xp` property in the actor's data. This would be accessible on the actor at `actor.system.xp`.

## Actor#getRollData()

Another important method for most system's is the actor's `getRollData()` method. This is the method that is called when the actor's token is selected and a roll is created in Foundry's chat dialog, and it's also the method you should call when creating roll data for any custom rolls. Similar to `prepareDerivedData()`, we'll break it up into sub-methods:

```js
  /**
   * Override getRollData() that's supplied to rolls.
   */
  getRollData() {
    const data = super.getRollData();

    // Prepare character roll data.
    this._getCharacterRollData(data);
    this._getNpcRollData(data);

    return data;
  }

  /**
   * Prepare character roll data.
   */
  _getCharacterRollData(data) {
    if (this.type !== 'character') return;

    // Copy the ability scores to the top level, so that rolls can use
    // formulas like `@str.mod + 4`.
    if (data.abilities) {
      for (let [k, v] of Object.entries(data.abilities)) {
        data[k] = foundry.utils.deepClone(v);
      }
    }

    // Add level for easier access, or fall back to 0.
    if (data.attributes.level) {
      data.lvl = data.attributes.level.value ?? 0;
    }

  /**
   * Prepare NPC roll data.
   */
  _getNpcRollData(data) {
    if (this.type !== 'npc') return;

    // Process additional NPC data here.
  }
```

Notice that we're not calculating new values here (it's helpful to have them available in all contexts, whether or not it's a roll), but what we are doing is taking properties deeply nested in the actor data like ability scores and the level and creating shorthands for them. So we're essentially updating so that in addition to the following working in roll formulas:

```
/r d20 + @attributes.level.value
```

We could instead do the following:

```
/r d20 + @lvl
```

## Wrapping up the Actor class

Finally, don't forget your closing `}` at the end of the class definition! Next, let's take a look at the class that retrieves data specifically for character sheets.

---

* **Prev:** [Creating your main system javascript file](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD05-Creating-your-main-JS-file)
* **Next:** [Extending the ActorSheet class](https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD07-Extending-the-ActorSheet-class)