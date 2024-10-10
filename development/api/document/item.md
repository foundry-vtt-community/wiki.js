---
title: Item
description: A document that represents any discrete piece of rules content
published: true
date: 2024-07-26T17:04:24.725Z
tags: documentation
editor: markdown
dateCreated: 2024-07-26T17:04:22.131Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Items are more than just physical gear; they can represent any discrete piece of rules content that can be added to an [Actor](/en/development/api/document/actor).

*Official Documentation*
- [Knowledge Base](https://foundryvtt.com/article/items/)
- [Item](https://foundryvtt.com/api/classes/client.Item.html)
- [BaseItem](https://foundryvtt.com/api/classes/foundry.documents.BaseItem.html)
- [Document](https://foundryvtt.com/api/classes/foundry.abstract.Document.html)
- [Items](https://foundryvtt.com/api/classes/client.Items.html)
- [ItemDirectory](https://foundryvtt.com/api/classes/client.ItemDirectory.html)
- [ItemSheet](https://foundryvtt.com/api/classes/client.ItemSheet.html)
- [ItemSheetV2](https://foundryvtt.com/api/classes/foundry.applications.sheets.ItemSheetV2.html)

**Legend**

```
Item.canUserCreate
Item#actor
```

## Overview

> Stub
> This section is a stub, you can help by contributing to it.

---
## Key Concepts

> Stub
> This section is a stub, you can help by contributing to it.

### Linked vs. Unlinked Actors

> Stub
> This section is a stub, you can help by contributing to it.

---
## API Interactions

> Stub
> This section is a stub, you can help by contributing to it.

---
## Specific Use Cases

> Stub
> This section is a stub, you can help by contributing to it.

### Containers

A common request within Foundry is being able to embed items within other items, e.g. putting a bedroll in a backpack. Unfortunately, the underlying database validation does not permit this, but you can accomplish containers with some rendering logic to hide items within a container while still keeping them adjacent in the parent collection.

> Stub
> This section is a stub, you can help by contributing to it.

### Item Grants

Many systems have implemented a "Item Grants". In short, this is a way for Items to add other items, such as an RPG class adding class features.

> Stub
> This section is a stub, you can help by contributing to it.

---
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.