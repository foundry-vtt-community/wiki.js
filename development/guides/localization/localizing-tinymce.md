---
title: Localizing the TinyMCE Editor
description: A guide on how to localize the TinyMCE text editor, which is normally out of scope of the Foundry VTT translation file.
published: true
date: 2021-04-21T16:41:47.413Z
tags: localization, translation, guide, tinymce
editor: markdown
dateCreated: 2021-01-13T00:54:19.266Z
---

> Please note that the licensing of TinyMCE translation files is unclear as of January 2021. See [TinyMCE GitHub issue #6470](https://github.com/tinymce/tinymce/issues/6470). It is **not recommended** to include the downloaded TinyMCE translation files as a part of your translation module **like this guide instructs you to do** until the licensing situation is clarified. The guide can still be used to translate the custom menus added by Foundry VTT.
{.is-warning}

Strings for the [TinyMCE](https://www.tiny.cloud/) text editor are not present in the usual Foundry Virtual Tabletop translation file as it is a third party library. TinyMCE, however, **does support** localization and the official documentation provides a guide on how to do it: https://www.tiny.cloud/docs/configure/localization/

This guide adapts the official TinyMCE with additional details on how to implement it in Foundry VTT. It was written for **Foundry VTT 0.7.9** and **TinyMCE 5.6.2**. This is an opinionated guide and instructs you to use certain formatting, file hierarchies, and the included JavaScript code contains logging statements. Finnish is used as an example language.

An example of a localized TinyMCE editor:

<table style="margin:0;">
  <thead>
    <tr>
      <th style="border-bottom:none;padding-right:0;padding-bottom:0;padding-left:0;">Default</th>
      <th style="border-bottom:none;padding-right:0;padding-bottom:0;padding-left:0;">Localized</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td style="border-bottom:none;"><img src="/development/guides/localization/tinymce/tinymce_en-us.png" alt="A TinyMCE editor localized to American English." style="width:100%;"></td>
    <td style="border-bottom:none;"><img src="/development/guides/localization/tinymce/tinymce_fi-fi.png" alt="A TinyMCE editor localized to Finnish." style="width:100%;"></td>
  </tr>
  </tbody>
</table>

## Localizing Custom Foundry VTT Menus
<img src="/development/guides/localization/tinymce/tinymce_custom_menu.png" alt="A custom TinyMCE menu in Foundry VTT 0.7.9." style="float:right;margin:1rem;">

Foundry VTT adds new menus into TinyMCE for which the official `.js` translation file does not provide localizations for. (An example pictured on the right.) To see what other formatting options the current version of Foundry VTT adds, copy the following code into the console (open with the <kbd>F12</kbd> key) and press enter: `CONFIG.TinyMCE.style_formats`

To avoid doubling the length of this guide, and to save you from replacing code you just copied, the guide is written with the assumption that you will want to localize these custom menus in addition to the TinyMCE core.

## Download a Language Package
1. Figure out the current version of TinyMCE used by Foundry VTT by looking at the **version** key in the file:
`<path to Foundry VTT>/resources/app/node_modules/tinymce/package.json`

2. Download a language package for your language and for the appropriate TinyMCE version from the official website: https://www.tiny.cloud/get-tiny/language-packages/
> Not all languages are 100% translated.
{.is-info}

3. Extract the downloaded archive. It contains the TinyMCE localization in a `.js` file.

## Load TinyMCE Localization
There are two methods for loading the TinyMCE localization:
1. A [more involved method](#two-json-files-method) of using two `.json` files and some extra JavaScript code.
1. A [quick and easy method](#quick-easy-method) of configuring TinyMCE to load the downloaded `.js` as-is.

Both methods will yield the same result for the end-user but the **first method is recommended** if you have a more complex translation workflow than simply editing text files or you are using an online translation service. Additionally it will make translating the custom Foundry VTT menus easier.

The following placeholder values are used in the code below. You need to replace them with the correct values for your module:
| Placeholder | Correct Value|
|-|-|
| `<language tag>` | The language tag for the translation. IETF language tag recommended. |
| `<language name>` | The name of the language in English for debug log messages. |
| `<your module name>` | The name of your module's directory. |
| `<TinyMCE lang code>` | The TinyMCE language code listed in the `Code` column of the [translation downloads page](https://www.tiny.cloud/get-tiny/language-packages/). |

> [Venea.net](https://www.venea.net/web/culture_code) has an excellent resource for [IETF language tags](https://en.wikipedia.org/wiki/IETF_language_tag).
{.is-info}

### Methods {.tabset}
#### Two JSON Files Method
> This is the recommended method for localizing TinyMCE.
{.is-success}

A more flexible way of localizing TinyMCE, involves modifying the downloaded `.js` translation file by moving the strings into a `.json` file, and adding some custom JavaScript code to load it. This allows you to use the same workflow as you usually do to translate core Foundry VTT and enables using various translation services to translate the `.json` file, should you wish to modify it.

Additionally this method will allow you to more cleanly translate custom menus added by Foundry VTT by separating them from the core TinyMCE strings.

You will be building (and the JavaScript code assumes) the following file structure in your module directory (replace `fi-FI` with your language tag):
<pre style="line-height:100%;margin:1rem;">
Your Module Directory
├── module.json         : Module information
│
├─┬─lang
│ ├── fi-FI.json        : Core Foundry VTT translation
│ │ 
│ └─┬─tinymce
│   ├── fi-FI.json      : Core TinyMCE translation
│   └── fi-FI_fvtt.json : Custom TinyMCE menu translation
│
└─┬─scripts
  ├── fi-FI.js          : Script to configure TinyMCE
  └── tinymce.js        : Script to load TinyMCE localization
</pre>

##### Steps
1. Copy the **JSON data** from the TinyMCE `.js` translation file you downloaded into a new file at `<path to your module>/lang/tinymce/<language tag>.json`. The `.js` file is formatted as follows:
```js
tinymce.addI18n('<TinyMCE lang code>',<multiline
JSON data>);
```

2. To translate **custom Foundry VTT menus**, simply add the strings shown in the GUI (**exactly** as written) into a JSON file at `<path to your module>/lang/tinymce/<language tag>_fvtt.json`. For example, you would translate the menu pictured above by adding the keys **Custom** and **Secret** to a JSON object:
```json
{
  "Custom": "Mukautetut",
  "Secret": "Salainen"
}
```

3. Copy the following code into a new file at `<path to your module>/scripts/tinymce.js`.
> :pencil2: Replace the 4 **\<placeholder\>** texts at the start with the correct values!
{.is-info}
```js
const lang_tag = "<language tag>";
const lang_name = "<language name>";
const tinymce_translations_directory = "/modules/<your module name>/lang/tinymce/";
const tinymce_lang_code = "<TinyMCE lang code>";

fetch(`${tinymce_translations_directory}${lang_tag}_fvtt.json`)
    .then(response => {
        "use strict";
        if (response.ok) {
            return response.json();
        }

        console.error(`${lang_tag} | Failed to load TinyMCE ${lang_name} localization (Foundry VTT additions): [${response.status}] ${response.statusText}`);
        return null;
    })
    .then(json => {
        "use strict";
        let localization = json;

        fetch(`${tinymce_translations_directory}${lang_tag}.json`)
            .then(response => {
                if (response.ok) {
                    return response.json();
                }

                console.error(`${lang_tag} | Failed to load TinyMCE ${lang_name} localization (core): [${response.status}] ${response.statusText}`);
                return null;
            })
            .then(json => {
                if (localization && json) {
                    localization = mergeObject(json, localization);
                } else if (json) {
                    localization = json;
                }

                if (localization) {
                    tinyMCE.addI18n(tinymce_lang_code, localization);
                    console.log(`${lang_tag} | Loaded ${lang_name} localization: TinyMCE`);
                }
            });
    });
```

4. Modify the original TinyMCE configuration to use the custom localization by copying the following code to a new file at `<path to your module>/scripts/<language tag>.js`:
> :pencil2: Replace the **\<placeholder\>** texts with the correct values!
{.is-info}
```js
const module_path = "/modules/<your module name>/";
const tinymce_lang_code = "<TinyMCE lang code>";

const ConfigureTinyMCE = function(tinymce_translation_path) {
    "use strict";
    CONFIG.TinyMCE = mergeObject(CONFIG.TinyMCE, {
        language: tinymce_lang_code,
        language_url: tinymce_translation_path
    });
};

Hooks.once("ready", () => {
    "use strict";
    // Only localize if the user has selected this language.
    if (lang_tag === game.settings.get("core", "language")) {
        ConfigureTinyMCE(`${module_path}scripts/tinymce.js`);
    }
});
```

5. Add the following code into your **module.json** at `<path to your module>/module.json`.
> :pencil2: Replace the **\<placeholder\>** text with the correct value!
{.is-info}
```json
"esmodules": [
        "scripts/<language tag>.js"
    ],
```

6. Enable your module in a world. Core Foundry VTT localization will load without enabling the module, but the TinyMCE localization requires enabling it.
7. After completing all steps successfully, when you open a TinyMCE editor for the first time (e.g., when writing a journal entry) you should see no error messages relating to TinyMCE and a message similar to this in the console (open with <kbd>F12</kbd>):
> ###### Console
> fi-FI | Loaded TinyMCE Finnish localization.

#### Quick & Easy Method

You will be building (and the JavaScript code assumes) the following file structure in your module directory (replace `fi-FI` with your language tag):
<pre style="line-height:100%;margin:1rem;">
Your Module
├── module.json  : Module information
│
├─┬─lang
│ ├── fi-FI.json : Core Foundry VTT translation
│ │ 
│ └─┬─tinymce
│   └── fi.js    : Core TinyMCE translation + custom menus
│
└─┬─scripts
  └── fi-FI.js   : Script to configure TinyMCE
</pre>

##### Steps
1. Copy the TinyMCE `.js` translation file you downloaded into `<path to your module>/lang/tinymce/<TinyMCE language code>.js`.
2. To translate **custom Foundry VTT menus**, simply add the strings shown in the GUI (**exactly** as written) into the `.js` file. The file is formatted as follows:
```js
tinymce.addI18n('<TinyMCE lang code>',<multiline
JSON data>);
```

For example, you would translate the menu pictured above by adding the keys **Custom** and **Secret** to the end of the JSON array in the file like this (lines 5 & 6):
```js
tinymce.addI18n('fi',{
...omitted data...
"Caption": "Seloste",
"Insert template": "Lis\u00e4\u00e4 pohja",
"Custom": "Mukautettu",
"Secret": "Salainen"
});
```
> :grey_exclamation: Add a comma (`,`) after the last item in the JSON array (line 4 in the example).
{.is-info}
3. Configure TinyMCE to use the localization by copying the following code to a new file at `<path to your module>/scripts/<language tag>.js`.
> :pencil2: Replace the **\<placeholder\>** texts with the correct values!
{.is-info}
```js
Hooks.once("ready", () => {
    "use strict";
    // Only localize if the user has selected this language.
    if ("<language tag>" === game.settings.get("core", "language")) {
        CONFIG.TinyMCE = mergeObject(CONFIG.TinyMCE, {
            language: "<TinyMCE language code>",
            language_url: "/modules/<your module name>/scripts/<TinyMCE language code>.js"
        });
    }
});
```

4. Add the following code into your **module.json** at `<path to your module>/module.json`.
> :pencil2: Replace the **\<placeholder\>** text with the correct value!
{.is-info}
```json
"esmodules": [
        "scripts/<language tag>.js"
    ],
```

5. Enable your module in a world. Core Foundry VTT localization will load without enabling the module, but the TinyMCE localization requires enabling it.
6. After completing all steps successfully, when you open a TinyMCE editor for the first time (e.g., when writing a journal entry) you should see no error messages relating to TinyMCE and a message similar to this in the console (open with <kbd>F12</kbd>):
> ###### Console
> fi-FI | Loaded Finnish localization: TinyMCE
