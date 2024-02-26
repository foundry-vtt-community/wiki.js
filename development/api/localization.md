---
title: Localization
description: A helper class which assists with localization (i18n) and string translation
published: true
date: 2024-02-26T18:37:34.745Z
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

The Localization class is available at `yourFoundryInstall\resources\app\client\apps\i18n.js`

## Key Concepts

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

### {{localize}}

This handlebars helper performs two functions. If a single argument is passed, it simply calls `localize` on the string. If multiple arguments are passed, the rest are collated and passed as the `data` for `format`.

```handlebars
<!-- Returns "Actor" -->
{{localize "DOCUMENT.Actor"}}

<!-- Returns "Create New Actor" -->
{{localize "DOCUMENT.Create" type="Actor"}}

```

## Specific Use Cases

Here are some additional things to know about localization.

### Core Supported Languages

The core software provides definitions for the following languages:

- English

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