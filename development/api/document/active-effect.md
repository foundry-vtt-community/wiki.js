---
title: Active Effect
description: An embedded document that can be used to modify the attributes of other documents during prepareData
published: true
date: 2026-04-19T04:08:25.854Z
tags: documentation
editor: markdown
dateCreated: 2024-06-08T05:46:12.955Z
---

![Up to date as of v14](https://img.shields.io/badge/FoundryVTT-v14-informational)

Active Effects are a built-in way for users, systems, and modules to dynamically alter actor properties.

**Foundry VTT API Documentation Links**  

*Specific Document Classes* 
- **[ActiveEffect](https://foundryvtt.com/api/v14/classes/foundry.documents.ActiveEffect.html)**  
  *Client-side class for Active Effects*  
- **[BaseActiveEffect](https://foundryvtt.com/api/v14/classes/foundry.documents.BaseActiveEffect.html)**  
  *Abstract base class for Active Effects.*  

*General Document Classes*
- **[Document](https://foundryvtt.com/api/v14/classes/foundry.abstract.Document.html)**  
  *Parent class for all Foundry document types.*  
- **[ClientDocument](https://foundryvtt.com/api/v14/classes/foundry.ClientDocument.html)**  
  *Class that extends the base Document class by adding client-specific behaviors.*  

#### **Applications Classes**  
- **[ActiveEffectConfig](https://foundryvtt.com/api/v14/classes/foundry.applications.sheets.ActiveEffectConfig.html)**  

**Legend**
```js
ActiveEffect.fromStatusEffect // `.` indicates static method or property
ActiveEffect#duration // `#` indicates instance method or property
```

## Overview

The ActiveEffect document class provides extensive built-in functionality; it comes with a core-defined sheet.
Most systems only need to provide some buttons on Actors and Items Sheets for basic CRUD operations. The actual changes are applied in the `ClientDocument#prepareEmbeddedDocuments` method during the prepareData cycle, altering the in-memory properties without changing the underlying source data.
This allows for effective representation of all sorts of changes, from a *shield* spell that improves defenses to a shapeshift that overhauls almost all fo the actor's statistics.

## Key Concepts

When working with Active Effects, you should keep the following in mind.

### Active Effect Embedding Behavior

- Active effects on an item are **copied to the parent actor** when the item is added to the actor.
- This occurs **only if** the Active Effect's `transfer` property is set to `true`.
- These copied effects are **deleted** when the item is removed from the actor.


### Base Schema

The valid properties for active effects are summed up by the [ActiveEffectData](https://foundryvtt.com/api/v14/interfaces/foundry.documents.types.ActiveEffectData.html) interface.

#### EffectChangeData

The core functionality of Active Effects is provided by the `changes` array, which takes the form of the [EffectChangeData](https://foundryvtt.com/api/v14/interfaces/foundry.documents.types.EffectChangeData.html) interface.

### Active Effect Change Properties

Each change in an Active Effect includes the following properties:

- **`key`**:  
  A string representing the attribute path within the Actor or Item data that the change modifies.  
  _Example_: `system.attributes.strength.value`

- **`value`**:  
  The value to be applied by the change. This is interpreted by the target property's DataField instance if one exists.
>   Notably, this field (since v14) supports dynamic variables (e.g., @some.foo), which are automatically resolved and replaced with the corresponding document data during the application process.
{.is-info}


- **`type`**:  
  An string indicating how the `value` should be applied to the target property.  
  The available type are the keys of [`CONST.ACTIVE_EFFECT_CHANGE_TYPES`](https://foundryvtt.com/api/v14/variables/CONST.ACTIVE_EFFECT_CHANGE_TYPES.html).
  
>  You can add your own types, simply need inject your configuration into `CONFIG.ActiveEffect.changeTypes` during the init hook of your package. (See `[wip]`)
{.is-info}

| Key        | Description | Example(s) |
|-------------|-----------|------------|
| **custom**  | The effect is handled programmatically by a system or module. No automatic application is performed. Use the [`applyActiveEffect` hook](https://foundryvtt.wiki/en/development/api/document/active-effect#applyactiveeffect-hook) to define custom logic. |  |
| **multiply** | Multiplies the base value by the effect value. Only applies to numeric values. | `2 * 3 = 6` |
| **add**      | Sums two values, concatenates strings, pushes onto Arrays, or adds to Sets. | `2 + 3 = 5`<br>`"Hello" + " World" = "Hello World"` |
| **subtract**      | Subtracts a numeric change values from target values, splices values from Arrays, or deletes an element from Sets. | `3 - 2 = 1`<br>`Set<"hello"|"world"> - "world" = Set<"hello">` |
| **downgrade** | Sets the result to the **lower** of the base and effect values. | `min(2, 0) = 0`<br>`min(2, 3) = 2` |
| **upgrade**   | Sets the result to the **higher** of the base and effect values. | `max(2, 4) = 4`<br>`max(2, 1) = 2` |
| **override**  | Replaces the base value entirely with the effect value. | `2 → 4` |

- The `phase` determines when then change is applied within the application lifecycle. You can think of phases as high-level priority groups that follow a strict order.
FoundryVTT includes two preconfigured phases: "initial" and "final" phases. Additionally packages can introduce custom phases.
    
- The `priority` of a change is the order in which this change is applied among other changes in a common phase: a null value is initialized to its default priority(the values of [`CONST.ACTIVE_EFFECT_CHANGE_TYPES`](https://foundryvtt.com/api/v14/variables/CONST.ACTIVE_EFFECT_CHANGE_TYPES.html)).
 Changes are applied in ascending order of priority:
	- Lower modes (e.g., `Custom`) are processed first.
	- Higher modes (e.g., `Override`) are applied last, allowing them to supersede earlier modifications.

### Supported Documents for ActiveEffect Modification

By default, ActiveEffect objects can modify `Actor` and `TokenDocument` data. To modify an Actor's Token specifically, you must prefix the change path with `token.`.
However, developers can extend this functionality within their own packages to allow effects to modify `Item` documents as well. (See `[wip]`)

## API Interactions

Here are the most common things to know about 

### CRUD Operations for Active Effects

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

Both the `Actor` and `TokenDocument` classes utilize the `applyActiveEffects` method to process transformations triggered by ActiveEffect instances.
#### Overriding `applyActiveEffects`
By overriding this method, you can inject custom logic to filter changes, adjust prioritization, or conditionally suppress effects before they are finalized.
```js
applyActiveEffects(phase) {
  // Logic to determine if effects should be suppressed or ignored
  for ( const effect of this.allApplicableEffects() ) {
    effect.determineSuppression(phase); 
  }

  // Execute the standard application logic
  return super.applyActiveEffects(phase);
}
```

### Custom Changes
FoundryVTT provides two primary ways to implement custom logic for Active Effects. You can either register a global Change Type in the `CONFIG` or use the `applyActiveEffect` Hook to intercept changes dynamically.

In the examples below, we will implement a POW (Power) change type, which raises the current attribute value to the power of the effect's value (i.e., $current^{change}$).

#### The register the custom type on `CONFIG`
Registering a custom type in `CONFIG.ActiveEffect.changeTypes` is the most integrated approach. This allows your custom logic to be recognized by the core logic and appear in ActiveEffectConfig selects.

When registering a custom change type you must provide a object that follows the [ActiveEffectChangeTypeConfig](https://foundryvtt.com/api/v14/interfaces/CONFIG.ActiveEffectChangeTypeConfig.html) interface.
This record requires a `label` (a locatable or located string) and a `defaultPriority` (a number determining the application order relative to other types).
Additionally, you can provide an optional `handler` function to define the programmatic logic for the transformation, and a `render` function to customize how the change input is displayed on the ActiveEffectConfig.
```js
Hooks.once("init", () => {
  CONFIG.ActiveEffect.changeTypes["MY_MODULE.POW"] = {
    label: "Power",
    defaultPriority: 40,
    handler: (targetDoc, change, { replacementData = {} } = {}) => {
      const exponent =
        foundry.dice.Roll.defaultImplementation.replaceFormulaData(
          change.value,
          replacementData,
          { recursive: true },
        );
     
      const base = foundry.utils.getProperty(targetDoc, change.key);

      if (typeof base !== "number" || typeof exponent !== "number")
        return current;

      const power = Math.pow(base, exponent);
      foundry.utils.setProperty(targetDoc, change.key, power);
    },
  };
});

```
#### The applyActiveEffect Hook
The [`applyActiveEffect`](https://foundryvtt.com/api/v14/functions/hookEvents.applyActiveEffect.html) hook triggers whenever an ActiveEffect with the CUSTOM mode is applied. This is ideal for one-off logic or complex calculations that don't need a dedicated UI type.
##### Hook Parameters
The callback function receives the following:
- `targetDoc`: The Document (Actor, Item, or TokenDocument) being modified.
- `change`: The specific change data object.
- `current`: The current value of the field before this change.
- `delta`: The parsed value of the change.
- `changes`: The object accumulating all changes to be applied to the document.

##### Example
In this example, we check if the change key is tagged with a custom suffix or if the system is expecting a power calculation.

```js
Hooks.on("applyActiveEffect", (targetDoc, change, current, delta, changes) => {
  if (!change.key.includes("MY_MODULE_POW")) return;
  const actualKey = change.key.replace("MY_MODULE_POW", "");
  const base = foundry.utils.getProperty(targetDoc, actualKey);

  // In the hook, 'delta' is already the parsed value of 'change.value'
  // and 'current' is the value of the target key before this change.
  // We just need validate that both are numbers.
  if (typeof base !== "number" || typeof delta !== "number") {
    return;
  }

  changes[targetPath] = Math.pow(base, delta);
});
```

## ActiveEffect Subtypes

> See https://foundryvtt.com/article/module-sub-types/

Foundry VTT allows **modules and game systems** to define custom `ActiveEffect` subtypes. This feature enables developers to extend the behavior and structure of Active Effects beyond the default implementation, making them more flexible and context-aware.

### Key Features
- **Subtype-Specific Data**  
  Custom subtypes can store additional or specialized data in the document’s `system` field. This allows for tailored configurations, such as:
  - Conditional triggers
  - Custom animations or visual effects
  - Integration with system-specific mechanics (e.g., spellcasting, buffs, debuffs)

- **Registration**  
  Subtypes are registered in the `CONFIG.ActiveEffect.documentClass` map. For example:
  ```js
  CONFIG.ActiveEffect.dataModels["mySubtype"] = MyCustomEffectSubtype;
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
> Stub
> This section is a stub, you can help by contributing to it.

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
