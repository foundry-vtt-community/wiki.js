---
title: Chat
description: 
published: true
date: 2025-07-26T18:59:41.225Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:21:56.970Z
---

![Up to date as of v13](https://img.shields.io/badge/FoundryVTT-v13-informational)

*Official Documentation*

- [Knowledge Base: Chat Messages](https://foundryvtt.com/article/chat/)
- [API Reference: ChatMessage](https://foundryvtt.com/api/classes/foundry.documents.ChatMessage.html)
- [API Reference: ChatMessages](https://foundryvtt.com/api/classes/foundry.documents.collections.ChatMessages.html)
- [API Reference: ChatLog](https://foundryvtt.com/api/classes/foundry.applications.sidebar.tabs.ChatLog.html)

## Chat Commands

While entering a message, users are able to prefix their message with a command. Some commands related to sending chat messages are listed below:

### In Character

Syntax: `/ic {message}`

Causes the message to be spoken by an associated character. With a character's token selected (or with a speaker identified through the Player Configuration window), players will automatically speak in character, removing the need to enter this command for every message.

### Out of Character

Syntax: `/ooc {message}`

Causes the message to be spoken out of character (OOC). OOC messages will be outlined by the player's color to make them more easily recognizable. A player without an identified speaker or a selected token will automatically speak out of character.

### Emotes

Syntax: `/emote {message}` or `/em {message}` or with version 0.6.5 and later, `/me {message}`

Causes the message to be an emote performed by the selected character. Emotes are in-character actions conveyed through text by the player, and therefore require the player to select a token (or link a character through the Player Configuration window). Entering `/emote waves his hand.` while controlling a character named Simon will send the message, "Simon waves his hand."

### Whispered Messages

Syntax: `/whisper {target} {message}` or `/w {target} {message}` or `@{target} {message}`

Whispers a message to the target. Non-GM players require the "Whisper Private Messages" permission to be able to send whispers; if they do, these messages will not be seen by GM/Assistant players.

To send a message to a player with a space in their name, wrap the name in square brackets: `/w [Arthur Dent] Where is your towel?`

Note that you can message multiple users at once by enclosing their names as a comma separated list within brackets. For example `/w [Andrew, Tim, Julia] What do you think?` or `@[James, Alicia] Should we attack, or sneak past?` Lastly, using `gm` or `players` will send a whispered message to all GM users or all non-GM users respectively.

## What is a macro?

A macro is a way to save bit of text that you bloop out into the chat often. It could be a die roll command, a whisper, or formatted text.

You can see more about the macro interface [here](https://foundryvtt.com/article/macros/).

## Rolling Dice

Basically, dice syntax works like this:

    /r {number}d{faces} + {modifiers}

Example:

    /r 2d10 + 5

Which sends this to chat:

![](https://paper-attachments.dropbox.com/s_18A9487B0EE61A81F393FA91A13C89DE192362383B6581DB8EF3234F6BDD2D71_1587942384206_image.png)

You can find more complicated dice syntax (such as roll & keep) [here](https://foundryvtt.com/article/dice/).

#### More dice tips:

* Some functions of JS builtin object [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) are supported in rolls. Example `[[ 1d6 + round(7/2) ]]`

* Explode with "operator" does not really need a number after operator and maximum die value is used in that case. Example: `/roll 1d4x=` will explode on 4.

### Roll Commands

The `/r` or `/roll` command does a normal, public roll. You can use the following alternative commands as well:

`/gmr` or `/gmroll` displays the roll to you and the GM only.

`/br` or `/broll` or `/blindroll` displays the roll to the GM only (and hides it from you.)

`/sr` or `/selfroll` displays the roll only to you.


### Using Attributes

In most Foundry systems, a number of attributes about an actor are made available to chat messages and rolls , and commonly referred to as `rollData`. These allow you to use your attributes as variables in all your formulas, so that when you change one of your attributes all of your formulas, macros, etc. will update along with it automatically. Systems have special attributes for different actor attributes; you will have to reference the system for those.

You can reference these attributes in your roll commands by typing @attribute, replacing the word "attribute" with the attribute key.

For an attribute with the key "prof", it looks like this:

    /r 2d10 + @prof

When you pop that out into chat, it will look like this, assuming the prof attribute contains the number "2":

![](https://i.imgur.com/Vw8jedz.png)

It will automatically draw from your assigned character's attributes unless you have a different token selected.

#### Seeing a list of attributes

If you wish to see the list of @attributes you can utilize, open console (F12) and type `_token.actor.getRollData()` (token should be selected). You will see a list of options you can reference in a dot notation. For example, if you wish to reference the stealth modifier for a D&D5e actor, it would be: `@skills.ste.mod`.

## Formatting

### Inline Rolls

You can roll dice inside *any* text by placing brackets around it, like so:

```
The quick brown fox jumps [[2d10+5]] over the lazy dog.
```

When you send that to chat, the dice will be rolled and the result will be displayed inside the text, like so:

![](https://i.imgur.com/jlOgjv6.png)


You can also put "delayed" rolls or “clickable” rolls inline. This will show the dice formula inline instead of automatically rolling the dice, and when clicked the dice will roll. You can accomplish this by including the roll command inside the brackets. Like so:

```
The quick brown fox jumps [[/r 2d10+5]] over the lazy dog.
```

When you send it to chat, it will look like this:

![](https://i.imgur.com/dIY1rFf.png)


And when you click on the roll formula, it will roll the dice, like so:

![](https://i.imgur.com/7rDxMMD.png)


You can embed inline rolls in any text box on Foundry (including on your character sheet, journal entries, macros, and the chat box), and you can make it look nice using HTML formatting as long as you condense the code before pasting it in.  See HTML Formatting for more information.

### Comments

You can add comments to die rolls by putting text after a # at the end, like so:

    /r 2d10+5 #This is my attack roll

Which will result in this:

![](https://i.imgur.com/KOfyoN9.png)


You can also including condensed html formatting in the text, like so:

    /r 2d10+5 #<h2> This is my attack roll </h2>

Which looks like this:

![](https://i.imgur.com/wHaW2Hs.png)


See [HTML Formatting](https://foundryvtt.wiki/en/basics/Chat#html) for more information on HTML.

### References

You can reference almost any entity in Foundry in *any* textbox in Foundry, like so:

| Entity          | Code                                |
| --------------- | ----------------------------------- |
| Character/Actor | `@Actor[Character Name]` (Note there must be an actor that exists with that name. Not just a Compendium entry.)           |
| Scene           | `@Scene[Scene Name]`                |
| Item            | `@Item[Item Name]` (As with actor references, this refers to an item (see item sidebar). A Compendium entry isn’t enough.)                                    |
| Journal Entry   | `@JournalEntry[Journal Entry Name]` |
| Compendium      | `@Compendium[Entry Name]`           |
| Roll Table      | `@RollTable[Roll Table Name]`       |
| Macro           | `@Macro[Macro Name]`                |

Note that linking to a macro or rollable table does not automatically roll it — it will create a button you must click to roll. 

Entry/object links are case-sensitive! Both the type (“JournalEntry”, not “journalEntry”) and the object’s name (“Abacus”, not “abacus”). You can also drag-and-drop objects to some places to create links.

In the case of Actor, Item, and other references of the @<type>[<name>] style, a reference is created to a specific object’s ID. If that object is later deleted, even if a new one with the same name is made, the existing link won’t refer to the new object. Also note that if multiple choices for a given name exist, the “first” one will be picked. If that “first” one happens to be one that a player can’t see due to permissions, then even if there’s a “second” one the player can see, clicking the reference link will give them a permissions error.

Example:

    This is some random text, and here is my skill check macro button @Macro[Initiative] also here is a journal entry I want to link to @JournalEntry[Kryx SRD]

Displays this:

![](https://paper-attachments.dropbox.com/s_18A9487B0EE61A81F393FA91A13C89DE192362383B6581DB8EF3234F6BDD2D71_1587943331481_image.png)

## HTML

You can format your journal entries, macros, etc — basically anything with a text box — using HTML. 

**Learning HTML?**
HTML is *extremely* simple. If you can put `<h1> tags </h1>` around some text, you can use HTML! If you want to learn some basic HTML, you can do the first few lessons before “create a text field” [here](https://www.freecodecamp.org/learn/), which all together shouldn’t take you more than 20 minutes.

**Using the built-in HTML Editor**
You don’t have to learn any HTML at all in order to make your formatting nice and pretty. On any description box (on your character, Journal entries, items, etc) there should be a text editor you can use. Just combine inline dice and references with using the buttons for formatting there to make things nice and pretty.

**Using an external HTML Editor**
For macros, you will need to use an external HTML editor. You can use [this one](http://htmleditor.in/index.html).  Make sure to pass it through [this compressor](https://www.textfixer.com/html/compress-html-compression.php) before you paste it in, however. If you want to use an external HTML editor to create the code for text boxes that *do* have a built-in editor (in cases where the built-in editor doesn't have enough features,) you'll need to click the <> icon in the built-in text editor to directly edit (and paste in) the code.

## Embedding Webpages
You can embed most webpages in Foundry anywhere there is a text editor.  When you have the text editor open, click the source button (<>) and paste in the following code, replacing YOUR_LINK with the link to your desired website (including https:// in your link):

```html
<div style="overflow: hidden; position: relative; min-height: 100%;"><iframe style="border: 0; height: 100%; left: 0; position: absolute; top: 0; width: 100%;" src="YOUR_LINK" width="100%" height="100%" allowfullscreen="true;">
</iframe></div>
```
This can be a useful way to embed rule references, SRDs, videos, and even character sheets. Some websites will not embed because of the website's private settings, and some websites (such as YouTube) require that you use a special embed link.