---
title: The base Entity class has been deprecated in favor of a DocumentMixin combined with AbstractBaseDocument class definitions.
description: 
published: true
date: 2021-02-07T17:09:56.382Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:09:56.382Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4380

## What changed

Entity has refactored in a big way to accomodate the new `AbstractBaseData` infrastucture. See the issue for more details.

## What you need to change

- [Unverified] `Entity#collection` formerly returned a `DocumentCollection` instance, but in order to standardize with the symmetric server-side model this property now only returns the collection name. `Entity#parentCollection` is now required to access the parent collection instance.


- [Unverified] `Entity.update(data, options)` has been deprecated in favor of `DocumentCollection#update(data, options)`. Backwards-compatible support will be preserved until 0.9.0.

- [Unverified] `Entity.delete(ids, options)` has been deprecated in favor of `DocumentCollection#delete(ids, options)`. Backwards-compatible support will be preserved until 0.9.0.

- [Unverified] Data preparation has been standardized across all document types. If you were previously defining the `prepareData` method for an `Entity` type, you need to look closely at the default behavior of `DocumentMixin#prepareData`. It is **critical** to either call `super.prepareData()` when extending this method OR to move your extensions to `prepareDerivedData()` method which runs after other preparation steps.


- [Unverified] Assignment of a document subclass implementation was previously performed as `CONFIG[documentName].entityClass`. This assignment is now performed as `CONFIG[documentName].documentClass`. Support for the old assignment will be preserved until 0.9.0.

### Research Notes

- 