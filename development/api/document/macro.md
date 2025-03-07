---
title: Macro
description: A document that contains information for executing a chat command or script
published: true
date: 2025-03-07T17:05:28.288Z
tags: 
editor: markdown
dateCreated: 2025-03-07T17:05:28.288Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Macros are a simple but powerful document type that allow users to execute arbitrary code.

*Official Documentation*
- [Knowledge Base](https://foundryvtt.com/article/macros/)
- [Macro](https://foundryvtt.com/api/classes/client.Macro.html)
- [BaseMacro](https://foundryvtt.com/api/classes/foundry.documents.BaseMacro.html)
- [Document](https://foundryvtt.com/api/classes/foundry.abstract.Document.html)
- [Macros](https://foundryvtt.com/api/classes/client.Macros.html)
- [MacroDirectory](https://foundryvtt.com/api/classes/client.MacroDirectory.html)
- [MacroConfig](https://foundryvtt.com/api/classes/client.MacroConfig.html)
- [Hotbar](https://foundryvtt.com/api/classes/client.Hotbar.html)

**Legend**

```
Macro.DEFAULT_ICON
Macro#execute
```

## Overview

> Stub
> This section is a stub, you can help by contributing to it.

---
## Key Concepts

> Stub
> This section is a stub, you can help by contributing to it.

---
## API Interactions
While macros are fairly simple documents with relatively few properties and methods, they still inherit the full set of functionality from Document and ClientDocument.

### Execute Scope

> Stub
> This section is a stub, you can help by contributing to it.

---
## Specific Use Cases
Here are some helpful tips and tricks for working with macros as a content developer.

### Exposing a package API

As a package developer for a system or module, you may want to provide a set of functions that users can invoke in macros. A common pattern is to create an `api` property on your package and add the functions there.

```js
function myFunc(scope) {}

Hooks.once("init", () => {
  // for a System developer
  game.system.api = { myFunc }
  // for a Module developer
  game.modules.get("my-module").api = { myFunc }
});
```

If you are already carving out a namespace in `globalThis` you can put these functions there instead. The goal is that actual macro invocations will be one liners like the following.

```js
game.modules.get('my-module').api.myFunc({ actor })
```

This is helpful because if you update the internals of `myFunc`, all users immediately benefit - no need to copy or import updated macro code.

---
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.