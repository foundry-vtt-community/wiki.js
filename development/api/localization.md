---
title: Localization
description: A helper class which assists with localization (i18n) and string translation
published: true
date: 2024-02-26T20:55:49.864Z
tags: 
editor: markdown
dateCreated: 2024-02-26T18:35:05.621Z
---

![Up to date as of v11](https://img.shields.io/badge/FoundryVTT-v11-informational)

Localization, or i18n, is how Foundry supports playing in many different languages.

**Official Documentation**
- [Knowledge Base](https://foundryvtt.com/article/localization/)
- [API](https://foundryvtt.com/api/classes/client.Localization.html)

## Overview

Localization allows Foundry to provide translated text content for UI elements on a per-application basis.

## Key Concepts

The Localization class is available at `yourFoundryInstall\resources\app\client\apps\i18n.js`

### Manifest Definition

Foundry loads localization from `.json` files provided by the core software, the system, any modules, and then the world. Packages can define localization files by adding a `languages` array to their manifest.json file and defining one or more objects inside that array:

```json
  "languages": [
    {
      "lang": "en",
      "name": "English",
      "path": "lang/en.json",
      "system": "system-id",
      "module": "module-id",
      "flags": {}
    }
  ],
```

The `lang` and `path` fields are mandatory; the first provides a valid [ISO 639-2 code](https://en.wikipedia.org/wiki/List_of_ISO_639-2_codes), while the second provides the relative path from your package's root to find the file. By convention, packages store these files in a `lang` folder found in their root, but if you're only ever going to provide a single set of translations (e.g. it's a paid module) you can just put it in the root of your module without any problems.

The name property is optional - if your language is among the [core supported languages](#core-supported-languages) then the name of the language is already set, and it's otherwise not really used elsewhere in the software.

The optional `system` and `module` fields allow you to tell Foundry to only conditionally register translations. This is useful for modules supporting multiple systems - you can provide two english translations, one for `dnd5e` the other for `pf2e`. If both are present, it will jointly validate that the listed system is being used and the listed module is active. You cannot do joint validation of multiple modules, but it would take extraordinary circumstances for this to be necessary; you can generally just ignore them.

Finally, the `flags` object is tacked on by Foundry in case the community has odd things they want to do with core module/system handling and can be safely ignored.

### Applications Only

Foundry does not support translating underlying document data, only preconfigured UI elements. This means that it does not support translating compendium content, such as the rules text of an item. The [Babele](https://foundryvtt.com/packages/babele) module is one way the community has found ways to support translating system and module content.

## API Interactions

The Localization class is instantiated at `game.i18n`; it has no static methods and does not ever need to be re-instantiated by a package developer.

### Language File Contents

A foundry language.json file can mix-and-match of `.`-separated fields and traditional JSON nesting. The general convention is that you use a reasonably unique namespace for translations specific to your package, often derived from your package ID; for example, `dnd5e` uses `DND5E` and `swade` uses `SWADE`.

Modules can override system-provided translations by providing a new translation with a matching key. A simple example for the `dnd5e` system, creating a nested namespace and then renaming the `"Magical"` property to `"Supernatural"`.

```json
{
  "MYMODULE": {
  	"Foo": "Bar"
  },
  "DND5E.Item.Property.Magical": "Supernatural"
}
```

### localize(stringId)

The most basic method is `game.i18n.localize`, which takes a localization string (using `.` separation for nested object properties) and if it can find a translation will return the translated text. If it cannot find a translation in the client's language it will then try to find an english-language translation, and if that fails it will simply return the original string.

### format(stringId, data = {})

A more complex version of localize, this finds the localization string but also will perform substitutions for parameters in brackets. For example, the core software has `"DOCUMENT.New": "New {type}"`. If you call `game.i18n.format("DOCUMENT.New", {type: "Actor"})`, it returns `"New Actor"`. You often will want to call `localize` on your data before passing it to `format`, as the inserted data is not automatically localized.


### {{localize stringId}}

In practice, you should rarely be directly calling `game.i18n.localize` and `game.i18n.format`; this render logic should be deferred to your handlebars template via the `localize` helper.

This handlebars helper performs two functions. If a single argument is passed, it simply calls `localize` on the string. If multiple arguments are passed, the rest are collated and passed as the `data` for `format`.

```handlebars
<!-- Returns "Actor" -->
{{localize "DOCUMENT.Actor"}}

<!-- Returns "Create New Actor" -->
{{localize "DOCUMENT.Create" type="Actor"}}

```

### has(stringId, fallback = true)

This simple helper method returns true or false if a translation key exists. You can pass `false` as the second argument to ignore english-language fallback.

### getListFormatter({style="long", type="conjunction"}={})

This returns a [List Formatter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/ListFormat/ListFormat) for the current client language which you can then apply to an array.

### sortObjects(objects, key)

This function sorts an array of objects by a given key, which is a string that uses `.` to denote nested fields. The objects you're sorting must already be localized; the reason to use this is it properly implements language-specific handling of special characters.
```js
const properties = [
  {
    label: {
      long: "Bravo",
      abbr: "B"
    },
    value: 1
  },
  {
    label: {
      long: "Alpha",
      abbr: "A"
    },
    value: 2
  },
  {
    label: {
      long: "Charlie",
      abbr: "C"
    },
    value: 3
  },
]

// mutates the provided array
game.i18n.sortObjects(properties, "label.long")
```

### Hooks.once("i18nInit", () => {})

This hook fires shortly after `init`, AFTER settings are registered and localizations are loaded. One common pattern is to define your package-specific `CONFIG` additions in `init` and then go back in an `i18ninit` hook and localize any labels stored in that object.

## Specific Use Cases

Here are some additional things to know about localization.

### Core Supported Languages

The core software provides definitions for the following languages:

- English

These translations can be found in `yourInstallPath\resources\app\public\lang\en.json`. They include many useful keys related to core functionality you may extending.

### Document Types

Translating package-provided types, e.g. `character` and `npc` as possible Actor types, is handled by a predefined structure. The pattern is `TYPES.DocumentType.type` These are used in core applications like the Create Dialog.

```json
{
	"TYPES": {
  	"Actor": {
    	"character": "Character",
      "npc": "Non-Player Character"
    }
    "Item": {
  		"weapon": "Weapon",
    	"spell": "Spell"
  	}
  }
}

```

### Word Order and Grammar

Let us assume the phrase “This is an apple” as an example. In English, you might want to translate this as follows:

```json
"modulename.phrase": "This is an",
"modulename.noun": "apple"
```

And then reuse the phrase “This is an” for other purposes. The problem is that in certain languages, this order is REVERSED. In languages such as Japanese, Turkish, Chinese, and other eastern/Asian languages it would become something akin to “An apple this is”.

The correct way to do the above would be something along these lines:
```json
"modulename.phrase": "This is an {noun}",
"modulename.noun": "apple"
```

This way, the translator can easily change the location of the noun before or after. For more complicated examples, there’s this wonderful part form monsterblock module about legendary action text which can serve as an example as to why having entries nested is important.

```json
"MOBLOKS5E.LegendaryText": "The {name} can take 3 legendary actions, choosing from the options below. Only one legendary action option can be used at a time and only at the end of another creature's turn. The {name} regains spent legendary actions at the start of its turn."
```

However, in the first example, you should take into consideration that some languages have gender-based nouns and noun classes. So “the word” should be split into “{the}” and “{word}” placeholders or just “{word}” and making the usage of “the” optional.


### Give space for two-byte letters and long worded languages
When creating containers for your UI, try to assume at least a 25~50% extra space for languages such as Japanese/Korean that use two-byte letters such as “日本” which are much bigger on the horizontal/vertical scale and easily break your carefully structured CSS settings. Or, make it so the container is scalable with the content.

Of course, large modules will require their own language overrides but keeping this in mind will allow your module to be localizable with minimal effort (since not everybody understands how to make their own CSS rules).

Try to merge string usage, unless it is on a different scope. Menu words like “save”, “close”, “Are you sure?” etc. always have the same meaning, therefore they should be reused whenever possible. However, use a new string if the scope is different.

For example, until recently the dnd5e system used the same “Save” string both for “saving menu settings” and for “abbreviation of saving throw”, which made stuff confusing. If unsure, you should ask somebody that does localizations and they can tell you if you split them properly or not.


### Try to avoid witty and clever messages
This is a hard one. English wits and jokes are a massive headache to localize. Try to make your UI/settings work for standard descriptions first. However, you should feel free to express your creativity with it, just let translators know that they are free to change these expressions to match the locale of where they are going to be used. Inexperienced people will try to go for literal translations and severely slow their process, while experienced people will automatically try to adapt the phrasing. However, we are afraid to infringe on the author’s rights, especially if we get called out after the fact, so please add some messages to express your willingness to have the translation adapted rather than being literally translated.

Add newly localizable strings on update notes if possible. If you would like to give more priority to localization and make localization feel welcome, it would help if you included new localizable strings in update notes. Or, express that new strings were added.


### VSCode Configuration

The [i18n-ally](https://github.com/lokalise/i18n-ally) extension for VSCode can allow you to display translations in-line with your code, better helping you catch bad translation strings. The following is a functional custom framework file you can put at `.vscode/i18n-ally-custom-framework.yml` to help the extension comprehend Foundry's localization strucutre.

```yaml
# .vscode/i18n-ally-custom-framework.yml

# An array of strings which contain Language Ids defined by VS Code
# You can check avaliable language ids here: https://code.visualstudio.com/docs/languages/overview#_language-id
languageIds:
  - javascript
  - typescript
  - handlebars

# An array of RegExes to find the key usage. **The key should be captured in the first match group**.
# You should unescape RegEx strings in order to fit in the YAML file
# To help with this, you can use https://www.freeformatter.com/json-escape.html
usageMatchRegex:
  # The following example shows how to detect `t("your.i18n.keys")`
  # the `{key}` will be placed by a proper keypath matching regex,
  # you can ignore it and use your own matching rules as well
  - "[^\\w\\d]game\\.i18n\\.localize\\(['\"`]({key})['\"`]\\)"
  - "[^\\w\\d]game\\.i18n\\.format\\(['\"`]({key})['\"`]"
  - "[^\\w\\d]ui\\.notifications\\.info\\(['\"`]({key})['\"`],\\s{\\s*.*\\slocalize: true\\s*.*}"
  - "[^\\w\\d]ui\\.notifications\\.warn\\(['\"`]({key})['\"`],\\s{\\s*.*\\slocalize: true\\s*.*}"
  - "[^\\w\\d]ui\\.notifications\\.error\\(['\"`]({key})['\"`],\\s{\\s*.*\\slocalize: true\\s*.*}"
  - "\\{\\{\\s*localize\\s+[\"']({key})['\"]\\s*\\}\\}"
  - "\\{\\{[\\w\\.\\s\\=]*\\(localize\\s+[\"']({key})['\"]\\)[\\w\\.\\s\\=]*\\}\\}"
  - "data-tooltip\\=[\"']({key})['\"]"

# If set to true, only enables this custom framework (will disable all built-in frameworks)
monopoly: true
```

You then need to finish configuration by adding the following to your `.vscode/settings.json`; here, `foundry` represents a `.gitignore`'d directory that can contain symlinks to core translations. (You can also use this directory to symlink the core Foundry files, but that requires adding them in a `jsconfig.json` file which is beyond the scope of this page). You can perform similar things with system translations if you're writing a module, e.g. a `dnd5e` folder that you symlink in *its* `en.json` file.

```json
  "i18n-ally.localesPaths": ["en.json", "foundry"],
  "i18n-ally.keystyle": "nested"
```

## Troubleshooting

### Language file isn't working

Edits to your manifest.json are not checked while Foundry is running; try shutting the software down and then restarting.