---
title: Serializing a DocumentData instance will now only return the _source component of that data which needs to be persisted in the database rather than the full data object including derived data elements or downstream transformations
description: 
published: true
date: 2021-04-21T15:06:12.452Z
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

## Research Notes


## Q&A

> **Q:** Does this mean that derived data (for instance a spell save DC or skill modifier in dnd5e) will not show up in the source document dbs going forward (only those fields that show up in the system template.json in the case of actors/items)? Will some kind of migration be required or perhaps be performed automatically to cull data that does not conform to the source template? 
>
> **A:** Yes, it means that only the base data will be stored to the database (as intended). Derived data should be computed in-memory using that base data only. It won't do any harm if some of the derived data is in the db as well (aside from inflating the filesize somewhat), but it would be ideal to clean it out at some point. I plan to provide some convenience functions for Actor/Item migration which can cull any data elements out of the object which are not explicitly defined in the base data.
> [Discord Link](https://discord.com/channels/170995199584108546/811676497965613117/811954014207737886)


> **Q:** I'm mostly using duplicate () to ensure that changes I make to the data before update are registered as changes on update. Given that deepClone is faster anyway, would it make sense to just switch every instance of duplicate to deepClone?
>
> **A:** You should consider replacing some instances of this, but probably not all:
>
> Since duplicate relies upon JSON.stringify it has a side-effect that it calls the toJSON method of any object which defines one. This is especially relevant in Foundry because the Document classes all define custom toJSON methods for how they should be serialized. If you, for example call duplicate(actor.data) you may be surprised to get the base data for the actor rather than the object which contains all the transformed/derived data elements. 
>
> Also duplicate is tolerant of encountering some object types that deepClone may fail for because those objects have defined tolerant ways to serialize to JSON.
>
> **Follow Up Q:** I see, in my case I'm not using any derived data from prepareData or similar because everything on my characters is stored in templated objects (though those may be empty objects or arrays in system.json) in which case it sounds like duplicate remains safe for me.
>
> **A:** when you're dealing with copying actor data in particular, I advise you to try/compare both methods to be sure you're not seeing anything unexpected
>
> [Discord Link](https://discord.com/channels/170995199584108546/811676497965613117/811955931638005800)