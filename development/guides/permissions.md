---
title: Using Permissions in Foundry
description: A guide on who can do what where, and how that affects your UI
published: true
date: 2021-06-15T20:16:12.246Z
tags: 
editor: markdown
dateCreated: 2021-06-11T18:51:47.310Z
---

# Using Permissions in Foundry
## A guide on who can do what where, and how that affects your UI
> This is a stub. Some things to cover in this guide include:
> - How the core System setting permissions affect users (such as whether it is possible to upload without filepickers, etc.)
> - What fields are affected by page permissions like Owner, Observer, Limited
> - How to query permission proactively
> - General advice on UI best practices

## How to set Permissions via API
To set or change the permissions users have for a given document, you just need to update the `permission` property as follows:

```javascript
document.update({
  permission: { 
    default: CONST.ENTITY_PERMISSIONS.OBSERVER,
    [someUserId]: CONST.ENTITY_PERMISSIONS.OWNER,
  },
});
```

The permission object uses a user id as key and a numeric value representing the permission level. The `default` key can be used to set the default permission level for all users. All available permission levels cann be found in `CONST.ENTITY_PERMISSIONS`:
```javascript
{
  NONE: 0,
  LIMITED: 1,
  OBSERVER: 2,
  OWNER: 3,
}
```