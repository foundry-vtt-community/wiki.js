---
title: Move Actor owned item management methods (getOwnedItem, createOwnedItem, updateOwnedItem, deleteOwnedItem) to the deprecation path in favor of manipulating OwnedItem instances directly.
description: 
published: true
date: 2021-04-21T15:06:04.848Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:39:14.620Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4459

## What changed

Several `Actor` methods are deprecated

```js
  /**
   * @deprecated since 0.8.0
   * @ignore
   */
  getOwnedItem(itemId) {
    console.warn("The Actor#getOwnedItem method has been deprecated in favor of Actor#items#get and will be removed in 0.9.0");
    return this.items.get(itemId);
  }

  /**
   * @deprecated since 0.8.0
   * @ignore
   */
  async createOwnedItem(itemData, options) {
    console.warn("The Actor#createOwnedItem method has been deprecated in favor of OwnedItem.create and will be removed in 0.9.0");
    return OwnedItem.create(this, itemData, options);
  }

  /**
   * @deprecated since 0.8.0
   * @ignore
   */
  async updateOwnedItem(data, options) {
    console.warn("The Actor#updateOwnedItem method has been deprecated in favor of OwnedItem#update and will be removed in 0.9.0");
    const item = this.items.get(data._id, {strict: true});
    return item.update(data, options);
  }

  /**
   * @deprecated since 0.8.0
   * @ignore
   */
  async deleteOwnedItem(id, options) {
    console.warn("The Actor#deleteOwnedItem method has been deprecated in favor of OwnedItem#delete and will be removed in 0.9.0");
    const item = this.items.get(id, {strict: true});
    return item.delete(options);
  }

```

## What you need to change

- [Unverified] stop using the deprecated methods

### Research Notes

- 