---
title: Localization Best Practices
description: Describes some best practices for package developers who wish to enable their package to be translated.
published: true
date: 2021-01-12T20:50:18.961Z
tags: localization, translation, guide
editor: markdown
dateCreated: 2020-11-19T16:43:45.270Z
---

> ### Foreward by @Brother Sharp
> Hello there!
> I made this document to help module developers create proper localization support for their modules/systems. These are mostly from my own personal experience and may conflict with existing technology best practices so take them with a grain of salt. I might use images from existing modules, this does not mean that something is good or bad, just so happens that it was convenient for the given example. Note that I cannot help you with how to set up the l18n environment since I am not proficient in it.
{.is-info}


It is important to understand that localization is a primary method for non-English speaking users to get ahold of your module and enjoy it. Localization is not something that can be done easily, especially depending on the kind of module or system, but following these points should give you an insight into how to make the lives of translators easier (while also avoiding refactors in the future).


## Which parts do I first localize?
Most developers start with localization at about the middle or end of their module/system, this is understandable given the amount of work put into it. At a popular request, many developers take the leap to make their work localizable. During this phase, it is not expected to get a full localization from the get-go.

Start with making the UI that is shown to the players localizable. Then move on to player settings, then finish it up with GM only settings/error messages. Most GMs are competent enough to understand what functionality their modules/systems have, but players won’t be very well at it given their limited permissions. Starting with player visible layers first helps us a lot.

## Split strings by usage/menus
If a module, try to split strings into blocks by menu they appear on. If a system, try to split them by the tab or nearest terminology.

Ex1: for systems, try to group up stuff that are related to each other. The following is the dnd5e skill lineup in the 5e localization file.

```
"DND5E.SkillAcr": "...",
"DND5E.SkillAni": "...",
"DND5E.SkillArc": "...",
"DND5E.SkillAth": "...",
"DND5E.SkillDec": "...",
"DND5E.SkillHis": "...",
```

Ex2: For modules, especially when making settings, try to group up the names and hints so the translator knows what they are about to translate.

```  
"minor-qol.ItemDeleteCheck.Name": "...",
"minor-qol.ItemDeleteCheck.Hint": "...",
"minor-qol.DamageImmunities.Name": "...",
"minor-qol.DamageImmunities.Hint": "...",
"minor-qol.SpeedItemRolls.Name": "...",
"minor-qol.SpeedItemRolls.Hint": "...",
```
Try to manually add empty line breaks to denote the end of one portion (menus etc.) and the start of another (settings etc.). This way we don’t have to spend much time opening it up inside of the application to check what we just translated.

Would be best if we could have comments, but JSON doesn’t work that way.

## Be careful of word order and grammar
Let us assume the phrase “This is an apple” as an example. In English, you might want to translate this as follows:

```
"modulename.phrase": "This is an",
"modulename.noun": "apple"
```

And then reuse the phrase “This is an” for other purposes. The problem is that in certain languages, this order is REVERSED. In languages such as Japanese, Turkish, Chinese, and other eastern/Asian languages it would become something akin to “An apple this is”.

The correct way to do the above would be something along these lines:
```
"modulename.phrase": "This is an {noun}",
"modulename.noun": "apple"
```

This way, the translator can easily change the location of the noun before or after. For more complicated examples, there’s this wonderful part form monsterblock module about legendary action text which can serve as an example as to why having entries nested is important.

```
"MOBLOKS5E.LegendaryText": "The {name} can take 3 legendary actions, choosing from the options below. Only one legendary action option can be used at a time and only at the end of another creature's turn. The {name} regains spent legendary actions at the start of its turn."
```

However, in the first example, you should take into consideration that some languages have gender-based nouns and noun classes. So “the word” should be split into “{the}” and “{word}” placeholders or just “{word}” and making the usage of “the” optional.


## Give space for two-byte letters and long worded languages
When creating containers for your UI, try to assume at least a 25~50% extra space for languages such as Japanese/Korean that use two-byte letters such as “日本” which are much bigger on the horizontal/vertical scale and easily break your carefully structured CSS settings. Or, make it so the container is scalable with the content.

Of course, large modules will require their own language overrides but keeping this in mind will allow your module to be localizable with minimal effort (since not everybody understands how to make their own CSS rules).

Try to merge string usage, unless it is on a different scope
Menu words like “save”, “close”, “Are you sure?” etc. always have the same meaning, therefore they should be reused whenever possible. However, use a new string if the scope is different.

For example, until recently the dnd5e system used the same “Save” string both for “saving menu settings” and for “abbreviation of saving throw”. Which made stuff confusing. If unsure, you should ask somebody that does localizations and they can tell you if you split them properly or not.

## Try to avoid witty and clever messages
This is a hard one. English wits and jokes are a massive headache to localize. Try to make your UI/settings work for standard descriptions first. However, you should feel free to express your creativity with it, just let translators know that they are free to change these expressions to match the locale of where they are going to be used. Inexperienced people will try to go for literal translations and severely slow their process, while experienced people will automatically try to adapt the phrasing. However, we are afraid to infringe on the author’s rights, especially if we get called out after the fact, so please add some messages to express your willingness to have the translation adapted rather than being literally translated.

Add newly localizable strings on update notes if possible
If you would like to give more priority to localization and make localization feel welcome, it would help if you included new localizable strings in update notes. Or, express that new strings were added.

## Try to avoid flags
This is advice for both developers and translators. Try to avoid including flags when showcasing the currently supported languages or when releasing a translation module. Certain countries do not necessarily associate themselves with the flag that a language is primarily known for, and may cause unwanted political friction (trust me, you don’t want to deal with this). Try using ISO language region codes instead.

