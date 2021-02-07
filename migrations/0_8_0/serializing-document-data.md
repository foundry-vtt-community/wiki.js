---
title: Serializing a DocumentData instance will now only return the _source component of that data which needs to be persisted in the database rather than the full data object including derived data elements or downstream transformations
description: 
published: true
date: 2021-02-07T17:43:44.626Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-07T17:43:44.626Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4090

## What changed

When preparing data for an `ActorSheet` it is possible someone is duplicating the actor data in order to avoid accidentally mutating some of the values.
```js
getData() {
  return {
    data: duplicate(this.actor.data)
  }
}
```
This will now have some unintended effects because the duplicated data will only contain `this.actor.data._source` rather than the derived properties that were defined.



## What you need to change

- [Unverified] Employ this workaround:

> Simple Workaround
> The simplest solution is to switch to using `foundry.utils.deepClone` (which does not use `JSON.stringify`) instead of `foundry.utils.duplicate` when deep-copying a data object where you want to keep derived elements in the copied output.
> ```js
> getData() {
>   return {
>     data: foundry.utils.deepClone(this.actor.data)
>   }
> }
> ```
> 
> Bonus Points: deepClone is faster than duplicate

### Research Notes

- 