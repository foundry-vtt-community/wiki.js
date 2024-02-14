---
title: Settings
description: 
published: true
date: 2024-02-14T22:05:54.875Z
tags: development, api, documentation, docs
editor: markdown
dateCreated: 2021-11-17T15:31:39.865Z
---

# Settings

![Up to date as of v11](https://img.shields.io/static/v1?label=FoundryVTT&message=v11&color=informational)

## Overview

Settings, like flags, are a way for packages to store and persist data. Settings are not tied to a specific document however, unlike flags. 

For the vast majority of use-cases, settings are intended to be modified by a UI, either a Menu or within the Module Settings panel itself. These settings are intended to be used to modify the functionality of a package, rather than store arbitrary data for that module or system.

---

## Key Concepts

### Scope
Also unlike flags they are able to leverage a 'scope' field to keep a piece of data specific to a user's localStorage (`scope: client`) or put that data in the world's database (`scope: world`).

### Permissions
There is a separate permission dedicated to governing which users can edit World Scoped settings. This is something to watch out for when using settings as your data storage medium.

---

## API Interactions

### Registering a Setting

*See [Setting Types](#setting-types) below for examples about the different types of settings that can be registered.*

> It is recommended to register settings during the `init` hook.
{.is-info}

All settings must be registered before they can be set or accessed. This needs to be done with [`game.settings.register`](https://foundryvtt.com/api/classes/client.ClientSettings.html#register), with `game.settings` being an instance of `ClientSettings`.

```js
/*
 * Create a custom config setting
 */
game.settings.register('myModuleName', 'mySettingName', {
  name: 'My Setting',
  hint: 'A description of the registered setting and its behavior.',
  scope: 'world',     // "world" = sync to db, "client" = local storage 
  config: true,       // false if you dont want it to show in module config
  type: Number,       // You want the primitive class, e.g. Number, not the name of the class as a string
  default: 0,
  onChange: value => { // value is the new value of the setting
    console.log(value)
  },
  requiresReload: true, // true if you want to prompt the user to reload
  /** Creates a select dropdown */
  choices: {
		1: "Option Label 1",
  	2: "Option Label 2",
  	3: "Option Label 3"
	},
  /** Number settings can have a range slider, with an optional step property */
  range: {
    min: 0,
    step: 2,
    max: 10
  },
  /** "audio", "image", "video", "imagevideo", "folder", "font", "graphics", "text", or "any" */
  filePicker: "any" 
});
```

#### Some registration notes
- `name` and `hint`, and the labels in `choices` are localized by the setting configuration application on render, so you can register settings in `init` and just pass a localizable string for those values
- `config` defaults to `undefined` which behaves the same as `false`
- `requiresReload` is useful for settings that make changes during the `init` or `setup` hooks.
- `scope` defaults to "client"
- You can pass a data model as the `type` for complex settings that need data validation.
- `filePicker`

#### Setting Types

The `type` of a setting is expected to be a constructor which is used when the setting's value is gotten. The 'normal' primitive constructors cover all basic use cases:
- [String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
- [Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
- [Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) - turns the setting into a checkbox
- [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

You can use fundamental language constructs as types
- [Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)

There's also some lesser used primitive types that are nevertheless eligible
- [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
- [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

It is possible however to leverage this functionality to do some advanced data manipulation with a complex setting object during the `get`. Doing so has some gotchas surrounding the `onChange` callback.

```js=
class SomeClass {
  constructor(parsedJson) {
    this.merged = parsedJson?.foo + parsedJson?.bar;
    this.foo = parsedJson?.foo;
    this.bar = parsedJson?.bar;
  }
}

game.settings.register('myModuleName', 'customClassSetting', { type: SomeClass });

game.settings.set('myModuleName', 'customClassSetting', {foo: 'foosius', bar: 'whatever'});

game.settings.get('myModuleName', 'customClassSetting').merged; // 'foosiuswhatever'
```

As an even more advanced use case, you could pass a `DataModel` as a setting to provide advanced validation; the type casting has a special case from these where it calls `YourDataModel.fromSource`.

#### Localization
When registering a setting, instead of passing a hard-coded string to `name` or `hint`, it is possible to pass a localization path to support translations. Both `name` and `hint` are run through [`game.i18n.localize`](https://foundryvtt.com/api/Localization.html#localize) before being displayed in the Setting UI.

### Setting a Setting's value


> Settings with `scope: world` cannot be `set` until the `ready` hook.
{.is-info}

A setting's value can be set with [`game.settings.set`](https://foundryvtt.com/api/classes/client.ClientSettings.html#set). It's important to note that a `scope: world` setting can only be set by a user with the "Modify Configuration Settings" permission (by default this is only Game Master and Assistant GM users), and that `scope: client` settings will only persist on the user's local machine.

```js
const whateverValue = 'foo';

game.settings.set('myModuleName','myModuleSetting', whateverValue);
```

#### Acceptable Values

> Easily handled data:
> - Objects and Arrays which do not contain functions
> - Strings
> - Numbers
> - Booleans
{.is-info}

A setting's value is stringified and stored as a string. This limits the possible values for a setting to anything which can survive a [`JSON.stringify()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) and subsequent [`JSON.parse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse).

Note that `JSON.stringify` will prefer to use a value's `toJSON()` method if one is available, all Foundry documents have such a method which strips the document back to its base data.

#### Type Constraints

If you wish to improve validation when updating a complex setting, you should consider a data model. If you're just using `String` or `Number`, it will run the new value through those primitives first before storing to the database (e.g. if the setting is `type: Number`, and someone passes `set(scope, key, "5")`, the setting will run `Number("5")` to cast the type).


### Getting a Setting's value

Settings can be read with [`game.settings.get`](https://foundryvtt.com/api/ClientSettings.html#get). 

```js
const someVariable = game.settings.get('myModuleName','myModuleSetting');

console.log(someVariable); // expected to be 'foo'
```

#### Returned Value Type

When getting a setting's value, the `type` of the setting registered is used by Core as a constructor for the returned value.

Example:
```js=
game.settings.register('myModuleName', 'myNumber', { type: Number });

game.settings.set('myModuleName', 'myNumber', 'some string');

game.settings.get('myModuleName', 'myNumber'); // NaN
```

For more information on the basic primitive constructors and how they convert values, [this article](https://javascript.info/type-conversions) has a good overview.

---

## Specific Use Cases


### Reacting to Setting Changes

There is no hook for when a setting changes, instead an `onChange` callback must be provided during registration (You of course *could* run `Hooks.call()` in that callback). This callback is fired after the setting has been set, meaning a `settings.get` inside this callback will return the new value of the setting, not the old.

The `onChange` callback does not fire if there are no differences between the `value` being `set` and the current value returned from `settings.get`.

This callback will fire on all clients for world scoped settings, but only locally for client scoped settings. Its only argument is the raw value of the setting that was `set`. 

> Because this `value` argument is not necessarily the same value that would be returned from `settings.get`, it is safer to get the new value in this callback if you intend to operate on it.
{.is-warning}

### Setting Menus

Sometimes a package is more complex than a few settings will allow a user to configure. In these cases it is recommended to register a settings menu with [`game.settings.registerMenu`](https://foundryvtt.com/api/classes/client.ClientSettings.html#registerMenu), and manage the configuration with a FormApplication or Dialog. Note that `registerMenu` does not register a setting by itself, simply a menu button.

Menus work best when used in conjunction with a registered setting of type `Object` which has been set to `config: false`. A menu could also be used to control many individual settings if desired.

```js
game.settings.registerMenu("myModule", "mySettingsMenu", {
  name: "My Settings Submenu",
  label: "Settings Menu Label",      // The text label used in the button
  hint: "A description of what will occur in the submenu dialog.",
  icon: "fas fa-bars",               // A Font Awesome icon used in the submenu button
  type: MySubmenuApplicationClass,   // A FormApplication subclass
  restricted: true                   // Restrict this submenu to gamemaster only?
});


game.settings.register('myModuleName', 'myComplexSettingName', {
  scope: 'world',     // "world" = sync to db, "client" = local storage 
  config: false,      // we will use the menu above to edit this setting
  type: Object,
  default: {},        // can be used to set up the default structure
});


/**
 * For more information about FormApplications, see:
 * https://hackmd.io/UsmsgTj6Qb6eDw3GTi5XCg
 */
class MySubmenuApplicationClass extends FormApplication {
  // lots of other things...
  
  getData() {
  	return game.settings.get('myModuleName', 'myComplexSettingName');
  }
  
  _updateObject(event, formData) {
    const data = expandObject(formData);
    game.settings.set('myModuleName', 'myComplexSettingName', data);
  }
}
```

#### Why would I want this?

FormApplications in particular allow you to run any logic you want during the `_updateObject` method. This could be leveraged to accomplish many things:

1. **Space.** You could use this to tidy up a lot of module settings which would otherwise take up an excessive amount of vertical space on the settings list.
2. **Validation.** Since you control the FormApplication's submit logic (`_updateObject`), you could run validation on user inputs before saving them to the setting value.
3. **Edit Setting Objects.** If you have a use case for a complex object of data being stored as a setting, a FormApplication menu would let your users manipulate that object directly.

---

## Troubleshooting

### Cannot set properties of undefined (setting 'key')

Happens when `game.settings.register` is called with an invalid first argument. This first argument is intended to be the `id` or `name` of an active package.

### Not a registered game setting

`game.settings.get` and `.set` both throw this error if the requested setting has not been registered yet.

### Cannot set a setting before Game is Ready

> Stub
> This section is a stub, you can help by contributing to it.

---

## Setting Registration Examples

This section will provide snippets and screenshots for the various common setting configurations. These snippets have the minimum number of options required to display the setting and may require tweaking for your specific use case.

### Boolean
![](https://i.imgur.com/RhykzIe.png)

```js
game.settings.register('core', 'myCheckbox', {
  name: 'My Boolean',
  config: true,
  type: Boolean,
});

game.settings.get('core', 'myCheckbox'); // false
```

### String/Text Input
![](https://i.imgur.com/EIj75IZ.png)

```js
game.settings.register('core', 'myInput', {
  name: 'My Text',
  config: true,
  type: String,
});

game.settings.get('core', 'myInput'); // 'Foo'
```

### Select Input
![](https://i.imgur.com/vTTz6Oh.png)

```js
game.settings.register('core', 'mySelect', {
  name: 'My Select',
  config: true,
  type: String,
  choices: {
    "a": "Option A",
    "b": "Option B"
  },
});

game.settings.get('core', 'mySelect'); // 'a'
```

There is no validation done on settings with `choices` to prevent `settings.set` from setting an invalid value.

The `key` of the `choices` object is what is stored in the setting when the user selects an option from the dropdown.

The `value`s of the `choices` object are automatically run through [`game.i18n.localize`](https://foundryvtt.com/api/Localization.html#localize) before being displayed in the Setting UI.

### Number Input
![](https://i.imgur.com/99B0E4V.png)

```js
game.settings.register('core', 'myNumber', {
  name: 'My Number',
  config: true,
  type: Number,
});

game.settings.get('core', 'myNumber'); // 1
```

### Number Range Slider
![](https://i.imgur.com/e4rxxYq.png)

```js
game.settings.register('core', 'myRange', {
  name: 'My Number Range',
  config: true,
  type: Number,
  range: {
    min: 0,
    max: 100,
    step: 10
  }
});

game.settings.get('core', 'myRange'); // 50
```

There is no validation done on settings with a `range` to prevent `settings.set` from setting a number that is out of bounds.

### File Picker
![](https://i.imgur.com/uzKq9h3.png)

```js
game.settings.register('core', 'myFile', {
  name: 'My File',
  config: true,
  type: String,
  filePicker: true,
});

game.settings.get('core', 'myFile'); // 'path/to/file'
```

#### File Picker Types

The following can be given to the `filePicker` option to change the behavior of the File Picker UI when it is opened. These are useful if you need the user to select only an image for instance.

- `'audio'`      - Displays audio files only
- `'image'`      - Displays image files only
- `'video'`      - Displays video files only
- `'imagevideo'` - Displays images and video files
- `'folder'` 		 - Allows selection of a directory (beware, it does not enforce directory selection)
- `'font'`       - Display font files only
- `'graphics'`   - Display 3D files only
- `'text'`       - Display text files only
- `'any'` - No different than `true`

#### Directory Picker

If the setting is registered with either the default `filePicker: true` or `filePicker: 'folder'` it is possible for a user to select a directory instead of a file. This is not forced however and the user might still select a file.

When saved, the directory path is the only string which is saved and does not contain information about the source which the directory was chosen from. Without strict assumptions and checking those assumptions, this kind of setting has a high chance of causing errors or unexpected behavior (e.g. creating a folder on the user's local storage instead of their configured S3 bucket).
