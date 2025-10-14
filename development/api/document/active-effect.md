---
title: Active Effect
description: An embedded document that can be used to modify the attributes of other documents during prepareData
published: true
date: 2025-10-14T22:30:34.943Z
tags: documentation
editor: markdown
dateCreated: 2024-06-08T05:46:12.955Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

Active Effects are a built-in way for users, systems, and modules to dynamically alter actor properties.

**Foundry VTT API Documentation Links**  

*Document Classes* 
- **[ActiveEffect](https://foundryvtt.com/api/v13/classes/foundry.documents.ActiveEffect.html)**  
  *Client class for Active Effects*  
- **[BaseActiveEffect](https://foundryvtt.com/api/v13/classes/foundry.documents.BaseActiveEffect.html)**  
  *Abstract base class for Active Effects.*  
- **[Document](https://foundryvtt.com/api/v13/classes/foundry.abstract.Document.html)**  
  *Parent class for all Foundry document types.*  

#### **Applications Classes**  
- **[ActiveEffectConfig](https://foundryvtt.com/api/v13/classes/foundry.applications.sheets.ActiveEffectConfig.html)**  
  *Latest implementation (FoundryVTT v13 or above).*  
- **[ActiveEffectConfig (Legacy)](https://foundryvtt.com/api/v12/classes/client.ActiveEffectConfig.html)**  
  *Deprecated version (FoundryVTT v12 or v11)*  

**Legend**
```js
ActiveEffect.fromStatusEffect // `.` indicates static method or property
ActiveEffect#duration // `#` indicates instance method or property
```

## Overview

The ActiveEffect document class provides extensive built-in functionality; it not only comes with its own core-defined sheet, but the actual effect application process is also handled by Foundry.
Most systems only need to provide sheet buttons for basic CRUD operations. The actual changes are applied during the `prepareData` process, altering the in-memory properties without changing the underlying source data.
This allows for effective representation of all sorts of changes, from a *shield* spell that improves defenses to a shapeshift that overhauls almost all fo the actor's statistics.

## Key Concepts

When working with Active Effects, you should keep the following in mind.

### Active Effect Embedding Behavior

If `CONFIG.ActiveEffect.legacyTransferral` is `true`:
- Active effects on an item are **copied to the parent actor** when the item is added to the actor.
- This occurs **only if** the Active Effect's `transfer` property is set to `true`.
- These copied effects are **deleted** when the item is removed from the actor.


If `legacyTransferral` is false:
- Active effects are **not copied** to the actor.
- However, they will still **apply to the actor** from within the item, provided the Active Effect's `transfer` property is `true`.

> **@deprecated** since **FoundryVTT v11**  
> This configuration can be set to `true` until **V14**, at which point it will be removed.

### Schema

The valid properties for active effects are summed up by the [ActiveEffectData](https://foundryvtt.com/api/v13/interfaces/foundry.documents.types.ActiveEffectData.html) interface.

#### EffectChangeData

The core functionality of Active Effects is provided by the `changes` array, which takes the form of the [EffectChangeData](https://foundryvtt.com/api/v13/interfaces/foundry.documents.types.EffectChangeData.html) interface.

### Active Effect Change Properties

Each change in an Active Effect includes the following properties:

- **`key`**:  
  A string representing the attribute path within the Actor or Item data that the change modifies.  
  _Example_: `system.attributes.strength.value`

- **`value`**:  
  The value to apply via the change. This is interpreted by the target property's corresponding `DataField` instance, if one is defined.

- **`mode`**:  
  An integer (0–5) indicating how the `value` should be applied to the target property.  
  The available modes are defined in [`CONST.ACTIVE_EFFECT_MODES`](https://foundryvtt.com/api/v13/variables/CONST.ACTIVE_EFFECT_MODES.html).


| Mode | Name        | Description | Example(s) |
|------|-------------|-----------|------------|
| `0`  | **Custom**  | The effect is handled programmatically by a system or module. No automatic application is performed. Use the [`applyActiveEffect` hook](https://foundryvtt.wiki/en/development/api/document/active-effect#applyactiveeffect-hook) to define custom logic. |  |
| `1`  | **Multiply** | Multiplies the base value by the effect value. Only applies to numeric values. | `2 * 3 = 6` |
| `2`  | **Add**      | Adds the effect value to the base value (numeric), or concatenates strings. | `2 + 3 = 5`<br>`"Hello" + " World" = "Hello World"` |
| `3`  | **Downgrade** | Sets the result to the **lower** of the base and effect values. | `min(2, 0) = 0`<br>`min(2, 3) = 2` |
| `4`  | **Upgrade**   | Sets the result to the **higher** of the base and effect values. | `max(2, 4) = 4`<br>`max(2, 1) = 2` |
| `5`  | **Override**  | Replaces the base value entirely with the effect value. | `2 → 4` |

    
- The `priority` of a change is not directly configurable via the Active Effect sheet. By default, it is calculated as `10 × mode`.
 Changes are applied in ascending order of priority:
	- Lower modes (e.g., `Multiply`) are processed first.
	- Higher modes (e.g., `Override`) are applied last, allowing them to supersede earlier modifications.

### Only Modify Actors

There's two important limitations to the core implementation of active effects:
- Active Effects don't modify token properties
- Active Effects don't modify Items.

An intrepid developer can work around this - there are a number of systems and modules that do so - but these actions are not natively supported by foundry.

## API Interactions

Here are the most common things to know about 

### CRUD Operations for Active Effects

Active Effects inherit all standard `Document` and `ClientDocument` methods, enabling full Create, Read, Update, and Delete functionality.

#### Creating an Active Effect

A typical [ApplicationClickAction]### CRUD Operations for Active Effects

Active Effects inherit all standard `Document` and `ClientDocument` methods, enabling full Create, Read, Update, and Delete functionality.

#### Creating an Active Effect

A typical [ApplicationClickAction](https://foundryvtt.com/api/v13/types/foundry.applications.types.ApplicationClickAction.html) for creating an Active Effect might look like this:

```js
#onCreateActiveEffect(event, target) {
  const cls = foundry.utils.getDocumentClass("ActiveEffect");
  cls.createDialog({}, { parent: this.document });
}
```

This uses [`ClientDocument.createDialog`](https://foundryvtt.com/api/v13/classes/foundry.ClientDocument.html#createdialog) to open the creation dialog with the current document as its parent.

#### Other Common Operations

- **Read**: Use `document.effects.get(effectId)` to retrieve a specific Active Effect.
- **Update**: Call `effect.update(data)` to modify an effect's properties.
- **Render**: Use `effect.render({ force: true })` to open the effect's sheet UI.
- **Delete**: Call `effect.delete()` to remove the effect from its parent document.

All other standard document operations are also available, allowing for full programmatic control over Active Effects.


### Actor#applyActiveEffects

The Actor class have the method [`Actor#applyActiveEffects`](https://foundryvtt.com/api/v13/classes/foundry.documents.Actor.html#applyactiveeffects), this method is used to apply any transformation to the Actor's data caused by a ActiveEffect.
It may be of interest to edit this method to condition the changes applied to the actor, reorder the prioritization, etc.
Something similar to:
```js
  applyActiveEffects() {
    for ( const effect of this.allApplicableEffects() ) {
      //Determine whether this Active Effect is suppressed or not.
      effect.determineSuppression(); 
    }
    return super.applyActiveEffects();
  }
```

### applyActiveEffect Hook
The [`applyActiveEffect`](https://foundryvtt.com/api/v13/functions/hookEvents.applyActiveEffect.html) hook is triggered whenever an `ActiveEffect` with the `CUSTOM` mode is applied. This hook allows system or module developers to implement custom handling for such change.
#### Hook Parameters
The hookedFunction will have the following parameters
```js
/**
 * A hook event that fires when a custom active effect is applied.
 * @event applyActiveEffect
 * @category Active Effects
 * @param {Actor} actor - The actor the active effect is being applied to
 * @param {EffectChangeData} change - The change data being applied
 * @param {*} current - The current value being modified
 * @param {*} delta - The parsed value of the change object
 * @param {object} changes - An object which accumulates changes to be applied
 */
function applyActiveEffect(actor, change, current, delta, changes) {}
```

#### Usage
System or module developers can use this hook to intercept and process custom logic for CUSTOM mode changes. Modifications to the changes object will be reflected in the final application of the effect.

A practical example of a custom handle could be:

```js
// Calculates the average of the current value and the delta value
Hooks.on("applyActiveEffect", (actor, change, current, delta, changes) => {
  let update;
  const ct = foundry.utils.getType(current);

  switch (ct) {
    case "number": {
      // Average of two numbers
      update = (current + delta) / 2;
      break;
    }
    default: {
      // Unsupported type, log a warning
      console.warn(
        `ActiveEffect "${change.key}" cannot calculate average for type: ${ct}`
      );
      update = current;
      break;
    }
  }

  changes[change.key] = update;
});

```

## Custom ActiveEffect Subtypes (Introduced in V12)

> See https://foundryvtt.com/article/module-sub-types/

Starting in **Version 12**, Foundry VTT allows **modules and game systems** to define custom `ActiveEffect` subtypes. This feature enables developers to extend the behavior and structure of Active Effects beyond the default implementation, making them more flexible and context-aware.

### Key Features
- **Subtype-Specific Data**  
  Custom subtypes can store additional or specialized data in the document’s `system` field. This allows for tailored configurations, such as:
  - Conditional triggers
  - Custom animations or visual effects
  - Integration with system-specific mechanics (e.g., spellcasting, buffs, debuffs)

- **Registration**  
  Subtypes are registered using the `CONFIG.ActiveEffect.documentClass` map. For example:
  ```js
  CONFIG.ActiveEffect.dataModels["mySubtype"] = MyCustomEffectSubtypeClass;
  ```

- **Usage**  
  When creating an Active Effect, specify the `type` field to use your custom subtype:
  ```js
  const effectData = {
    type: "mySubtype",
    system: {
      customProperty: "value"
    }
  };
  await actor.createEmbeddedDocuments("ActiveEffect", [effectData]);
  ```

### Example Use Cases

- A **status effect module** could introduce a `ConditionEffect` subtype with icons, severity levels, and stacking rules.
- A **combat automation tool** might use a `TriggerEffect` subtype that activates based on initiative or damage thresholds.

## Specific Use Cases
###  Attribute Key Suggestions for Active Effects
One common challenge when configuring Active Effects is knowing which attribute keys are valid and will produce meaningful changes. Since effects rely on precise data paths (e.g., `system.attributes.hp.value`), it's easy for users to make mistakes or feel uncertain about what to enter.

To improve usability, you can enhance the Active Effect configuration dialog by injecting a `<datalist>` element that provides autocomplete suggestions for valid attribute keys. This helps users discover supported paths and reduces trial-and-error.

#### Implementation Example

Add the following hook to your system or module to attach a `<datalist>` to the `key` input field:

```js
/**
 * Adds a datalist helper for suggesting valid Actor attribute keys in the ActiveEffect config dialog.
 */
Hooks.on("renderActiveEffectConfig", (activeEffectConfig, html, data) => {
  const effectsSection = html.querySelector("section[data-tab='effects']");
  if (!effectsSection) return;

  const datalist = document.createElement("datalist");
  datalist.id = "attribute-key-list";

  const inputFields = effectsSection.querySelectorAll(".key input");
  inputFields.forEach(input => input.setAttribute("list", datalist.id));

  const attributeKeys = [];

  for (const model of Object.values(CONFIG.Actor.dataModels)) {
    model.schema.apply(function() {
      if (!(this instanceof foundry.data.fields.SchemaField)) {
        attributeKeys.push({
          key: this.fieldPath,
          label: this.label, 
        });
      }
    });
  }

  attributeKeys
    .sort((a, b) => a.key.localeCompare(b.key))
    .forEach(({ key, label }) => {
      const option = document.createElement("option");
      option.value = key;
      if (label) option.label = label;
      datalist.appendChild(option);
    });

  effectsSection.appendChild(datalist);
});
```

### Status Effects
> Stub
> This section is a stub, you can help by contributing to it.

___
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.
