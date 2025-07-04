---
title: ApplicationV2 Conversion Guide
description: Comprehensive guide for converting Applications to ApplicationV2.
published: true
date: 2025-06-25T08:34:58.163Z
tags: 
editor: markdown
dateCreated: 2025-06-24T18:01:11.261Z
---

**Note:** This guide was originally made for the DCC system, so some terms will mention DCC and you should use your own package's terms instead.

## Table of Contents

1. [Critical V13 Breaking Changes](#critical-v13-breaking-changes)
2. [Basic ApplicationV2 Migration](#basic-applicationv2-migration)
3. [Form and Dialog Configuration](#form-and-dialog-configuration)
4. [Event System Migration](#event-system-migration)
5. [Tab System Migration](#tab-system-migration)
6. [Drag and Drop Migration](#drag-and-drop-migration)
7. [Template and Editor Migration](#template-and-editor-migration)
8. [Complete Migration Examples](#complete-migration-examples)
9. [Testing and Validation](#testing-and-validation)
10. [Migration Checklist](#migration-checklist)

---

## Critical V13 Breaking Changes

### HTML Element Changes (CRITICAL)

**V12 vs V13 HTML Parameter Change:**
- **V12**: `html` parameters in hooks and `activateListeners()` are **jQuery objects**
- **V13**: `html` parameters are **plain DOM elements** (no jQuery methods)

**Required V13 Conversions:**
```javascript
// V12 jQuery-style (BREAKS in V13)
html.find('.my-button').click(handler)
html.html('<p>Content</p>')
html.addClass('active')

// V13 DOM-style (V13 compatible)
html.querySelector('.my-button').addEventListener('click', handler)
html.innerHTML = '<p>Content</p>'
html.classList.add('active')
```

## CSS Layering and Variables

***CRITICAL**: V13 introduces CSS Layers, which is good, but means that your existing styling that relied Foundry styles is probably broken.
To fix this, you need to implement your own theming using CSS variables and layers. See "Implementing Themes" section below for details on how to do this.

* Checkboxes are now using FontAwesome icons instead of checkboxes, and styling them is now very tricky. Hopefully someone will write a guide on how to do this the right way.
* Important to note is that you must style the "before' and "after" pseudo-elements of the checkbox, not the checkbox itself.
* You will also have to style the "active" state of the checkbox for before and after pseudo-elements.
* Many other style changes have been made that may affect your system, so be sure to test all UI elements thoroughly.

```css

---

## Basic ApplicationV2 Migration

### 1. Change Class Inheritance

**For Actor Sheets:**
```javascript
// V1 (Old)
class DCCActorSheet extends FormApplication {

// V2 (New)
const { HandlebarsApplicationMixin } = foundry.applications.api
const { ActorSheetV2 } = foundry.applications.sheets

class DCCActorSheet extends HandlebarsApplicationMixin(ActorSheetV2) {
```

**For Dialogs and Config Applications:**
```javascript
// V1 (Old)
class MyDialog extends FormApplication {

// V2 (New)
const { ApplicationV2, HandlebarsApplicationMixin } = foundry.applications.api

class MyDialog extends HandlebarsApplicationMixin(ApplicationV2) {
```

### 2. Convert defaultOptions to DEFAULT_OPTIONS

```javascript
// V1 Pattern (Old)
static get defaultOptions() {
  return foundry.utils.mergeObject(super.defaultOptions, {
    classes: ['dcc', 'sheet', 'actor'],
    template: 'systems/dcc/templates/actor-sheet.html',
    width: 600,
    height: 600,
    resizable: true
  })
}

// V2 Pattern (New)
/** @inheritDoc */
static DEFAULT_OPTIONS = {
  classes: ['dcc', 'sheet', 'actor', 'themed', 'theme-light'], // ⚠️ themed classes are workaround only
  position: {
    width: 600,
    height: 600
  },
  window: {
    resizable: true,
    title: 'DCC.ActorSheetTitle' // Just the localization key
  }
}
```

### 3. Rename getData() to _prepareContext()

```javascript
// V1 Pattern (Old)
getData() {
  const context = super.getData()
  context.data = this.object.system
  context.flags = this.object.flags
  return context
}

// V2 Pattern (New)
async _prepareContext(options) {
  const context = await super._prepareContext(options)
  context.system = this.actor.system  // Note: this.actor for ActorSheetV2
  context.flags = this.actor.flags     // or this.document for ApplicationV2
  return context
}
```

### 4. Define Template Parts

```javascript
/** @inheritDoc */
static PARTS = {
  form: {
    template: 'systems/dcc/templates/dialog-actor-config.html'
  }
}
```

---

## Form and Dialog Configuration

### Form Tag Requirements

**CRITICAL**: Dialogs and forms MUST include `tag: 'form'` in DEFAULT_OPTIONS:

```javascript
static DEFAULT_OPTIONS = {
  tag: 'form',  // REQUIRED for dialogs and forms
  form: {
    submitOnChange: false,
    closeOnSubmit: true
  }
}
```

**Template Requirements**: When using `tag: 'form'`, templates should NOT contain `<form>` elements:

```html
<!-- WRONG: Nested forms are invalid HTML -->
<form class="config-dialog">
  <!-- content -->
</form>

<!-- CORRECT: Use section or div -->
<section class="config-dialog">
  <!-- content -->
</section>
```

### Form Submission Patterns

**Pattern 1: Static Form Handler (Recommended)**
```javascript
static DEFAULT_OPTIONS = {
  tag: 'form',
  form: {
    handler: MyClass.#onSubmitForm,
    closeOnSubmit: true,
    submitOnChange: false
  }
}

/**
 * Handle form submission
 * @this {MyClass}
 * @param {SubmitEvent} event
 * @param {HTMLFormElement} form
 * @param {FormDataExtended} formData
 */
static async #onSubmitForm(event, form, formData) {
  event.preventDefault()
  await this.document.update(formData.object) // Note: formData.object
}
```

### Inline Item Modifications on Actor Sheets

When you need to edit items inline on an actor sheet (e.g., weapons, equipment, spells), you must handle these updates separately from the main actor update. This is because ApplicationV2's form validation will strip out nested item data.

**Template Pattern:**
```html
{{!-- Use "items" prefix for inline item edits --}}
<li class="weapon" data-item-id="{{weapon._id}}">
  <input type="text"
         name="items.{{weapon._id}}.name"
         value="{{weapon.name}}"/>
  <input type="checkbox"
         name="items.{{weapon._id}}.system.equipped"
         {{checked weapon.system.equipped}}/>
  <input type="text"
         name="items.{{weapon._id}}.system.damage"
         value="{{weapon.system.damage}}"/>
</li>
```

**Implementation Pattern (actor-sheet.js:1025-1066):**
```javascript
/** @override */
_processFormData(event, form, formData) {
  // Extract the raw form data object BEFORE validation strips out items
  const expanded = foundry.utils.expandObject(formData.object)

  // Handle items separately if they exist
  if (expanded.items) {
    // Store for later processing
    this._pendingItemUpdates = Object.entries(expanded.items).map(([id, itemData]) => ({
      _id: id,
      ...itemData
    }))

    // Remove from the expanded object
    delete expanded.items

    // Flatten and replace the existing formData.object properties
    const flattened = foundry.utils.flattenObject(expanded)

    // Clear existing object and repopulate (since we can't reassign)
    for (const key in formData.object) {
      delete formData.object[key]
    }
    Object.assign(formData.object, flattened)
  }

  // Call parent with modified formData
  return super._processFormData(event, form, formData)
}

/** @override */
async _processSubmitData(event, form, formData) {
  // Process the actor data normally
  const result = await super._processSubmitData(event, form, formData)

  // Now handle any pending item updates
  if (this._pendingItemUpdates?.length > 0) {
    await this.document.updateEmbeddedDocuments('Item', this._pendingItemUpdates)
    delete this._pendingItemUpdates // Clean up
  }

  return result
}
```

**Key Points:**
- Use `items.{{id}}.property` naming convention in templates
- Override `_processFormData` to extract item data before validation
- Store item updates temporarily in `this._pendingItemUpdates`
- Override `_processSubmitData` to apply item updates after actor update
- Clean up temporary storage after updates complete

### Constructor Pattern Changes

**CRITICAL**: ApplicationV2 constructor takes ALL parameters in a single options object:

```javascript
// V1 (FormApplication) - WRONG for V2
new DCCActorConfig(this.actor, { position: { ... } })

// V2 (ApplicationV2) - CORRECT
new DCCActorConfig({
  document: this.actor,  // Document goes INSIDE options
  position: { ... }
})
```

### Document Access in V2

```javascript
// In your ApplicationV2 class
get document() {
  return this.options.document  // Document comes from options
}

// Usage in methods
async _prepareContext(options) {
  const context = await super._prepareContext(options)
  context.actor = this.document
  return context
}
```

---

## Event System Migration

### Converting activateListeners to Actions

**V1 Pattern (Old):**
```javascript
activateListeners(html) {
  super.activateListeners(html)

  html.find('.ability-label').click(this._onRollAbilityCheck.bind(this))
  html.find('.item-create').click(this._onItemCreate.bind(this))
  html.find('.item-delete').click(this._onItemDelete.bind(this))
}

_onRollAbilityCheck(event) {
  event.preventDefault()
  const ability = event.currentTarget.dataset.ability
  this.actor.rollAbilityCheck(ability)
}
```

**V2 Pattern (New):**
```javascript
static DEFAULT_OPTIONS = {
  actions: {
    rollAbilityCheck: this.#rollAbilityCheck,
    itemCreate: this.#itemCreate,
    itemDelete: this.#itemDelete
  }
}

/**
 * Handle ability check rolls
 * @this {DCCActorSheet}
 * @param {PointerEvent} event
 * @param {HTMLElement} target
 */
static async #rollAbilityCheck(event, target) {
  event.preventDefault()
  const ability = target.dataset.ability
  await this.actor.rollAbilityCheck(ability)
}
```

### Template Changes for Actions

**V1 HTML (Old):**
```html
<span class="ability-label" data-ability="str">Strength</span>
<button class="item-create" data-type="weapon">Create Weapon</button>
```

**V2 HTML (New):**
```html
<span data-action="rollAbilityCheck" data-ability="str">Strength</span>
<button data-action="itemCreate" data-type="weapon">Create Weapon</button>
```

**Key Points:**
- `data-action` value must match the key in the `actions` object
- Other `data-*` attributes are passed to the handler via `target` parameter
- Remove JavaScript-specific classes like `.item-create` - use semantic classes for styling only

### Image Editing Migration

**V1 Pattern (Old):**
```html
<img src="{{img}}" data-edit="img" alt="Portrait">
```

**V2 Pattern (New):**
```html
<img src="{{img}}" data-action="editImage" data-field="img" alt="Portrait">
```

```javascript
static DEFAULT_OPTIONS = {
  actions: {
    editImage: this.#onEditImage
  }
}

static async #onEditImage(event, target) {
  const field = target.dataset.field || "img"
  const current = foundry.utils.getProperty(this.document, field)

  const fp = new foundry.applications.apps.FilePicker({
    type: "image",
    current: current,
    callback: (path) => this.document.update({ [field]: path })
  })

  fp.render(true)
}
```

---

## Tab System Migration

### Basic Tab Configuration

**V1 Pattern (Old):**
```javascript
static get defaultOptions() {
  return foundry.utils.mergeObject(super.defaultOptions, {
    tabs: [{
      navSelector: '.tabs',
      contentSelector: '.tab-content',
      initial: 'character'
    }]
  })
}
```

**V2 Pattern (New):**
```javascript
static TABS = {
  sheet: {
    tabs: [
      { id: 'character', group: 'sheet', label: 'DCC.Character' },
      { id: 'equipment', group: 'sheet', label: 'DCC.Equipment' },
      { id: 'notes', group: 'sheet', label: 'DCC.Notes' }
    ],
    initial: 'character'
  }
}

static PARTS = {
  tabs: {
    template: 'systems/dcc/templates/actor-partial-tabs.html'
  },
  character: {
    template: 'systems/dcc/templates/actor-partial-pc-common.html'
  },
  equipment: {
    template: 'systems/dcc/templates/actor-partial-pc-equipment.html'
  },
  notes: {
    template: 'systems/dcc/templates/actor-partial-pc-notes.html'
  }
}
```

### Tab Template Requirements

**CRITICAL**: Tab templates MUST have proper structure:

```html
{{!-- Individual Tab Template --}}
<section class="tab {{tabs.character.id}} {{tabs.character.cssClass}}"
         data-tab="{{tabs.character.id}}"
         data-group="{{tabs.character.group}}">
  {{!-- Tab content here --}}
  <div class="character-details">
    <!-- form fields, etc. -->
  </div>
</section>
```

**Required Elements:**
- Root element must have `tab` CSS class
- Must include `data-tab="{{tabs.tabId.id}}"` and `data-group="{{tabs.tabId.group}}"`
- Must include `{{tabs.tabId.cssClass}}` for dynamic CSS classes
- Use `<section>` or `<div>` (never `<form>` inside a form)

### Dynamic Tab Configuration

For applications where tabs vary based on document type:

```javascript
_getTabsConfig(group) {
  const tabs = foundry.utils.deepClone(super._getTabsConfig(group))

  // Modify tabs based on document properties
  if (this.document.type === 'weapon') {
    tabs.tabs.push({ id: 'combat', group: 'sheet', label: 'DCC.Combat' })
  }

  return tabs
}
```

### ⚠️ CSS Warning for Tabs

**Problem**: If parent elements have `display: grid` or `display: flex`, ALL tabs will be visible at once.

**Solution**: Never set `display` properties on elements that directly contain tabs:

```css
/* WRONG - Breaks tab switching */
.sheet-body { display: grid; }

/* CORRECT - Apply to inner elements */
.tab-content-wrapper { /* no display here */ }
.character-grid { display: grid; }
```

---

## Drag and Drop Migration

### Understanding the V13 Drag/Drop Architecture

**CRITICAL INSIGHT**: FoundryVTT V13 has a two-tier drag/drop system:

1. **ApplicationV2 (Generic)**: Manual setup required, no automatic handlers
2. **ActorSheetV2 (Actor-specific)**: Automatic `.draggable` setup with basic item/effect support

#### ActorSheetV2 Automatic Features

If you're extending `ActorSheetV2`, you get these features automatically:

```javascript
const { ActorSheetV2 } = foundry.applications.sheets

class MyActorSheet extends HandlebarsApplicationMixin(ActorSheetV2) {
  // ActorSheetV2 automatically provides:
  // - DragDrop setup with '.draggable' selector in _onRender()
  // - Permission checks via _canDragStart/_canDragDrop (checks isEditable)
  // - Basic item dragging for elements with data-item-id
  // - Active effect dragging for elements with data-effect-id
  // - Item sorting within the same actor via _onSortItem
  // - Document drop handling with delegation to _onDropItem, _onDropActiveEffect, etc.
}
```

**What ActorSheetV2 gives you for free:**
- Elements with class `draggable` are automatically draggable
- Items with `data-item-id` create proper item drag data
- Active effects with `data-effect-id` create proper effect drag data
- Dropping items on the sheet automatically adds them to the actor
- Dropping items from the same actor triggers sorting via `_onSortItem`
- Permission checks based on `isEditable` property

#### When You Need Manual Setup

You need manual drag/drop setup when:
1. Using base `ApplicationV2` (not ActorSheetV2)
2. Creating custom drag types beyond basic items/effects
3. Need different selectors than `.draggable`
4. Need custom permission logic

### ApplicationV2 vs ActorSheetV2 Comparison

**V1 Pattern (Old - Automatic):**
```javascript
static get defaultOptions() {
  return foundry.utils.mergeObject(super.defaultOptions, {
    dragDrop: [{ dragSelector: '.item', dropSelector: '.item-list' }]
  })
}
// Framework automatically created and bound DragDrop handlers
```

**V2 ActorSheetV2 Pattern (Automatic for Basic Items):**
```javascript
const { ActorSheetV2 } = foundry.applications.sheets

class MyActorSheet extends HandlebarsApplicationMixin(ActorSheetV2) {
  // No drag/drop setup needed for basic items!
  // Just add class="draggable" and data-item-id to your templates
}
```

**Template for ActorSheetV2 Auto Drag/Drop:**
```html
<!-- ActorSheetV2 handles this automatically -->
<li class="item draggable" data-item-id="{{item._id}}">
  {{item.name}}
</li>

<!-- Active effects work automatically too -->
<li class="effect draggable" data-effect-id="{{effect._id}}">
  {{effect.name}}
</li>
```

**V2 ApplicationV2 Pattern (Manual Setup Required):**
```javascript
const { DragDrop } = foundry.applications.ux

class MyAppV2 extends HandlebarsApplicationMixin(ApplicationV2) {
  #dragDrop

  static DEFAULT_OPTIONS = {
    dragDrop: [{
      dragSelector: '[data-drag="true"]',
      dropSelector: '.drop-zone'
    }]
  }

  constructor(options = {}) {
    super(options)
    this.#dragDrop = this.#createDragDropHandlers()
  }

  #createDragDropHandlers() {
    return this.options.dragDrop.map((d) => {
      d.permissions = {
        dragstart: this._canDragStart.bind(this),
        drop: this._canDragDrop.bind(this)
      }
      d.callbacks = {
        dragstart: this._onDragStart.bind(this),
        dragover: this._onDragOver.bind(this),
        drop: this._onDrop.bind(this)
      }
      return new DragDrop(d)
    })
  }

  _onRender(context, options) {
    this.#dragDrop.forEach((d) => d.bind(this.element))
  }

  _canDragStart(selector) {
    return this.document.isOwner && this.isEditable
  }

  _canDragDrop(selector) {
    return this.document.isOwner && this.isEditable
  }

  _onDragOver(event) {
    // Optional: handle dragover events if needed
  }
}
```

### Important Implementation Notes

#### Event Handler Signatures
ApplicationV2 calls drag handlers with different signatures than v1:

```javascript
// ✅ Correct V2 signature - single event parameter
_onDragStart(event) {
  const element = event.currentTarget
  // Implementation
}

// ❌ Old v1 signature - two parameters (not used by ApplicationV2)
_onDragStart(event, target) {
  // This signature is not used by the framework
}
```

#### Static Method Access
If your drag handlers need to call static helper methods, make sure they're public:

```javascript
// ✅ Good - Public static method accessible from instance methods
static findDataset(element, attribute) {
  while (element && !(attribute in element.dataset)) {
    element = element.parentElement
  }
  return element?.dataset[attribute] || null
}

// ❌ Bad - Private static method not accessible from instances
static #findDataset(element, attribute) {
  // Can't call this from _onDragStart instance method
}

// Usage in drag handler
_onDragStart(event) {
  const itemId = MyClass.findDataset(event.currentTarget, 'itemId')
}
```

### Template Requirements

Add `data-drag="true"` to draggable elements:

```html
<ol class="items">
  {{#each items}}
  <li data-drag="true" data-item-id="{{this.id}}" data-drag-action="weapon">
    {{this.name}}
  </li>
  {{/each}}
</ol>
```

### DCC System Example: Hybrid Approach

The DCC system uses a hybrid approach that extends ActorSheetV2's automatic features with custom drag types:

```javascript
class DCCActorSheet extends HandlebarsApplicationMixin(ActorSheetV2) {
  #dragDrop

  static DEFAULT_OPTIONS = {
    // Custom selectors override ActorSheetV2 defaults
    dragDrop: [{
      dragSelector: '[data-drag="true"]',  // Custom selector
      dropSelector: '.item-list, .weapon-list, .armor-list, .skill-list'
    }]
  }

  constructor(options = {}) {
    super(options)
    this.#dragDrop = this.#createDragDropHandlers()
  }

  _onRender(context, options) {
    // Manual setup replaces ActorSheetV2's automatic setup
    this.#dragDrop.forEach((d) => d.bind(this.element))
  }

  // Custom drag handler for DCC-specific drag types
  _onDragStart(event) {
    const li = event.currentTarget
    if (!li.dataset.drag) return

    const dragAction = li.dataset.dragAction
    let dragData = null

    switch (dragAction) {
      case 'ability': {
        // Custom DCC ability check macro creation
        const abilityId = DCCActorSheet.findDataset(event.currentTarget, 'ability')
        dragData = {
          type: 'Ability',
          actorId: this.actor.id,
          data: { abilityId }
        }
        break
      }
      case 'spellCheck': {
        // Custom DCC spell check macro creation
        const ability = DCCActorSheet.findDataset(event.currentTarget, 'ability')
        dragData = {
          type: 'Spell Check',
          actorId: this.actor.id,
          data: { ability }
        }
        break
      }
      case 'weapon': {
        // Falls back to ActorSheetV2 item handling if needed
        const itemId = DCCActorSheet.findDataset(event.currentTarget, 'itemId')
        const weapon = this.actor.items.get(itemId)
        if (weapon) {
          dragData = Object.assign(weapon.toDragData(), {
            dccType: 'Weapon',
            actorId: this.actor.id
          })
        }
        break
      }
    }

    if (dragData) {
      if (this.actor.isToken) dragData.tokenId = this.actor.token.id
      event.dataTransfer.setData('text/plain', JSON.stringify(dragData))
    }
  }

  _onDrop(event) {
    const data = foundry.applications.ux.TextEditor.getDragEventData(event)
    if (!data) return false

    // Convert DCC-specific drops back to standard Item drops
    if (data.type === 'DCC Item') {
      data.type = 'Item'
    }

    // Delegate to ActorSheetV2's built-in drop handling
    return super._onDrop?.(event)
  }
}
```

### Key Insight: Choose Your Approach

1. **For simple actor sheets**: Use ActorSheetV2 with `.draggable` class - no setup needed
2. **For custom drag types**: Override with manual setup (like DCC system)
3. **For non-actor applications**: Use full manual ApplicationV2 setup

### ActorSheetV2 Built-in Methods You Can Override

ActorSheetV2 provides these methods you can customize:

```javascript
// Permission checks (default: checks this.isEditable)
_canDragStart(selector) { return this.isEditable }
_canDragDrop(selector) { return this.isEditable }

// Drag start (default: handles items with data-item-id, effects with data-effect-id)
_onDragStart(event) { /* creates item/effect drag data */ }

// Drop handling (default: delegates to _onDropDocument)
_onDrop(event) { /* processes drop data, calls hooks */ }

// Document drop routing (default: routes to specific handlers)
_onDropDocument(event, document) { /* routes by document type */ }

// Item drops (default: creates item on actor or sorts within actor)
_onDropItem(event, item) { /* handles item creation/sorting */ }

// Item sorting (default: uses Foundry's integer sort algorithm)
_onSortItem(event, item) { /* reorders items within actor */ }
```

This hybrid approach allows you to leverage ActorSheetV2's built-in functionality while adding system-specific features.

### Understanding ActorSheetV2 vs DocumentSheetV2 vs ApplicationV2

FoundryVTT V13 provides a hierarchy of v2 application classes:

```
ApplicationV2 (Base)
├── DocumentSheetV2 (Document-specific)
│   └── ActorSheetV2 (Actor-specific)
└── DialogV2 (Modal dialogs)
```

#### ApplicationV2 (Base Class)
**Use for**: Custom applications, configuration dialogs, tools
**Provides**:
- Render state management (NONE, RENDERING, RENDERED, CLOSING, CLOSED, ERROR)
- Window framing and positioning
- Event handling and lifecycle management
- Tab system for multi-panel interfaces
- Action system for declarative event handling
- Form handling with validation

#### DocumentSheetV2 (Document Sheets)
**Use for**: Item sheets, journal entry sheets, other document types
**Extends ApplicationV2 with**:
- Document permissions and ownership checks (`isVisible`, `isEditable`)
- Form submission pipeline with validation
- Header controls (configure sheet, copy UUID, edit image, import from compendium)
- Sheet theming and styling support
- Document update/creation handling

#### ActorSheetV2 (Actor Sheets)
**Use for**: Actor character sheets
**Extends DocumentSheetV2 with**:
- **Automatic drag/drop setup** with `.draggable` selector
- Default handlers for items (`data-item-id`) and effects (`data-effect-id`)
- Item sorting within same actor using integer sort algorithm
- Document drop delegation system
- Additional header controls for token/portrait management
- Actor and token getters for convenient access

#### DialogV2 (Modal Dialogs)
**Use for**: User prompts, confirmations, input collection
**Extends ApplicationV2 with**:
- Button-based user interaction with callbacks
- Modal/non-modal display modes
- Keyboard navigation (Enter/Escape handling)
- Factory methods (`confirm()`, `prompt()`, `input()`, `wait()`)
- Promise-based async interaction patterns

### Choosing the Right Base Class

```javascript
// For actor sheets - use ActorSheetV2
class MyActorSheet extends HandlebarsApplicationMixin(ActorSheetV2) {
  // Gets automatic drag/drop, actor-specific features
}

// For item sheets - use DocumentSheetV2
class MyItemSheet extends HandlebarsApplicationMixin(DocumentSheetV2) {
  // Gets document handling, no automatic drag/drop
}

// For configuration dialogs - use ApplicationV2
class MyConfigDialog extends HandlebarsApplicationMixin(ApplicationV2) {
  // Gets basic application features only
}

// For user prompts - use DialogV2
class MyPrompt extends DialogV2 {
  // Gets modal behavior, button handling
}
```

---

## Template and Editor Migration

### ProseMirror Editor Migration

**V12 Pattern (Deprecated):**
```handlebars
{{editor corruptionHTML target="system.class.corruption" engine="prosemirror" button=true editable=editable}}
```

**V13 Pattern (Required):**
```handlebars
{{#if editable}}
  <prose-mirror
    name="system.class.corruption"
    button="true"
    editable="{{editable}}"
    toggled="false"
    value="{{system.class.corruption}}">
    {{{corruptionHTML}}}
  </prose-mirror>
{{else}}
  {{{corruptionHTML}}}
{{/if}}
```

### Context Preparation for Editors

```javascript
async _prepareContext(options) {
  const context = await super._prepareContext(options)

  // Enrich content for display
  context.corruptionHTML = await foundry.applications.ux.TextEditor.implementation.enrichHTML(
    this.document.system.class.corruption,
    {
      secrets: this.document.isOwner,
      relativeTo: this.document
    }
  )

  return context
}
```

### Static Initializer Configuration Issue

**CRITICAL**: Cannot use CONFIG values in static field initializers:

```javascript
// ❌ BROKEN - CONFIG.DCC is undefined during static initialization
static PARTS = {
  form: {
    template: CONFIG.DCC.templates.rollModifierDialog // ERROR
  }
}

// ✅ WORKING - Hard-coded template path
static PARTS = {
  form: {
    template: 'systems/dcc/templates/dialog-roll-modifiers.html'
  }
}
```

---

## Complete Migration Examples

### Basic Dialog Migration

**Before (V1):**
```javascript
class DCCActorConfig extends FormApplication {
  static get defaultOptions() {
    const options = super.defaultOptions
    options.template = 'systems/dcc/templates/dialog-actor-config.html'
    options.width = 380
    return options
  }

  get title() {
    return `${this.object.name}: ${game.i18n.localize('DCC.SheetConfig')}`
  }

  getData() {
    const data = this.object
    data.config = CONFIG.DCC
    return data
  }

  async _updateObject(event, formData) {
    await this.object.update(formData)
  }
}
```

**After (V2):**
```javascript
const { ApplicationV2, HandlebarsApplicationMixin } = foundry.applications.api

class DCCActorConfig extends HandlebarsApplicationMixin(ApplicationV2) {
  /** @inheritDoc */
  static DEFAULT_OPTIONS = {
    classes: ['dcc', 'sheet', 'actor-config'],
    tag: 'form',
    position: { width: 380, height: 'auto' },
    window: { title: 'DCC.SheetConfig', resizable: false },
    form: {
      handler: DCCActorConfig.#onSubmitForm,
      closeOnSubmit: true
    }
  }

  /** @inheritDoc */
  static PARTS = {
    form: {
      template: 'systems/dcc/templates/dialog-actor-config.html'
    }
  }

  get document() {
    return this.options.document
  }

  get title() {
    return `${this.document.name}: ${game.i18n.localize('DCC.SheetConfig')}`
  }

  async _prepareContext(options) {
    const context = await super._prepareContext(options)
    context.config = CONFIG.DCC
    context.actor = this.document
    return context
  }

  static async #onSubmitForm(event, form, formData) {
    event.preventDefault()
    await this.document.update(formData.object)
  }
}
```

### Actor Sheet with Tabs Migration

**Key Changes:**
1. Split single template into multiple tab templates
2. Define PARTS for each template
3. Define TABS configuration
4. Convert event handlers to actions
5. Update HTML templates with data-action attributes

See the complete examples in the original sections above for detailed implementations.
