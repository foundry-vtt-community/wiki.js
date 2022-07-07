---
title: Migration Summary for v10
description: 
published: true
date: 2022-07-07T04:22:30.623Z
tags: 
editor: markdown
dateCreated: 2022-03-23T23:44:16.293Z
---


> Updated as of v10 - Prototype 1
{.is-info}

## Documents

![migration-v10.png](/migrations/foundry-core-v10/migration-v10.png)
*Thanks Spice_King for the spicy memes.*

> Most likely affects: Systems, Modules
{.is-info}

### Issues worth Reading
- [Rationale and Technical Discussion](https://github.com/foundryvtt/foundryvtt/issues/6841)
- [Overview of breaking changes](https://github.com/foundryvtt/foundryvtt/issues/6849)

### What changed between v9 and v10?

> There is currently a compatibility shim in place which softens the impact of this change. This compatibility layer aims to provide forwards and backwards compatiblity to the greatest extent possible.
>
> Methods involved:
> - `DataModel.migrateData`
> - `DataModel.shimData`


`Document`s have changed from having a `DocumentData` instance as the `data` attribute of the Document instance, to instead being a subclass of `DocumentData` directly.

Practically this means for example that attributes previously accessible from `someDocument.data.field` are now located at `someDocument.field`.

#### `system` data

Instead of being at `someDocument.data.data`, all system-schema defined data is now accessible at `someDocument.system`.

```javascript
// v9
const foo = someDocument.data.data.some.path;

// v10
const foo = someDocument.system.some.path;
```

#### `update` methods

The old `DocumentData#update` method which was used to update the local document without touching the database has been moved to `DataModel#updateSource`

```javascript
// v9
someDocument.data.update({ 'some.path': 'new value' });

// v10
someDocument.updateSource({ 'some.path': 'new value' });
```

## Manifest deprecations

Sevral manifest changes have no reached the end of their deprecation periods. Consequently, it's recommended to check your server logs as part of the migration process to verify that your package is using the latest manifest fields.
The issue on the GitLab for this is [#6666](https://github.com/foundryvtt/foundryvtt/issues/6666) 😈
In addition to the changes mentioned in that issue, it seems that using this formulation in your `languages` path field will now throw an error: `modules/myModule/lang/en.json`.
This seems to be a common pattern despite it being against the [documentation](https://foundryvtt.com/article/localization/) (which is not new):
> A path relative to the root directory of the manifest where localization strings are provided in JSON format.
The correct format is just `lang/en.json` if you have your localization files in a `lang` directory that is on the same level as your manifest.
