---
title: Localizing Map Note Icon Names
description: A guide on how to localize map note icons with a custom JSON file.
published: true
date: 2021-01-31T22:28:33.581Z
tags: localization, translation, guide, map note
editor: markdown
dateCreated: 2021-01-13T21:23:53.391Z
---

Strings for the drop-down list of note icons in map notes are not present in the usual Foundry Virtual Tabletop translation file. However, they **can be** localized by overriding the list of icons in the software configuration with JavaScript.

This guide was written for **Foundry VTT 0.7.9**. This is an opinionated guide and instructs you to use certain formatting, file hierarchies, and the included JavaScript code contains logging statements. Finnish is used as an example language.

You will be building (and the JavaScript code assumes) the following file structure in your module directory (replace `fi-FI` with your language code):
<pre style="line-height:100%;margin:1rem;">
your-module-directory
├── module.json        : Module information
│
├─┬─lang
│ └── fi-FI_icons.json : Core Foundry VTT translation
│
└─┬─scripts
  └── fi-FI.js         : Script to load at startup
</pre>
The following placeholder values are used in the code below. You need to replace them with the correct values for your module:
| Placeholder | Correct Value|
|-|-|
| `<language code>` | The language code for the translation. IETF language tag recommended. |
| `<language name>` | The name of the language in English for debug log messages. |
| `<your module name>` | The name of your module's directory. |

> [Venea.net](https://www.venea.net/web/culture_code) has an excellent resource for [IETF language tags](https://en.wikipedia.org/wiki/IETF_language_tag).
{.is-info}

<table style="margin:0;">
  <thead>
    <tr>
      <th style="border-bottom:none;padding-right:0;padding-bottom:0;padding-left:0;">Default</th>
      <th style="border-bottom:none;padding-right:0;padding-bottom:0;padding-left:0;">Localized</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td style="border-bottom:none;"><img src="/development/guides/localization/map-note-icons/map_note_icons_en-us.png" alt="A TinyMCE editor localized to American English." style="width:100%;"></td>
    <td style="border-bottom:none;"><img src="/development/guides/localization/map-note-icons/map_note_icons_fi-fi.png" alt="A TinyMCE editor localized to Finnish." style="width:100%;"></td>
  </tr>
  </tbody>
</table>

## Creating the Map Note Icon Translation JSON
Copy the following code into the console (open with the <kbd>F12</kbd> key) and press <kbd>Enter</kbd>:
```js
JSON.stringify(
    Object.keys(CONFIG.JournalEntry.noteIcons)
        .reduce((o, key) => ({ ...o, [key]: key }), {}),
    null, 2);
```
Copy the printed JSON string into a new `.json` file at `<path to your module>/lang/<language code>_icons.json` and translate the strings. The file should be formatted as such (only two keys shown as a sample):
```json
{
  "Anchor": "Ankkuri",
  "Barrel": "Tynnyri",
}
```

## Loading the Translation File
1. Copy the following JavaScript function into the JavaScript file that you are loading at startup. (E.g., `<path to your module>/scripts/<language code>.js`.) If you do have such a file, create one now.
> :pencil2: Replace the 3 **\<placeholder\>** texts at the start with the correct values!
{.is-info}
```js
const lang_code = "<language code>";
const lang_name = "<language name>";
const module_path = "/modules/<your module name>/";

const LocalizeNoteIcons = function(note_icons_translation_path) {
    "use strict";
    fetch(note_icons_translation_path)
        .then(response => {
            if (response.ok) {
                return response.json();
            }

            console.error(`${lang_code} | Failed to load Note Icons ${lang_name} localization: [${response.status}] ${response.statusText}`);
            return null;
        })
        .then(localized_names => {
            let localized_note_icons = {};

            for (let source_icon_name in CONFIG.JournalEntry.noteIcons) {
                if (CONFIG.JournalEntry.noteIcons.hasOwnProperty(source_icon_name)) {
                    // Default to unlocalized icon name to ensure newly added unlocalized icons are not skipped.
                    let icon_name = source_icon_name;

                    if (localized_names.hasOwnProperty(source_icon_name)) {
                        icon_name = localized_names[source_icon_name];
                    } else {
                        console.warn(`${lang_code} | Missing localization for icon name "${source_icon_name}".`);
                    }

                    localized_note_icons[icon_name] = CONFIG.JournalEntry.noteIcons[source_icon_name];
                }
            }

            // Sort icons according to the localized names.
            CONFIG.JournalEntry.noteIcons = Object.keys(localized_note_icons)
                .sort()
                .reduce((acc, key) => {
                    acc[key] = localized_note_icons[key];
                    return acc;
                }, {});

            console.log(`${lang_code} | Loaded ${lang_name} localization: Note Icons`);
        });
};

Hooks.once("ready", () => {
    "use strict";

    // Only localize if the user has selected this language.
    if (lang_code === game.settings.get("core", "language")) {
        LocalizeNoteIcons(`${module_path}/lang/${lang_code}_icons.json`);
    }
});
```

2. If you already have an `ready` hook in your script, only copy the `LocalizeNoteIcons` function and add a call to it in your `ready` hook:
```js
LocalizeNoteIcons("<path to note icons translation JSON>");
```

3. If you created a new `.js` file in step 1, add the following code into your **module.json** at `<path to your module>/module.json`.
> :pencil2: Replace the **\<placeholder\>** text with the correct value!
{.is-info}
```json
"esmodules": [
        "scripts/<language code>.js"
    ],
```

4. Enable your module in a world. Core Foundry VTT localization will load without enabling the module, but localizing the map note icons requires enabling it.
7. After completing all steps successfully, when you open a world you should see no error or warning messages relating to Note Icons and a message similar to this in the console (open with <kbd>F12</kbd>):
> ###### Console
> fi-FI | Loaded map note icons Finnish localization.
