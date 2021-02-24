---
title: Languages
description: Translations available for Foundry VTT.
published: true
date: 2021-02-24T21:34:35.254Z
tags: translations, languages, localizations
editor: markdown
dateCreated: 2021-02-24T21:22:56.597Z
---

Foundry Virtual Tabletop has a broad international community and the core software (as of version 0.7.9) has been translated to 12 languages. All translations are community created by real people and few contain machine translations.

For additional translations (including ones for game systems) see the [list of translation modules](https://foundryvtt.com/packages/tag/translation) on the Foundry VTT website (always up to date) or the [Translations](https://foundryvtt.wiki/en/community/Community-Translations) page here in the wiki (may not be up to date).

## Table of Supported Languages
| Code | Language | Manifest URL | Local Discord Community |
|:-|:-|:-|:-|
| ca-ES | Catalan / català | [Latest version](https://gitlab.com/montver/foundry-vtt-catala/-/raw/master/ca/module.json) | <i class="fab fa-discord"></i> [Foundry VTT Español](https://discord.gg/MHCerwd) (shared with Spanish) |
| zh-Hans | Chinese (simplified) / 中文（简体） | [Latest version](https://raw.githubusercontent.com/hktrpg/foundry_zh-tw/main/module.json) | ? |
| cs-CZ | Czech / čeština | [Latest version](https://gitlab.com/ptoseklukas/foundryvtt-lang-cs-cz/-/raw/master/cs-CZ/module.json) | <i class="fab fa-discord"></i> [The Foundry CZ komunita](https://discord.gg/7dHDqEW) (also Slovak) |
| fi-FI | Finnish / suomi | *work in progress* | <i class="fab fa-discord"></i> [Foundry VTT Suomi](https://discord.gg/U4y3cNebbg) |
| fr-FR | French / français | [Latest version](https://gitlab.com/baktov.sugar/foundryvtt-lang-fr-fr/-/raw/master/fr-FR/module.json) | <i class="fab fa-discord"></i> [La Fonderie](https://discord.gg/pPSDNJk) |
| de-DE | German / Deutsch | [Latest version](https://gitlab.com/henry4k/foundryvtt-lang-de/-/raw/master/module.json) | <i class="fab fa-discord"></i> [Die Gießerei (FoundryVTT)](https://discord.gg/XrKAZ5J) |
| it-IT | Italian / italiano | [Latest version](https://gitlab.com/riccisi/foundryvtt-lang-it-it/-/raw/master/it-IT/module.json) | <i class="fab fa-discord"></i> [Foundry_ITA](https://discord.gg/hsRcTby) |
| ja-JP | Japanese / 日本語 | [Latest version](https://raw.githubusercontent.com/BrotherSharper/foundryVTTja/master/module.json) | <i class="fab fa-discord"></i> [オンセ工房日本支部(Foundry VTT)](https://discord.gg/vM4YM27) |
| ko-KR | Korean / 한국어 | [Latest version](https://raw.githubusercontent.com/ShoyuVanilla/FoundryVTT-lang-ko-KR/master/module.json) | <i class="fab fa-discord"></i> [FVTT Korea(비공식)](https://discord.gg/DRbbn5w) |
| po-PO | Polish / polski | *work in progress* | ? |
| pt-BR | Portuguese (Brazil) / Português (Brasil) | [Latest version](https://gitlab.com/foundryvtt-pt-br/core/-/raw/master/pt-BR/module.json) | <i class="fab fa-discord"></i> [Foundry VTT Brasil](https://discord.gg/XNC86FBnQ2) |
| ru-RU | Russian / русский | [Latest version](https://raw.githubusercontent.com/Phenomen/foundry-vtt-ru/master/module.json) | <i class="fab fa-discord"></i> [Foundry VTT Russian Community](https://discord.gg/Z2CXFy35WF) |
| es-ES | Spanish / español | [Latest version](https://gitlab.com/carlosjrlu/foundryvtt-es/-/raw/master/es/module.json) | <i class="fab fa-discord"></i> [Foundry VTT Español](https://discord.gg/MHCerwd) (shared with Catalan) |
| sv-SE | Swedish / Svenska | [Latest version](https://raw.githubusercontent.com/xdy/foundryvtt-lang-sv/master/module.json) | *none* |

## Installing a Translation Module
1. The easiest way to install a module is to right click the "Latest version" link of the language you would like to install and copy the link. Note that the latest version is likely to only support the newest version of the game.
1. In Foundry VTT open the "Add-On Modules" tab.
![add-on_modules_tab.png](/fvtt-ui/add-on_modules_tab.png)
1. Click the "Install Module" button.
![install_module_button.png](/fvtt-ui/install_module_button.png)
1. At the bottom of the window click on the text field in the center (`https://path/to/module.json`) and use <kbd>Ctrl + V</kbd> (<kbd>Cmd + V</kbd> on a Mac) to paste the copied URL into the field.
![manifest_url.png](/fvtt-ui/manifest_url.png)
1. Press the "Install" button.
1. Go to the "Configuration" tab.
![configuration_tab.png](/fvtt-ui/configuration_tab.png)
1. Select your language from the "Default Settings" drop-down menu. The correct language will usually have "core" in the name.
![default_language_setting.png](/fvtt-ui/default_language_setting.png)
1. Restart the game to change the language.

## Contributing
If you would like to help with translating Foundry VTT, it's modules, or game systems, the best way is to join your local Discord community listed in the table above and ask for more information there. If your language is not listed in the table, your best bet is to ask for advice on the `#translations` channel on <i class="fab fa-discord"></i> [the official Foundry VTT Discord server](https://discordapp.com/invite/DDBZUDf) and look into the [localization guides](https://foundryvtt.wiki/en/development/guides/localization).

For more information about the community translation teams, their tools, and code repositories see the [Translations](https://foundryvtt.wiki/en/community/Community-Translations) page.

## Wiki Localizations
This community wiki is available in the following languages:
- [日本語 (Japanese)](https://foundryvtt.wiki/ja/home)
- [English (English)](https://foundryvtt.wiki/en/home)
- [Français (French)](https://foundryvtt.wiki/fr/home)
- [Suomi (Finnish)](https://foundryvtt.wiki/fi/home)