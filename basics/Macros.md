---
title: Macros
description: 
published: true
date: 2025-07-27T23:08:01.463Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:22:44.591Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)


<!--tl=4-->
<!--ts-->
   * [What is a Macro?](#what-is-a-macro)
   * [Macro Types](#macro-types)
      * [Chat Macros](#chat-macros)
      * [Script Macros](#script-macros)
         * [Example: Random Table Roll](#example-random-table-roll)
         * [Example: Rolling a skill check for DnD 5e](#example-rolling-a-skill-check-for-dnd-5e)
         * [Example: Rolling an attribute check for DnD 5e](#example-rolling-an-attribute-check-for-dnd-5e)
         * [Example: Play Audio](#example-play-audio)
<!--te-->

## What is a Macro?
A macro is a set of commands (such as a roll, a message, or a JavaScript scriptlet) that Foundry can execute when the associated macro button is pressed.

## Macro Types
### Chat Macros

Chat macros are chat commands that have been saved for later reuse. See section on [Chat commands](https://foundryvtt.wiki/en/basics/Chat/)

### Script Macros

Script Macros in Foundry use the underlying [JavaScript API](https://foundryvtt.com/api) to execute various commands. The API respects the permissions of the user executing the macro, so players cannot delete your big bad evil guy for example.

See also: [Learning API](https://foundryvtt.wiki/en/development/guides/API-Learning-API), and [API Snippets](https://foundry-vtt-community.github.io/development/API-Snippets/) (many of them can be used in a Macros).
Flix's [Beginners Macro Guide](https://github.com/GamerFlix/foundryvtt-api-guide/blob/main/macro_guide.md) is also a great starting point to learn the basics.
Some examples of using script macros are below.

#### Example: Random Table Roll 

```js
await game.tables.getName('Table Name').draw();
```

Replace the `Table Name` with the one you want to use.

#### Example: Rolling a skill check for DnD 5e

Here are a few simple scripts useful for rolling skill checks using the dnd5e system.

```js
// This would roll an athletics skill check for the players character. 
// Just replace 'ath' with the appropriate skill abbreviation for the
// skill you'd like to roll.
await character.rollSkill({ skill: "ath" });

// This would be used if a player controls more than one character.
// It would roll the check for which ever token the player has selected.
await actor.rollSkill({ skill: "ath" });
```

List of skill abbreviations:
```
"acr" => Acrobatics
"ani" => Animal Handling
"arc" => Arcana
"ath" => Athletics
"dec" => Deception
"his" => History
"ins" => Insight
"itm" => Intimidation
"inv" => Investigation
"med" => Medicine
"nat" => Nature
"prc" => Perception
"prf" => Performance
"per" => Persuasion
"rel" => Religion
"slt" => Sleight of Hand
"ste" => Stealth
"sur" => Survival
```

#### Example: Rolling an attribute check for DnD 5e

These are examples for rolling an attribute check.

```js
// This would pop up a dialog asking if you'd like
// to roll a Strength Test or a Strength Save.
// Just replace "str" with the appropriate ability
// abbreviation.
character.rollAbility({ ability: "str" });

// or this for a player that is controlling multiple
// tokens.
actor.rollAbility({ ability: "str" });

// This would skip the dialog and roll an ability test
await character.rollAbilityCheck({ ability: "str" });

// And this would skip the dialog and roll an ability save
await character.rollSavingThrow({ ability: "str" });
```

List of ability abbreviations:
```
"str" => Strength
"dex" => Dexterity
"con" => Constitution
"int" => Intelligence
"wis" => Wisdom
"cha" => Charisma
```
#### Example: Play Audio
This is a direct way to play audio.

<!--- {% raw %} --->
```js
AudioHelper.play({src: "audio/SFX/Fire arrow.mp3", volume: 0.8, autoplay: true, loop: false}, true);
```
<!--- {% endraw %} --->
