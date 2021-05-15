---
title: Migration Summary for 0.8.x
description: 
published: true
date: 2021-05-15T05:28:07.911Z
tags: 
editor: markdown
dateCreated: 2021-05-01T03:24:28.830Z
---

## Documents

![documents.png](/documents.png)

> Most Likely Affects: Systems, Modules
{.is-info}

> Stub

### What changed between 0.7 and 0.8?

> Stub
> [Entity -> Document](https://foundryvtt.wiki/en/migrations/0_8_0/no-base-entity)
> All APIs standardized


## ActorSheet#getData

> Most Likely Affects: Systems
{.is-info}

Relevant Issue: [Redesign the structure of ActorSheet#getData to provide more sensible references for the actor, its data, and any items or effects that the actor owns.
](https://gitlab.com/foundrynet/foundryvtt/-/issues/4321)

ActorSheet's default `getData` method's return signature changed. If your system did not override this to create your own data for the template, your template might need adjusting.

### What changed between 0.7 and 0.8?

In 0.7 `ActorSheet#getData()#actor` was a copy of `Actor#data`. In 0.8 it is a copy of the Actor document itself. This means the path to the value being changed is no longer the same as the path to the value being displayed.

Practically this is what might need to be changed in your handlebars templates:

#### :x: 0.7
```html
<input name="data.foo" value="{{data.foo}}" />
```

#### :heavy_check_mark: 0.8
```html
<input name="data.foo" value="{{data.data.foo}}" />
```


## Hooks

> Most Likely Affects: Systems, Modules
{.is-info}

> Stub
> New Hooks Documentation: https://foundryvtt.com/api/alpha/hookEvents.html


### What changed between 0.7 and 0.8?

> Stub
> [Hook Call Signatures standardized](https://foundryvtt.wiki/en/migrations/0_8_1/standard-hook-signatures)


## Tiles

> Most Likely Affects: Modules
{.is-info}

> Stub

### What changed between 0.7 and 0.8?

> Stub
> [TilesLayer was merged into BackgroundLayer/ForegroundLayer](https://foundryvtt.wiki/en/migrations/0_8_2/tiles-layers)


## Audio

> Most Likely Affects: Modules
{.is-info}

> Stub

### What changed between 0.7 and 0.8?

> Stub
> [Howler Removed](https://foundryvtt.wiki/en/migrations/0_8_2/tiles-layers)
> [New Sound API](https://foundryvtt.com/api/alpha/AudioHelper.html)

