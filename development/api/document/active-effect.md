---
title: Active Effect
description: An embedded document that can be used to modify the attributes of other documents during prepareData
published: true
date: 2024-07-26T17:24:58.645Z
tags: documentation
editor: markdown
dateCreated: 2024-06-08T05:46:12.955Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Active Effects are a built-in way for users, systems, and modules to dynamically alter actor properties.

*Official documentation*

- [Knowledge Base](https://foundryvtt.com/article/active-effects/)
- [ActiveEffect](https://foundryvtt.com/api/classes/client.ActiveEffect.html)
- [BaseActiveEffect](https://foundryvtt.com/api/classes/foundry.documents.BaseActiveEffect.html)
- [Document](https://foundryvtt.com/api/classes/foundry.abstract.Document.html)
- [ActiveEffectConfig](https://foundryvtt.com/api/classes/client.ActiveEffectConfig.html)

**Legend**
```js
ActiveEffect.fromStatusEffect // `.` indicates static method or property
ActiveEffect#duration // `#` indicates instance method or property
```

## Overview

The ActiveEffect document class provides extensive built-in functionality; it not only comes with its own core-defined sheet, but the actual effect application process is also handled by Foundry. Most systems only need to provide sheet buttons for basic CRUD operations. The actual changes are applied during the `prepareData` process, altering the in-memory properties without changing the underlying source data. This allows for effective representation of all sorts of changes, from a *shield* spell that improves defenses to a shapeshift that overhauls almost all fo the actor's statistics.

## Key Concepts

When working with Active Effects, you should keep the following in mind.

### Embedded in Actors and Items

Active effects can be embedded in both actors and items. When embedded in an item, effects only do things if the item is then embedded in an actor.

If `CONFIG.ActiveEffect.legacyTransferral` is true (default), any active effects on an item are copied to the parent actor when the item is added to the actor. The effects on the item then functionally do nothing, and any future effects added to an item already on an actor do nothing.

If you set `CONFIG.ActiveEffect.legacyTransferral = false` in your `init` hook, then the effect application process will not only check the `effects` collection of the actor, but also of any items on the actor. This is **recommended** to do for all systems - `legacyTransferral`, as its name suggests, is in place because prior to v11 Foundry did not support grandchild documents.

### Schema

The valid properties for active effects are summed up by the [ActiveEffectData](https://foundryvtt.com/api/interfaces/foundry.types.ActiveEffectData.html) interface.

#### EffectChangeData

The core functionality of Active Effects is provided by the `changes` array, which takes the form of the [EffectChangeData](https://foundryvtt.com/api/interfaces/foundry.types.EffectChangeData.html) interface.

- The `key` property uses dot notation for nested properties, e.g. `system.attributes.strength.value`
- The `value` property is intelligently handled by the target properties corresponding `DataField` instance, if available
- The `mode` property can be one of six values, 0-6. They correspond to:
  0. Special
  1. Multiply
  2. Add
  3. Downgrade
  4. Upgrade
  5. Override
- The `priority` property is not naturally configurable on the Active Effect sheet. Instead, it defaults to `10 * mode`. Active effects are applied from low priority to high, so by default a `multiply` will apply first *then* any `add` operations, etc.

### Only Modify Actors

There's two important limitations to the core implementation of active effects:
- Active Effects don't modify token properties
- Active Effects don't modify Items.

An intrepid developer can work around this - there are a number of systems and modules that do so - but these actions are not natively supported by foundry. It's worth noting that the actual `ActiveEffect#apply` doesn't make reference to any specific actor properties, it's just an issue of the effects are only natively applied *to* actors. 

___
## API Interactions

Here are the most common things to know about 

### CRUD Operations

Active effects inherit all of the ordinary Document and ClientDocument operations. A typical sheet listener to create an Active Effect looks like the following:

```js
_createEffect(event) {
  aeCls = getDocumentClass("ActiveEffect");
  aeCls.createDialog({}, {parent: this.document});
}
```

Similarly, you can use `document.effects.get` to fetch an active effect, `effect.render(true)` to open the sheet, `effect.delete()` to delete the effect, and all the other numerous document functions.

### Actor#applyActiveEffects

The Actor class have the method `applyActiveEffects`, this method is used to apply any transformation to the Actor's data caused by a ActiveEffect.
It may be of interest to edit this method to condition the changes applied to the actor, reorder the prioritization, etc. Something similar to:
```js
  applyActiveEffects() {
    for ( const effect of this.allApplicableEffects() ) {
      //Determine whether this Active Effect is suppressed or not.
      effect.determineSuppression(); 
    }
    return super.applyActiveEffects();
  }
```

### applyActiveEffect hook

> Stub
> This section is a stub, you can help by contributing to it.

### `type` and `system`
> Stub
> This section is a stub, you can help by contributing to it.

___
## Specific Use Cases

> *My system users want to know what they can fill in as Attribute Keys that the ActiveEffect can actually change..*

You could add the following helper Hook to your system, which adds a `<datalist>` to the ActiveEffects configuration application dialogue.
```js
/**
 * Adding a multselect helper for showing Actor attributes in ActiveEffect dialogue
 */
Hooks.on("renderActiveEffectConfig", async (activeEffectConfig, html, data) => {
  const effectsSection = html[0].querySelector("section[data-tab='effects']");
  const inputFields = effectsSection.querySelectorAll(".key input");
  const datalist = document.createElement("datalist");
  const attributeKeyOptions = {};

  datalist.id = 'attribute-key-list';
  inputFields.forEach((inputField) => {
    inputField.setAttribute('list', 'attribute-key-list');
  });

  for (const datamodel in CONFIG.Actor.dataModels) {
    CONFIG.Actor.dataModels[datamodel].schema.apply(function() {
      if ( !(this instanceof foundry.data.fields.SchemaField) ) {
        attributeKeyOptions[this.fieldPath] = this.label;
      }
    });
  }

  const sortedKeys =  Object.keys(attributeKeyOptions).sort();
  sortedKeys.forEach(key => {
    const attributeKeyOption = document.createElement("option");
    attributeKeyOption.value = key;
    if ( !!attributeKeyOptions[key] )
      attributeKeyOption.label = attributeKeyOptions[key];
    datalist.appendChild(attributeKeyOption);
  });

  effectsSection.append(datalist);
});
```

### Status Effects
> Stub
> This section is a stub, you can help by contributing to it.

___
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.
