---
title: Data Model
description: The abstract base class which defines the data schema contained within a Document.
published: true
date: 2024-02-16T07:54:54.103Z
tags: documentation
editor: markdown
dateCreated: 2024-02-15T18:00:00.416Z
---

![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

Official documentation
- [v10 Data Model](https://foundryvtt.com/article/v10-data-model/)
- [Introduction to System Data Models](https://foundryvtt.com/article/system-data-models/)
- [DataModel API reference](https://foundryvtt.com/api/classes/foundry.abstract.DataModel.html)
- [DataField API reference](https://foundryvtt.com/api/classes/foundry.data.fields.DataField.html)



## Overview
The data model is the root of how Foundry synchronizes information between the client and server. It includes functions for:

- Cleaning data
- Validating data
- Migrating data
- Keeping what's saved to the database separate from what's served to developers

**As a System developer**: Data models can entirely replace the type-specific field initialization of `template`json; the [dnd5e](https://github.com/foundryvtt/dnd5e/blob/master/template.json) system is an example of how much you can trim down that file, letting the data model do the rest.

**As a Module developer**: Data models are necessary for [Module Sub-Types](https://foundryvtt.com/article/module-sub-types/) module sub-types, where you provide your own new type of Actor, Item, JournalEntry, or other document sub-type. 

### Legend
```js
DataModel.defineSchema // `.` indicates static method or property
DataModel#invalid // `#` indicates instance method or property
```
--- 
## Key Concepts

Working with data models doesn't have to be daunting - almost all of the functionality is provided just by extending the appropriate `DataModel` class. (System and module developers usually want to start by extending `foundry.abstract.TypeDataModel`)

### The Schema

The one piece of information you MUST provide is the `static defineSchema()` method, which returns an key-value record where every value is a subclass of DataField. Foundry makes many of these subclasses available at `foundry.data.fields`, so it's common practice to lead a schema definition with `const fields = foundry.data.fields`. The following diagram provides some details on the inheritance

```md
DataField
	SchemaField
  	EmbeddedDataField
    	EmbeddedDocumentField
    DocumentStatsField
  BooleanField
  NumberField
  	AngleField
    AlphaField
    IntegerSortField
  StringField
  	DocumentIdField
    	ForeignDocumentField
    ColorField
    FilePathField
    JSONField
    HTMLField
  ObjectField
  	DocumentOwnershipField
    TypeDataField
  ArrayField
  	SetField
    EmbeddedCollectionField
    	EmbeddedCollectionDeltaField
```

You don't have to use the most nested versions of a field; in fact, it's frequently beter not to — StringField works great by itself. Furthermore, several of these fields are NOT for system and module developers (e.g. EmbeddedCollectionField), as they require server-side support: This isn't a clever way to do "items within items".

#### DataField options

There's quite a few options you can pass to DataField, which are officially documented [here](https://foundryvtt.com/api/interfaces/foundry.data.fields.DataFieldOptions.html). However, the most common change in a subclass is its handling of the options and what the defaults are. 

Here's some important information to know about each option:

##### `required` and `nullable`

*Default*: `required: false` and `nullable: false`

These two options constitute a special form of validation; `required` prevents passing an undefined value, while `nullable` allows a `null` value.

##### `initial`

*Default*: `undefined`

In addition to being a static value, this can be a function which takes in the entire data model as an argument and returns a value. If `required` is true and there's no `initial` value, this will create errors if the field is not passed in the constructor.

`StringField` and its descendants modify the default slightly; if you pass `required: true, blank: true`, that's equivalent to also passing `initial: ""`.

#### `readonly`

*Default*: `false`

This option prevents a field from being changed after initial creation. Readonly fields can still be altered by `Document#_preCreate` and the `preCreateDocument` hook, and can be dynamically set if `initial` is a function.

#### `validate` and `validationError`

*Default*: `validate: undefined` and `validationError: "is not a valid value"`

If defined, `validate` should have the signature `(value, options) => boolean`; returning false is functionally the same as throwing a `DataModelValidationFailure`. This can be useful when you don't want to entirely define a new DataField subclass but do want a bit of additional handling, such as enforcing that a `StringField` is all lowercase with no special characters. The second argument, `options`, is [documented here](https://foundryvtt.com/api/interfaces/foundry.data.fields.DataFieldValidationOptions.html).

The `validationError` option can be used with or without the `validate` option; it simply replaces the default console errors generated on a type validation failure. Many data field subclasses replace the default string with more specific language.

#### `label` and `hint`

*Default*: `""` (for both) 

These two fields nominally allow you to pair your UI work with the core data definitions. In practice, these are difficult to access; From an `ActorSheet`, the path would be `this.actor.system.getField('relative.object.path').label`. These otherwise aren't used anywhere natively within Foundry, not even for token bars.

#### Other Options

DataField subclasses sometimes take additional options:

- [NumberFieldOptions](https://foundryvtt.com/api/modules/foundry.data.fields.html#NumberFieldOptions)
- [StringFieldOptions](https://foundryvtt.com/api/modules/foundry.data.fields.html#StringFieldOptions)
- [FilePathFieldOptions](https://foundryvtt.com/api/modules/foundry.data.fields.html#FilePathFieldOptions)

#### Migrating from template.json

Classically, Foundry uses the `templates` object to define shared properties. `DataModel.defineSchema()` allows you to use standard [object-oriented principles](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_programming) to define inheritance. 

The official [Introduction to System Development](https://foundryvtt.com/article/system-development/) article provides the following snippet of a template.json as an example:
```json
"Actor": {
  "types": ["hero", "pawn", "villain"],
  "templates": {
    "background": {
      "biography": "",
      "hairColor": "blue"
    },
    "resources": {
      "health": {
        "min": 0,
        "value": 10,
        "max": 10
      },
      "power": {
        "min": 0,
        "value": 1,
        "max": 3
      }
    }
  },
  "hero": {
    "templates": ["background", "resources"],
    "goodness": {
      "value": 5,
      "max": 10
    }
  },
  "pawn": {
    "templates": ["resources"]
  },
  "villain": {
    "templates": ["background", "resources"],
    "wickedness": {
      "value": 5,
      "max": 100
    }
  }
},
```

With a data model, we have two non-mutually exclusive options

1. Define a hierarchy of inheritance where we call `super.defineSchema()`
2. Define functions that return valid schema objects we can mix in

For example, we could structure it like this

```js

// I threw this at the top of the file because we're re-using it lots of places
// but you probably want to break all of these class and function definitions up
// and can just stick this inside `defineSchema`
const fields = foundry.data.fields;

// Example of a helper function that allows us to minimize repetition
// You could also wrap the return object in `new SchemaField()`
function resourceField(initialValue, initialMax) {
  return {
		// Make sure to call new so you invoke the constructor!
    min: new fields.NumberField({ initial: 0 }),
    value: new fields.NumberField({ initial: initialValue }),
    max: new fields.NumberField({ initial: initialMax }),
  };
}

class CommonActorData extends foundry.abstract.TypeDataModel {
	static defineSchema() {
    // Note that the return is just a simple object
  	return {
      resources: new fields.SchemaField({
  			// Whenever you want to have nested objects, wrap it in SchemaField
      	health: new SchemaField(resourceField(10, 10)),
        power: new SchemaField(resourceField(1, 3))
      })
    }
  }
}

// Pawns would just have the basic resources but then you could add additional methods
class PawnData extends CommonActorData {} 

class CharacterData extends CommonActorData {
	static defineSchema() {
    // CharacterData inherits those resource fields
  	const commonData = super.defineSchema();
    return {
      // Using destructuring to effectively append our additional data here
    	...commonData,
      background: new fields.SchemaField({
        // Example of using a specialized field, in this case to help with sanitation
        biography: new fields.HTMLField({ initial: "" }),
        hairColor: new fields.StringField({ initial: "blue" })
      }),
    }
  }
}

// We can have branching inheritance; both VillainData and HeroData extend CharacterData
class HeroData extends CharacterData {
	static defineSchema() {
  	const characterData = super.defineSchema();
    return {
    	...characterData,
      goodness: new fields.SchemaField({
        value: new fields.NumberField({ initial: 5 }),
        max: new fields.NumberField({ initial: 10 })
      }),
    }
  }
}

class VillainData extends CharacterData {
	static defineSchema() {
  	const characterData = super.defineSchema();
    return {
    	...characterData,
      wickedness: new fields.SchemaField({
        value: new fields.NumberField({ initial: 5 }),
        max: new fields.NumberField({ initial: 100 })
      }),
    }
  }
}
```

### DataModel#constructor 

Developers generally don't need to know the ins and outs of how `new DataModel` works. In case you do, the following summarizes the steps that occur using a `new Actor` as an example. For reference, `Actor extends ClientDocumentMixin(BaseActor)`, and `BaseActor extends Document extends DataModel`, so there are five distinct layers of inheritance happening.

`Actor` doesn't override the `constructor`, but `ClientDocumentMixin` does; that calls `super` and then instantiates the `apps` record and the `_sheet` pointer. The super call skips through `BaseActor` and `Document`, as neither override the constructor, landing us in `DataModel#constructor`. Within this function several steps happen:

1. `_source` is set to the return of `_initializeSource`
2. `_configure` is called
3. `validate` is called
4. `_initialize` is called

The following sections explain each of those function calls. Whenever a data model is *updated*, only `validate` and `_initialize` are called, as the first two define many read-only properties.

#### DataModel#\_initializeSource

`Actor#_initializeSource` routes to `BaseActor#_initializeSource`, which calls `super` then sets up some default `prototypeToken` properties. Otherwise, the relevant pieces are in `DataModel`, which checks that the source data provided is an object then calls `migrateDataSafe`, `cleanData`, and `shimData`. Importantly, these changes are at the lowest level of the data model and safeguard the data that is actually saved to the database.

**migrateDataSafe**: An error-checking wrapper for `migrateData`, this moves old data loaded from the database into new formats (this is a synchronous operation; the moves are not saved back to the database automatically). `DataModel#migrateData` calls `this.schema.migrateSource()`, which ripples down to trigger migrations on any embedded data models such as `Actor#system`.

**cleanData**: This just calls `schema.clean`, which propagates calls to all the fields to run `_cast` and `_cleanType`. For example, NumberField constricts the value to any provided `min` or `max` values, `StringField` will `trim` the input, and generally the DataFields attempt type coercion.

**shimData**: This is where Foundry adds pointers like `Actor#data` pointing to `Actor#system` with their deprecation warnings getter/setters.

#### DataModel#\_configure

This function defines all sorts of additional pointers and getters necessary to make the data model function. Data Model does not natively do anything here, it's strictly for subclasses. `Actor#_configure` calls `super` then defines its `_dependentTokens`; in `Document#_configure`, the document's collection relationships are setup, both where it can be found as well as any embedded collections it might have.

#### DataModel#validate

This method is similar to `cleanData` but is more thorough, allowing things like joint validation rules where multiple fields are considered together. A simple example of this is folders checking that their parent pointer is not pointing to themselves, checking the `folder` property against the `_id` property.

#### DataModel#\_initialize

This method copies data from the `_source` field to the top level of the data model. For `Actor`, the soonest layer of inheritance is `ClientDocument`, which calls `super` before kicking off the `prepareData` cycle; for more on that, check out [From Load to Render](/en/development/guides/from-load-to-render). 

---
## API Interactions

Beyond their class definitions, there's a few other things to know with data models.

### Registering Data Models

Document data models MUST be registered in an init hook.

```js
// If we had properly exported all of our classes from the full example earlier
import { PawnData, HeroData, VillainData } from "./module/data.mjs"

Hooks.once("init", () => {
  // Systems can just use direct assignment here
  // since they'll be merging into an empty object
  // But it's generally safer to just use mergeObject when possible
  foundry.utils.mergeObject(CONFIG.Actor.dataModels, {
    // The keys are the types defined in our template.json
    pawn: PawnData,
    hero: HeroData,
    villain: VillainData
  })
  // You can repeat with other document types, e.g. CONFIG.Item.dataModels
})
```

---
## Specific Use Cases

[Settings](/en/development/api/settings) can take a data model as an argument when registered, allowing  you to have a strongly typed data.

> Stub
> This section is a stub, you can help by contributing to it.

---
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.