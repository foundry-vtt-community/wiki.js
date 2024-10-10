---
title: Actor
description: One of the most fundamental documents within Foundry Virtual Tabletop is the Actor, as they are the protagonists, allies, monsters, antagonists, and persons within the World that you create.
published: true
date: 2024-07-30T18:16:13.052Z
tags: docs
editor: markdown
dateCreated: 2024-07-26T16:55:14.280Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Actors are the most important document type in Foundry, with unique interactions with almost every other form of document. They can represent player characters, NPCs, and in many systems even vehicles and groups of actors!

*Official Documentation*
- [Knowledge Base](https://foundryvtt.com/article/actors/)
- [Actor](https://foundryvtt.com/api/classes/client.Actor.html)
- [BaseActor](https://foundryvtt.com/api/classes/foundry.documents.BaseActor.html)
- [Document](https://foundryvtt.com/api/classes/foundry.abstract.Document.html)
- [Actors](https://foundryvtt.com/api/classes/client.Actors.html)
- [ActorDirectory](https://foundryvtt.com/api/classes/client.ActorDirectory.html)
- [ActorSheet](https://foundryvtt.com/api/classes/client.ActorSheet.html)
- [ActorSheetV2](https://foundryvtt.com/api/classes/foundry.applications.sheets.ActorSheetV2.html)

**Legend**

```
Actor.canUserCreate
Actor#isToken
```

## Overview

> Stub
> This section is a stub, you can help by contributing to it.

---
## Key Concepts

> Stub
> This section is a stub, you can help by contributing to it.

### Linked vs. Unlinked Actors

When you drag and drop an actor from the sidebar to the map, you are creating a token. The behaviour of this token will depend on the configuration of the Actor and the Prototype Token. Linked actors will update the data from one to another. Changes made on the token are reflected on the Actor.

Unlinked Actors, on the other side, creates a "copy" of the Actor for each token. Changes on the Token do not reflect on the Actor directly. Changes on the Actor will not be reflected on Tokens that already exists.

The common approach is to link the PC Characters, while leaving enemies and monsters unlinked.



> TODO
> Check if there is any easier way to configure this on the prototype token, if that requires the override of the `onCreate` method.

---
## API Interactions

> Stub
> This section is a stub, you can help by contributing to it.

---
## Specific Use Cases

> Stub
> This section is a stub, you can help by contributing to it.

### Group Actors

Many systems have implemented a "Group" actor. In short, this is an actor that has UUID links to other actors.

> Stub
> This section is a stub, you can help by contributing to it.

---
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.