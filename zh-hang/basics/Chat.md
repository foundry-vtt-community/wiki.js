---
title: Chat
description: 
published: true
date: 2021-04-18T00:25:32.278Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:21:56.970Z
---

# Chat, Macro, & Formatting Guide

# 聊天命令

輸入信息時，用戶可以在命令前添加信息前綴。 下面列出了一些與發送聊天信息有關的命令：

## In Character

句型: `/ic {message}`

使信息由關聯角色說出。 如果已選擇角色的Token（或通過“玩家配置”視窗選擇說話者）後，玩家將自動以角色說話，而無需為每條信息輸入此命令。

## Out of Character

句型: `/ooc {message}`

使信息以超遊（OOC）角度發出。 OOC信息將以玩家的顏色進行輸出，以使其更易於識別。沒有指定說話者或選定TOKEN的玩家將自動(OOC)。

## Emotes

句型: `/emote {message}` or `/em {message}` , `/me {message}`

使信息成為所選角色執行的表情動作。 表情是玩家通過文本傳達的角色內動作，因此要求玩家已選擇Token（或通過“玩家配置”鏈接角色）。 輸入`/ emote 揮動他的手`。同時一個叫Simon的角色將發送信息：`Simon揮動他的手。`

## Whispered Messages

句型: `/whisper {target} {message}` or `/w {target} {message}` or `@{target} {message}`

對目標低語。非GM玩家需要獲得“Whisper Private Messages私訊”權限才能發送私訊；如果這樣，GM / Assistant將看不到這些信息。

要將信息發送給姓名中帶有空格的玩家，請將該姓名括在方括號中：`/ w [Arthur Dent]毛巾在哪裡？`

請注意，您可以通過在方括號中用逗號分隔的形式，同時向多個用戶發送信息。 例如`/w [Andrew, Tim, Julia] 你覺得怎樣？` 或 `@[James,Alicia]我們應該進攻還是偷偷溜走？` 
最後，用`GM`或`players`可以分別私訊所有GM用戶或所有非GM用戶。

# What is a macro?

Macro是一種讓玩家快捷地去進行某些動作的方式。它可以是MOD命令，私訊或格式化的文本。

您可以在[此處](https://foundryvtt.com/article/macros/) 上了解有關Macros的更多信息。

# Rolling Dice

基本上，骰子的工作方式如下：

    /r {數量}d{面數} + {增減}}

例子:

    /r 2d10 + 5

哪個發送到聊天：

![](https://paper-attachments.dropbox.com/s_18A9487B0EE61A81F393FA91A13C89DE192362383B6581DB8EF3234F6BDD2D71_1587942384206_image.png)


您可以在 [這裡](https://foundryvtt.com/article/dice/) 中找到更複雜的骰子語法（例如Roll和保留）。

### More dice tips:

* 支援某些JS 方式運算擲骰 [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) . 例如 `[[ 1d6 + round(7/2) ]]`

## Roll Commands

`/r` or `/roll`該命令會進行正常的公開擲骰。 您也可以使用以下替代命令：

`/gmr` or `/gmroll` 僅向您和GM顯示該擲骰。

`/br` or `/broll` or `/blindroll` 僅向GM顯示該擲骰（並向您隱藏該擲骰）。

`/sr` or `/selfroll` 僅向您顯示該擲骰。


## Using Attributes

If you are using the Simple Worldbuilding System, your character sheet stores a number of basic attributes about your character in the Attributes tab. These allow you to use your attributes as variables in all your formulas, so when you change one of your attribute all your formulas, macros, etc update along with it automatically. Most systems have special attributes for different parts of the character sheet -- you will have to reference the system for those.

You can reference these attributes in your roll commands by typing @attribute, replacing the word "attribute" with the attribute key. 

For an attribute with the key "init", it looks like this:

    /r 2d10 + @prof

When you pop that out into chat, it will look like this, assuming the init attribute contains the number "2":

![](https://i.imgur.com/Vw8jedz.png)


It will automatically draw from your assigned character's attributes unless you have a different token selected.

### Seeing a list of attributes

If you wish to see the list of @attributes you can utilize, open console (F12) and type `_token.actor.data.data` (token should be selected). You will see a list of options you can reference in a dot notation. For example, if you wish to reference the stealth modifier, it would be: `@skills.ste.mod`.

The top level attributes you can access with the @ symbol are: `@abilities, @attributes, @bonuses, @currency, @details, @resources, @skills, @spells, @traits`.


# Formatting

## Inline Rolls

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

## Comments

You can add comments to die rolls by putting text after a # at the end, like so:

    /r 2d10+5 #This is my attack roll

Which will result in this:

![](https://i.imgur.com/KOfyoN9.png)


You can also including condensed html formatting in the text, like so:

    /r 2d10+5 #<h2> This is my attack roll </h2>

Which looks like this:

![](https://i.imgur.com/wHaW2Hs.png)


See HTML Formatting for more information on HTML.

## References

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

# Embedding Webpages
You can embed most webpages in Foundry anywhere there is a text editor.  When you have the text editor open, click the source button (<>) and paste in the following code, replacing YOUR_LINK with the link to your desired website (including https:// in your link):

```html
<div style="overflow: hidden; position: relative; min-height: 100%;"><iframe style="border: 0; height: 100%; left: 0; position: absolute; top: 0; width: 100%;" src="YOUR_LINK" width="100%" height="100%" allowfullscreen="true;">
</iframe></div>
```
This can be a useful way to embed rule references, SRDs, videos, and even character sheets. Some websites will not embed because of the website's private settings, and some websites (such as YouTube) require that you use a special embed link.

# Approximating a Character Sheet
If you’re using the Simple Worldbuilding System and are industrious, you can put together a perfectly serviceable (if ugly) character sheet with clickable roll buttons in the Description box of your character, using nothing but dice syntax, references, and formatting. Greta has the beginnings of one here:

![](https://paper-attachments.dropbox.com/s_18A9487B0EE61A81F393FA91A13C89DE192362383B6581DB8EF3234F6BDD2D71_1587945338468_image.png)