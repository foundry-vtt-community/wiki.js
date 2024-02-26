---
title: API Documentation
description: 
published: true
date: 2024-02-26T19:57:13.587Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:25:27.383Z
---

# API Documentation

This `/development/api/` section of the wiki is intended to be a highly structured section documenting commonly needed API classes and concepts for Foundry Core. This section is intended to supplement the Official API, not replace it, and will link to the official documentation as often as is practical.

If a page appears here which does not conform the to style notes below it may be completely rewritten, moved into the /guides directory, or deleted entirely.

## Official API
Atropos has a Developer API Documentation section on the official `foundryvtt.com` website [here](https://foundryvtt.com/api/).

This is generated from the JSDoc comments within `foundry.js` that is accessible from every local installation of Foundry. It is advisable to open this file up and search it in your IDE, as all of the JSDoc comments are present and you can easily see what is really going on in the code. The bundled code can be found in `yourFoundryInstallLocation/resources/app/public/scripts/foundry.js`, while the unbundled code can be found in `yourFoundryInstallLocation/resources/app/client/` and `yourFoundryInstallLocation/resources/app/common/`.

---

## Exisiting Documentation

### [Application](/en/development/api/application)

#### Topics
- Common Methods (render, getData, activateListeners)
- Automatic rerendering on updates

### [Compendium Collection](/en/development/api/CompendiumCollection)
Compendiums and how data is stored

#### Topics
- Accessing compendiums
- Fetching documents
- Updating documents
- The FoundryVTT CLI

### [Data Model](/en/development/api/DataModel)
- Data Model Schema
- Registering Data models
- TypeDataModel
- Data Models for Settings

### [Dialog](/en/development/api/dialog)
API documentation for the Dialog UI class used to inform and prompt users with simple popups.

#### Topics
- Use with a Promise or Callback
- Prompt and Confirm factories
- Form Inputs with Dialogs


### [Document](/en/development/api/document)
Everything you need to know about the data architecture of Foundry VTT Core (aka Documents).

#### Topics
- Primary vs Embedded Documents
- Document Data organization
- Document Modification Context
- How to Create/Read/Update/Delete a Document
- Document CRUD Event Cycles


### [Flags](/en/development/api/flags)
Everything you need to know about using Flags to store arbitrary data on Documents.

#### Topics
- Location on the Schema
- Flag Scopes and Data Structure
- How to Set/Read/Unset a Flag
- Gotchas with Objects

### [Helpers and Utils](/en/development/api/helpers)
Independently useful functions in the Foundry API

#### Topics
- foundry.utils
- HandlebarsHelpers
- Primitive extensions

### [Hooks](/en/development/api/hooks)
Everying you need to know about working with and creating Hook events.

#### Topics
- Registering Hook Callbacks
- Calling Hooks

### [Localization](/en/development/api/localization)
A helper class which assists with localization (i18n) and string translation

#### Topics

- Localizing in JS
- Localizing in Handlebars
- Localization best practices

### [Settings](/en/development/api/settings)
Everything you need to know about using Settings for your package.

#### Topics
- World vs Client setting scope
- Registering Settings
- How to Create/Read/Update/Delete a Setting
- Setting Menus
- Possible Setting configurations


### [Sockets](/en/development/api/sockets)
API documentation for the Socket functionality available to packages.

#### Topics
- Prerequisites
- Emitting events
- Responding to broadcasts


---

## Style Notes

Each page in this directory should conform to the following stylistically:


### Interlinking

As often as is practical, link between pages of these documents, and to credible external sources of knowledge. The [Wikipedia Manual of Style](https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style/Linking) has excellent examples of "underlinking" and "overlinking".

Some guiding questions to ask:

- Will linking to something enable the reader to understand this content better?
- Has this (other) page been linked to recently within this document?

#### Examples of Credible External Sources

- foundryvtt.com/api Official Documentation
- foundryvtt.com Knowledge Base

### Relevant Version Annotation

A shield.io badge must be placed at the top of every page which details the "core version we know this document is up to date as of." This allows readers and contributors alike to know at a glance if the document they're looking at is potentially out of date.

![Up to date as of v8](https://img.shields.io/badge/FoundryVTT-v8-informational)

```
![Up to date as of <VERSION>](https://img.shields.io/badge/FoundryVTT-v<VERSION>-informational)
```

### Asset Management (Images, etc.)

#### Naming

Name the page the same as the Class or Concept it documents. The goal is to be intuitive when scanning or searching for a specific chunk of the API a user might be having a problem with.

E.g. `Document` describes Document, DocumentData, and some Collection logic. All of these feasibly fit together within one page.

#### Placement

Assets for pages must be in a similarly named subdirectory to the page using them.

#### Example

```
/development/api
└── document
    └── document-console.png

```

The `development/api/document` page references a `document-console` image which is housed under the `development/api/document` asset directory.

### Voice

The following apply to all pages outside of the "Guides" section. Guides are subject to a more relaxed set of guidelines and may exhibit more personal voice if the author chooses.

#### Be concise

Write as little as is necessary to get the concept across. Our goal is to be skimmable so that GMs and Players who are in a hurry can find what they are looking for quickly.

#### Be formal

Do not reference the user/developer with "you" but instead choose wording such as "the user." Always reference "the user" or an Actor with the pronoun "they."

Emulate something which could be found in official documentation for a company's products, or something akin to large video gaming wikis (e.g. [The Official Minecraft Wiki](https://minecraft.fandom.com/wiki/Minecraft_Wiki)).

#### Strive to fit in

This documentation should read as though it were written in one voice, not many. Do your best to mimic the style of an existing page or section which applies to your content.

#### Own this collectively, not individually

Do not take it personally if someone's contributions rewrite portions of your own in order to either update them, correct them, or conform them to a shifting standard. We strive for a culture of working together to improve these docs.

## Page Anatomy

Each API page should have the following at Heading Level 2 (as the main title of the page is heading level 1):

1. **Overview** - A short section containing an introduction to the API section being documented. This section's objective is to confirm to the user they are looking at the right palce.
2. **Key Concepts** - Explain any overarching concepts which help the user understand the _why_ here.
3. **API Interactions** - Individual api methods and properties should be documented here. Do not document every method or property the way the official docs do, instead focus on the parts which are most likely to be useful.
4. **Specific Use Cases** - Snippets for accomplishing specific goals related to the API concept being explored. These snippets might require the use of several methods or properties strung together.
5. **Troubleshooting** - Any common errors that are easy to run into should be documented as well as some common reasons why the error might be appearing.

Each heading level 2 section should have a horizontal rule between them.

Other sections can be appended to the end of the document as deemed necessary.

### Stubs

If a section has no content yet, place a stub inside a blockquote:
```
> Stub
> This section is a stub, you can help by contributing to it.
```
