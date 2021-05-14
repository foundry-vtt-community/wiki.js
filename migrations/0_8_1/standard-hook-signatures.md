---
title: Establish universal standard for hook signatures which work symmetrically for all types of Document definitions.
description: 
published: true
date: 2021-04-21T16:50:25.390Z
tags: 0.8.1
editor: markdown
dateCreated: 2021-04-14T12:44:22.241Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4658

## What changed

TL;DR - Hook signatures are now standardized in 0.8.1

The new standardized signatures are as follows:
```
preCreate[documentName](document:Document, data:object, options:object, userId:string) {}
create[documentName](document:Document, options:object, userId: string) {}
preUpdate[documentName](document:Document, change:object, options:object, userId: string) {}
update[documentName](document:Document, change:object, options:object, userId: string) {}
preDelete[documentName](document:Document, options:object, userId: string) {}
delete[documentName](document:Document, options:object, userId: string) {}
```

## What you need to change

* [Unverified] Some of these are changed from previous and your code expecting hooks will need to update.

### Research Notes

* 