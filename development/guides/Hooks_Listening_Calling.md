---
title: Hooks  Listening & Calling
description: a guide on how to piggyback on Foundry's API
published: true
date: 2021-08-19T11:50:03.052Z
tags: 
editor: markdown
dateCreated: 2021-08-13T11:35:11.211Z
---


> This document is up to date as of 0.8.8
{.is-info}


The Foundry API provides it's own event system for developers to take advantage of in the form of Hooks.
Most of the time, it's used to piggyback on a variety of runtime events, ranging from a simple `'hoverToken'` (the mouse entering/exiting a token area) to **any Document update** sent to the database, through **every rendering of any Application**.
But, like anything Foundry, it can be daunting at first, and there's definitely more than meets the eye.

If you don't care about the specifics, you can skip ahead to the good stuff: [registering a hook callback](#answering-the-call-how-to-register-and-unregister-a-hook-callback).

# The Hooks class
Although the Foundry API documentation mentions a constructor to get new instances of the [Hooks](https://foundryvtt.com/api/Hooks.html) class, it's only used as a 'global constant', providing **a set of static methods** (and some private attributes), hence the use of a capital 'H' in the syntax `Hooks.once('init', ...` that most of us declared at some point.
Some Foundry 'events' use a `Hooks.call()` or a `Hooks.callAll()`, with a hard-coded or dynamic hookName as well as a various number of arguments, for example:
```javascript
Hooks.callAll("ready");
// called when the game is done initializing its data structure and is 'ready'
``` 
```javascript
Hooks.call("dropCanvasData", this, data);
// when anything is dragDropped onto the canvas
// ('this' being the canvas instance) 
``` 
```javascript
Hooks.callAll(`update${doc.documentName}`, doc, change, options, userId);
// that will trigger for any document type,
// giving us a wide range of hookNames to deal with like
// 'updateActor', 'updateMacro', 'updateChatMessage'...
``` 
At which point the Hooks class will check if any hook has been registered with that name and, for each one, will execute it's accompanying function. If you did not register a specific hook all this does is just screaming into the void.

### .on or .once:
In order to receive the call, and piggyback on the triggering event, you must have registered a hook with a matching name and the callback function to be executed.
Hooks can be registered either with `Hooks.on()` or `Hooks.once()`. In both cases the callback is stored in a private variable for later use, the former remaining callable until manually unregistered, and the later being removed with a `Hooks.off()` after it's first and only callback has been executed.

**ⓘ** It is worth noting that the hook registering function **returns the index of the newly created hook**, that can be used as an argument for the unregistering process.


### .call or .callAll and more...
Not all calls are created equal, and it can impact the extent of what you can do when answering them.
`Hooks.callAll()` will execute All the callback functions that were registered for that event, regardless of what they're actually doing.

```javascript
Hooks.on("init", async function () {
//obviously triggered by a Hooks.callAll() that's gonna call every module/system that registered 'init', no matter what
});
``` 

`Hooks.call()` will execute all the callback functions until the end of it's array or if any one of the callback returns false. In the later case, one usually returns false to signify that the hook has been dealt with and no further calls are needed.

```javascript
Hooks.on("renderCompendium", (compendiumApp, html, appData) => {
//TODO: add real world, working example
});
``` 

But there's **one last relevant case** regarding `Hooks.call()`. Since the call waits for a returned boolean, in order to decide whether it's worth calling the rest of the callback stack, some Foundry events use that fact to grant the callback the power to decide if the calling event should proceed with it's natural conclusion.

For example, returning `false` in a `Hooks.call("dropCanvasData")` callback will **prevent the placeableObject to be created on the canvas.**

```javascript
  //if there's a canvas init, register a permanent protection against some 'types'
  Hooks.once('canvasInit', (canvas) => {
    Hooks.on("dropCanvasData", (canvas, dropData) => {
      if ( dropData.type === 'Actor') {
        const actor = game.actors.get(dropData.id);
        if ( actor.name === 'Xx_DarkSasuke69420_xX' ) { return false; }
      }
    });
  });
``` 
Unfortunately, you cannot know, without searching in foundry.js, if a call is interruptible or not (thus returning false being sometimes meaningless).

# Where there's a hook, there's a way (Knowing what to hook in to)
Knowing if a hook even exists and it's argument list could be painfull, but hopefully, Foundry conveniently provides a Boolean that can be toggled for that : 
This can be typed directly in the console or in your `Hooks.on('init', ...)`, for instance.

```javascript
CONFIG.debug.hooks = true;
``` 

An **even more convenient way** is having a 'script' macro to toggle it on/off *(courtesy from @Eunomiac#8172, on the League's discord server)* : 
```javascript
CONFIG.debug.hooks = !CONFIG.debug.hooks;
if (CONFIG.debug.hooks)
    console.log("NOW LISTENING TO ALL HOOK CALLS!");
else
    console.log("HOOK LISTENING DISABLED.");
``` 

> Bonus Tip: Install [Developer Mode](https://www.foundryvtt-hub.com/package/_dev-mode/) and you can explore the hooks debug more easily.
{.is-info}

From there, it's pretty straight forward to just _'do an action in game'_ and see if any hook is called, how many arguments it has and their nature/content.
![debug hooks](https://drive.google.com/uc?export=view&id=1FEDnrXtThbxhDVghh0NGyzGw9wsMyxWI)

We can see that, in accordance with [the example above](#the-hooks-class), we do indeed have 
* a document, here a ChatMessage
* an object containing what has changed (what was updated)
* the options passed to the update call
* the id of the user that triggered the update


### List of hooks
> The Foundry VTT API documentation provides a [List of hookEvents](https://foundryvtt.com/api/hookEvents.html), as the following list lacks context and might not stand the passage of future versions.
{.is-info}

<details>
  <summary>Spoiler: Raw list as of 0.8.8</summary>
  
Hooks.callAll() 
```javascript
Hooks.callAll("updateWorldTime", worldTime, dt)
Hooks.callAll("init")
Hooks.callAll('setup')
Hooks.callAll("ready")
Hooks.callAll("pauseGame", this.data.paused)
Hooks.callAll(`create${type}`, doc, options, userId)
Hooks.callAll(`update${doc.documentName}`, doc, change, options, userId)
Hooks.callAll(`delete${doc.documentName}`, doc, options, userId)
Hooks.callAll("updateCompendium", this, documents, options, userId)
Hooks.callAll(`preCreate${embeddedName}`, doc, d, options, user.id)
Hooks.callAll(`preUpdate${embeddedName}`, doc, d, options, user.id)
Hooks.callAll(`preDelete${embeddedName}`, doc, options, user.id)
Hooks.callAll("preUpdateActor", this.actor, data, options, user.id)
Hooks.callAll(`create${embeddedName}`, doc, options, userId)
Hooks.callAll(`update${embeddedName}`, doc, d, options, userId)
Hooks.callAll(`delete${embeddedName}`, doc, options, userId)
Hooks.callAll("updateActor", this.actor, data, options, userId)
Hooks.callAll('canvasInit', this)
Hooks.callAll("canvasPan", this, constrained)
Hooks.callAll("canvasPan", this, {x: stage.pivot.x, y: stage.pivot.y, scale: stage.scale.x})
Hooks.callAll("control"+this.constructor.embeddedName, this, this._controlled)
Hooks.callAll("control"+this.constructor.embeddedName, this, this._controlled)
Hooks.callAll("hover"+this.constructor.embeddedName, this, this._hover)
Hooks.callAll("hover"+this.constructor.embeddedName, this, this._hover)
Hooks.callAll("collapseSidebar", this, this._collapsed)
Hooks.callAll("collapseSidebar", this, this._collapsed)
Hooks.callAll("changeSidebarTab", app)
Hooks.callAll(`getSceneControlButtons`, controls)
Hooks.callAll("collapseSceneNavigation", this, this._collapsed)
Hooks.callAll("collapseSceneNavigation", this, this._collapsed)
Hooks.callAll("initializePointSourceShaders", this, this.animation.type)
Hooks.callAll("targetToken", this.user, token, targeted)
Hooks.callAll("lightingRefresh", this)
Hooks.callAll("sightRefresh", this)
Hooks.callAll("rtcSettingsChanged", this, changed)
Hooks.callAll(`${key}Changed`, volume)
```

Hooks.call() :
```javascript
Hooks.call(`render${cls.name}`, this, html, data)
Hooks.call(`get${cls.name}HeaderButtons`, this, buttons)
Hooks.call(`close${cls.name}`, this, el)
Hooks.call("applyActiveEffect", actor, change)
Hooks.call("renderChatMessage", this, html, messageData)
Hooks.call("canvasReady", this)
Hooks.call(`paste${cls.name}`, this._copy, toCreate)
Hooks.call(`get${cls.name}FolderContext`, html, folderOptions)
Hooks.call(`get${cls.name}EntryContext`, html, entryOptions)
Hooks.call("getSceneNavigationContext", html, contextOptions)
Hooks.call(`getUserContextOptions`, html, contextOptions)
Hooks.call(`get${cls.name}EntryContext`, html, entryOptions)
Hooks.call(`paste${cls.name}`, this._copy, toCreate)
``` 

Hooks.call() with interruption  
```javascript
Hooks.call(`preCreate${type}`, doc, createData, options, user.id)
Hooks.call(`preUpdate${doc.documentName}`, doc, update, options, user.id)
Hooks.call(`preDelete${doc.documentName}`, doc, options, user.id)
Hooks.call("modifyTokenAttribute", {attribute, value, isDelta, isBar}, updates)
Hooks.call("dropCanvasData", this, data)
Hooks.call("dropActorSheetData", actor, this, data)
Hooks.call("dropRollTableSheetData", this.document, this, data)
Hooks.call("chatBubble", token, html, message, {emote})
Hooks.call("hotbarDrop", this, data, slot)
Hooks.call("chatMessage", this, message, chatData)
Hooks.call(`get${cls.name}EntryContext`, html, entryOptions)
Hooks.call(`get${cls.name}EntryContext`, html, entryOptions)
Hooks.call(`get${cls.name}SoundContext`, html, entryOptions)
```
</details>

# Answering the call: How to Register (and Unregister) a Hook Callback 
## Registering a hook callback
It's JavaScript, so there are many ways to implement your hook callback. ie inline, inline with an arrow function, by reference, with or without arguments, with a .bind(this)...
```javascript
//classic inline callback function
Hooks.once('init', async function () {
  console.log('M20E | Initialisation du système');
}
``` 
```javascript
//callback with selected arguments. If the args were not declared prior to the function reference, then
//createMacro would get 'bar' and 'dropedData' as arguments
Hooks.on('hotbarDrop', (bar, dropedData, slot) => createMacro(dropedData, slot));
```
```javascript
//callback with no arguments declared, theses can be declared in the function definition
//in that case we use a .bind(this) for the function (unless static) is specific to the instance it's in
//also, keeping a reference to the hook index for later unregister
const myHookId = Hooks.on('updateActor', this.onUpdateActor.bind(this));

//function declaration (same context) the argument are declared in the same order as the original call
onUpdateActor(actor, data, options, userId) {
//do my things here
}
```

### Where should I declare my hook ?
Most general purpose hooks would usually be in your main module file (though I would advise for some restraint on the inline declarations, but that's besides the point), and more specific ones in the classes that need them.
However some events are dependent on other one (ie a dragDrop on the canvas will only happen after the canvas own init, as in [the example above](#call-or-callall-and-more)), hence often declaring a Hooks.on inside the callback for another hook.
Weirdly enough, I once thought I could declare `Hooks.on('renderChatLog', chat.addChatListeners);` inside the 'ready' hook. Well tuns out it doesn't work and has to be declared on the same level as said 'ready' hook.
<br/>

### ⚠ the issue with having multiple clients.
Some game events triggered by a user, can generate a hook call for all connected clients. Depending on what you use it for, that can be an unwanted behavior. **Hence the need for some checks in your hooks.**
A classic example of this, is the use of the 'actorCreate' hook to populate a list of default items (that's prior 0.8.x and the advent of the `#_preCreate()` 👍 ).
```javascript
Hooks.on('createActor', async function (actor, options, userID) {
  //check current user is the one that triggered the création
  //(we wouldn't want to add abilities to the actor multiple times)
  if (userID != game.user.id) { return;}
```

## Removing a hook callback
Sometimes it can be usefull (and not only good practice) to unregister a hook callback.
> As a matter of fact, since the Hooks object **keeps a reference** to each callback instance that was registered, it stands to reason that there cannot be any [**garbage collection**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management#references).
{.is-warning}

ie : If you've declared a hook in an Application instance, **closing said application will not free up the memory** for there is still a reference to it somewhere in the Hooks object (and the app instance will still respond to it's calls after being closed).
Hence the need for **proper hook disposal** :  

### removing by index
It's as easy as `Hooks.off('updateActor', myHookIndex);` but it obviously **requires having stored the hookIndex** on registration.
One could imagine storing pairs of {hookName, hookIndex} in order to later unregister multiple hooks in a row.

### removing by function reference
This way might seem easier (and cleaner, codewise), but in some instances, it can prove challenging. Basic syntax, using previous example would be
`Hooks.off('updateActor', this.onUpdateActor);`.
But, remember how we declared said hook [earlier](#registering-a-hook-callback) using a `.bind(this)` ?
Well this is going to make things harder.

In this instance, `Hooks.off('updateActor', this.onUpdateActor);` will fail to remove the hook, as well as `Hooks.off('updateActor', this.onUpdateActor.bind(this));`.
A solution to this, *courtesy of @Calego#0914* is to declare the callback as an arrow function like so:

```javascript
  onUpdateActor = (actor, data, options, userId) => {
    if ( actor.id !== this.diceThrow.actor.id ) { return ; }
    //...
  }
```
Using an arrow declaration implies a 'this' being in a broader context than just the function itself, in that case, our class instance. This makes .bind(this) useless, allows for a cleaner declaration, and finally makes our unregister by reference possible : 
```javascript
 export default class DiceDialog extends Application {
  
  /** @override */
  constructor(diceThrow, options) {
    super(options);
    //register a hook on updateActor in order to refresh the diceThrow with updated actor values.
    Hooks.on('updateActor', this.onUpdateActor); //no binding required !
    Hooks.on('updateCoreRollMode', this.onUpdateCoreRollMode);
    Hooks.on('systemSettingChanged', this.onSystemSettingChanged);
  }

  //our actual arrow declaration
  onUpdateActor = (actor, data, options, userId) => {
    if ( actor.id !== this.diceThrow.actor.id ) { return ; }
    //...
  }

  //easy clean up
  async close(options={}) {
    //do some cleaning
    Hooks.off('updateActor', this.onUpdateActor);
    Hooks.off('updateCoreRollMode', this.onUpdateCoreRollMode);
    Hooks.off('systemSettingChanged', this.onSystemSettingChanged);
    this.diceThrow = null; //remove circular reference
    
    return super.close(options);
  }
}
```

# Calling: When there's no hook to hold on to
At some point, you might want to create your own calls, be it to enrich the API your module provides or just to facilitate your own coding implementation.
> The [Library Module](https://foundryvtt.wiki/en/development/library-modules) page, provides a good example of a 'Hooks.callAll' in a module to advertize it's readyness alongside a reference to it's own API.
{.is-info}

> It is worth mentionning at this point that hook calls are not sockets. Although some core events might give the impression that a single call happens for every client, hooks calls are local.
{.is-warning}

A more 'niche' use case, if you need a hook for some core event that doesn't provide feedback. The following example adds a callAll() when the user changes it's rollMode using the dropDown list from the chatLog side panel : 
```javascript
Hooks.once('ready', async function () {

    //adds a Hooks.call on rollMode setting change
    const onRollModeChange = function(newRollMode) {
        ChatLog._setRollMode(newRollMode); //the original onChange function
        Hooks.callAll('updateCoreRollMode', newRollMode);
    };

    //replace the original onChange function (_setRollMode) with our own that actually provides a Hooks call
    const rollModeSetting = game.settings.settings.get('core.rollMode');
    rollModeSetting.onChange = onRollModeChange;
});
``` 
