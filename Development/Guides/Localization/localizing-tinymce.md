---
title: Localizing the TinyMCE Editor
description: A guide on how to localize the TinyMCE text editor, which is normally out of scope of the Foundry VTT translation file.
published: false
date: 2021-01-13T16:12:32.810Z
tags: localization, translation, guide, tinymce
editor: markdown
dateCreated: 2021-01-13T00:54:19.266Z
---

# Localizing the TinyMCE Editor
Strings for the [TinyMCE](https://www.tiny.cloud/) text editor are not present in the usual Foundry Virtual Tabletop translation file as it is a third party library. TinyMCE, however, **does support** localization and the official documentation provides a guide on how to do it: https://www.tiny.cloud/docs/configure/localization/

This guide adapts the official TinyMCE with additional details on how to implement it in Foundry VTT. It was written for **Foundry VTT 0.7.9** and **TinyMCE 5.6.2**. This is an opinionated guide and instructs you to use certain formatting and file hierarchies. Finnish is used as an example language.

An example of a localized TinyMCE editor:

<table style="margin:0;">
  <tbody>
  <tr>
    <td style="border-bottom:none;"><img src="/development/guides/localization/tinymce/tinymce_en-us.png" alt="A TinyMCE editor localized to American English." style="width:100%;"></td>
    <td style="border-bottom:none;"><img src="/development/guides/localization/tinymce/tinymce_fi-fi.png" alt="A TinyMCE editor localized to Finnish." style="width:100%;"></td>
  </tr>
  </tbody>
</table>

## Localizing Custom Foundry VTT Menus
<img src="/development/guides/localization/tinymce/tinymce_custom_menu.png" alt="A custom TinyMCE menu in Foundry VTT 0.7.9." style="float:right;margin:1rem;">

Foundry VTT adds new menus into TinyMCE for which the official `.js` localization file does not provide localizations for. (An example pictured on the right.) To see what other formatting options the current version of Foundry VTT adds, copy the following code into the console (open with the <kbd>F12</kbd> key) and press enter: `CONFIG.TinyMCE.style_formats`

To avoid doubling the length of this guide, and to save you from replacing code you just copied, the guide is written with the assumption that you will want to localize these custom menus in addition to the TinyMCE core.

## Download a Language Package
1. Figure out the current version of TinyMCE used by Foundry VTT by looking at the **version** key in the file: `<path to Foundry VTT>/resources/app/node_modules/tinymce/package.json`

2. Download a language package for your language and for the appropriate TinyMCE version from the official website: https://www.tiny.cloud/get-tiny/language-packages/
> **NOTE:** Not all languages are 100% translated.
{.is-info}

3. Extract the downloaded archive. It contains the TinyMCE localization in a `.js` file.

## Load TinyMCE Localization {.tabset}
There are two methods for loading the TinyMCE localization:
1. A [more involved method](#the-two-json-files-method) of using two `.json` files and some extra JavaScript code.
1. A [quick and easy method](#the-quick-easy-method) of configuring TinyMCE to load the downloaded `.js` as-is.

Both methods will yield the same result for the end-user but the **first method is recommended** if you have a more complex translation workflow than simply editing text files or you are using an online translation service. Additionally it will make translating the custom Foundry VTT menus easier.

### Two JSON Files Method
> This is the recommended method for localizing TinyMCE.
{.is-success}

A more flexible way of localizing TinyMCE, involves modifying the downloaded `.js` localization file by moving the strings into a `.json` file, and adding some custom JavaScript code to load it. This allows you to use the same workflow as you usually do to translate core Foundry VTT and enables using various translation services to translate the `.json` file, should you wish to modify it.

Additionally this method will allow you to more cleanly translate custom menus added by Foundry VTT by separating them from the core TinyMCE strings.

1. Copy the **JSON data** from the TinyMCE `.js` localization file you downloaded into a new file at `<path to your module>/lang/tinymce/<language code>.json`. The `.js` file is formatted as follows:
```js
tinymce.addI18n('<TinyMCE lang code>',<multiline
JSON data>);
```

2. To translate **custom Foundry VTT menus**, simply add the strings shown in the GUI (**exactly** as written) into a JSON file at `<path to your module>/lang/tinymce/fvtt_<language code>.json`. For example, you would translate the menu pictured above by adding the keys **Custom** and **Secret** to a JSON object:
```json
{
  "Custom": "Mukautetut",
  "Secret": "Salainen"
}
```

3. Copy the following code into a new file at `<path to your module>/scripts/tinymce.js`.
> **NOTE:** Replace the 4 **\<placeholder\>** texts at the start with the correct values!
{.is-info}
```js
const lang_code = "<language code>";
const lang_name = "<language name>";
const localization_path = "/modules/<your module name>/lang/tinymce/";
const tinymce_lang_code = "<TinyMCE lang code>";

fetch(`${localization_path}fvtt_${lang_code}.json`)
    .then(response => {
        "use strict";
        if (response.ok) {
            return response.json();
        }

        console.error(`${lang_code} | Failed to load TinyMCE ${lang_name} localization (Foundry VTT additions): ` + response.status);
        return null;
    })
    .then(json => {
        "use strict";
        let localization = json;

        fetch(`${localization_path}${lang_code}.json`)
            .then(response => {
                if (response.ok) {
                    return response.json();
                }

                console.error(`${lang_code} | Failed to load TinyMCE ${lang_name} localization (core): ` + response.status);
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
                    console.log(`${lang_code} | Loaded TinyMCE ${lang_name} localization.`);
                }
            });
    });
```

4. Modify the original TinyMCE configuration to use the custom localization by copying the following code to a new file at `<path to your module>/scripts/<language code>.js`:
> **NOTE:** Replace the **\<placeholder\>** texts with the correct values!
{.is-info}
```js
Hooks.once("init", () => {
    CONFIG.TinyMCE = mergeObject(CONFIG.TinyMCE, {
        "use strict";

        language: "<TinyMCE language code>",
        language_url: "/modules/<your module name>/scripts/tinymce.js"
    });
});
```

5. Add the following code into your **module.json** at `<path to your module>/module.json`.
> **NOTE:** Replace the **\<placeholder\>** text with the correct value!
{.is-info}
```json
"esmodules": [
        "scripts/<language code>.js"
    ],
```

6. Enable your module in a world. Core Foundry VTT localization will load without enabling the module, but the TinyMCE localization requires enabling it.
7. After completing all steps successfully, when you open a TinyMCE editor for the first time (e.g., when writing a journal entry) you should see no error messages relating to TinyMCE and a message similar to this in the console (open with <kbd>F12</kbd>):
> ### Console
> fi-FI | Loaded TinyMCE Finnish localization.

### Quick & Easy Method
1. Copy the TinyMCE `.js` localization file you downloaded into `<path to your module>/lang/tinymce/<TinyMCE language code>.js`.
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
> **REMEMBER:** Add a comma (`,`) after the last item in the JSON array (line 4 in the example).
{.is-info}
3. Configure TinyMCE to use the localization by copying the following code to a new file at `<path to your module>/scripts/<language code>.js`.
> **NOTE:** Replace the **\<placeholder\>** texts with the correct values!
{.is-info}
```js
Hooks.once("init", () => {
    "use strict";

    CONFIG.TinyMCE = mergeObject(CONFIG.TinyMCE, {
        language: "<TinyMCE language code>",
        language_url: "/modules/<your module name>/scripts/<TinyMCE language code>.js"
    });
});
```

4. Add the following code into your **module.json** at `<path to your module>/module.json`.
> **NOTE:** Replace the **\<placeholder\>** text with the correct value!
{.is-info}
```json
"esmodules": [
        "scripts/<language code>.js"
    ],
```

5. Enable your module in a world. Core Foundry VTT localization will load without enabling the module, but the TinyMCE localization requires enabling it.
6. After completing all steps successfully, when you open a TinyMCE editor for the first time (e.g., when writing a journal entry) you should see no error messages relating to TinyMCE and a message similar to this in the console (open with <kbd>F12</kbd>):
> ### Console
> fi-FI | Loaded TinyMCE Finnish localization.