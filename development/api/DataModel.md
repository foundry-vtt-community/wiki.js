---
title: Data Model
description: The abstract base class which defines the data schema contained within a Document.
published: true
date: 2024-02-15T18:00:18.858Z
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

> Stub
> This section is a stub, you can help by contributing to it.

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

### \_source initialization

> Stub
> This section is a stub, you can help by contributing to it.

### Data validation

> Stub
> This section is a stub, you can help by contributing to it.

---
## API Interactions

Beyond their class definitions, there's a few other things to know with data models.

### Registering Data Models

Data models MUST be registered in an init hook.

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

> Stub
> This section is a stub, you can help by contributing to it.

---
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.