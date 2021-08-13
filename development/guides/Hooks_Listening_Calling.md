---
title: Hooks  Listening & Calling
description: a guide on how to piggyback on Foundry's API
published: true
date: 2021-08-13T11:35:11.211Z
tags: 
editor: markdown
dateCreated: 2021-08-13T11:35:11.211Z
---

The Foundry API provides it's own hooking system for us to take advantage of.<br/>
Most of the time, it's used to piggyback on a variety of runtime events, ranging from a simple `'hoverToken'` (the mouse entering/exiting a token area) to **any Document update** sent to the database, through **every rendering of any Application**.<br/>
But, like anything Foundry, it can be daunting at first, and there's definitely more than meets the eye.<br/>

If you don't care about the specifics, you can skip to the good part : [registering a hook](#answering-the-call--how-to-register-and-unregister-a-hook-).

# The Hooks class
Although the Foundry API documentation mentions a constructor to get new instances of the [Hooks](https://foundryvtt.com/api/Hooks.html) class, it's only used as a 'global constant', providing **a set of static methods** (and some private attributes), hence the use of a capital 'H' in the syntax `Hooks.once('init', ...` that most of us declared at some point.

Some Foundry 'events' use a `Hooks.call()` or a `Hooks.callAll()`, with a hard-coded or dynamic hookName as well as a various number of arguments.<br/>
ie :
```javascript
Hooks.callAll("ready");
``` 
called at the beginning of the initialize sequence of a Game,<br/>
```javascript
Hooks.call("dropCanvasData", this, data);
``` 
when anything is dragDropped onto the canvas ('this' being the canvas instance) 
```javascript
Hooks.callAll(`update${doc.documentName}`, doc, change, options, userId);
``` 
that will trigger for any document type, giving us a wide range of hookNames to deal with like 'updateActor', 'updateMacro', 'updateChatMessage'...<br/>
At which point the Hooks class will check if any hook has been registered with that name and, for each one, will execute it's accompanying function. If you did not register a specific hook all this does is just screaming into the void.

### .on or .once :
In order to receive the call, and piggyback on the triggering event, you must have registered a hook with a matching name and the callback function to be executed.
Hooks can be registered either with `Hooks.on()` or `Hooks.once()`. In both cases the callback is stored in Hooks._hooks for later use, the former remaining callable until manually unregistered, and the later being removed with a `Hooks.off()` after it's first and only callback has been executed.<br/>
ⓘ It is worth noting that the hook registering function **returns the index of the newly created hook**, that can be used as an argument for the unregistering process.<br/>
![Hooks._hooks](https://drive.google.com/uc?export=view&id=1kbuBCf2z0hC-pxeIscxrkg_BV9qDg6kB)
<br/>
Logging the hook container shows all the registered hooks along with their array of callback functions. Since 'init' and 'ready' were already called (and registered for a one time only) they now display as empty arrays.

### .call or .callAll or more...
Not all calls are created equal, and it can impact the extent of what you can do when answering them.
Hooks.callAll() will execute All the callback functions that were registered for that event, regardless of what they're actually doing. ie :<br/>
```javascript
Hooks.on("init", async function () {
//obviously triggered by a Hooks.callAll() that's gonna call every module/system that registered 'init', no matter what
});
``` 
Hooks.call() will execute all the callback functions until the end of it's array or if any one of the callback returns false.
In the later case, one usually returns false to signify that the hook has been dealt with and no further calls are needed.
```javascript
Hooks.on("renderCompendium", (compendiumApp, html, appData) => {
//TODO : add real world, working example
});
``` 
But there's one last relevant case regarding Hooks.call(). Since the call waits for a returned boolean, in order to decide whether it's worth calling the rest of the callback stack, some Foundry events use that fact to allow the callback the power to decide if the calling event should proceed with it's natural conclusion.<br/>
ie : returning false when answering a Hooks.call("dropCanvasData") will prevent the placeableObject to be created on the canvas.
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
Unfortunately, you cannot know, without searching in foundry.js, if a call is interruptible or not (thus return false sometimes being meaningless).

# Where there's a hook, there's a way (Knowing what to hook on to)
Knowing if a hook even exists and it's arguments could be painfull, but Foundry conveniently provides a Boolean that can be toggled for that : 
```javascript
CONFIG.debug.hooks = true;
``` 
This can be typed directly in the console or in your `Hooks.on('init', ...)`, for instance.<br/>
An **even more convenient way** is having a 'script' macro to toggle it on/off _(from @Eunomiac#8172, on the League's discord server)_ : 
```javascript
CONFIG.debug.hooks = !CONFIG.debug.hooks;
if (CONFIG.debug.hooks)
    console.log("NOW LISTENING TO ALL HOOK CALLS!");
else
    console.log("HOOK LISTENING DISABLED.");
``` 
From there, it's pretty straight forward to just _'do an action in game'_ and see if any hook is called, how many arguments it has and their nature/content.<br/>
![debug hooks](https://drive.google.com/uc?export=view&id=1FEDnrXtThbxhDVghh0NGyzGw9wsMyxWI)
<br/>
We can see that, in accordance with [the example above](#the-hooks-class), we do indeed have 
* a doc, here a ChatMessage instance,
* an object containing what has changed (what was updated),
* the options passed to the update call, and
* an Id, that's in this case the user that triggered the update.

### Raw list of hooks as of 0.8.8
<details>
  <summary>click me</summary>

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

# Answering the call : how to register (and unregister) a hook : 
## Registering a hook
It's JavaScript, so there are many ways to implement your hook register. ie inline, inline with an arrow function, by reference, with or without arguments, with a bind(this)...<br/>
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
Most general purpose hooks would usually be in your main module file(though I would advise for some restraint on the inline declarations, but that's besides the point), and more specific ones in the classes that need them.<br/>
However some events are dependent on other (ie a dragDrop on the canvas will only happen after the canvas own init as in [the example above](#on-or-once-)), hence often declaring a Hooks.on inside the callback for another hook.<br/>
Weirdly enough, I once thought I could declare `Hooks.on('renderChatLog', chat.addChatListeners);` inside the 'ready' hook. Well tuns out it doesn't work and has to be declared on the same level as said 'ready' hook.<br/>

### ⚠ the issue with having multiple clients.
Some game events triggered by a user, can generate a hook call for all connected clients. Depending on what you use it for, that can be an unwanted behavior. Hence the need for some checks in your hooks.<br/>
A classic example of this, is the use of the 'actorCreate' hook to populate a list of default items (that's prior 0.8.x and the advent of the #_preCreate() 👍 ).<br/>
```javascript
Hooks.on('createActor', async function (actor, options, userID) {
  //check current user is the one that triggered the création
  //(we wouldn't want to add abilities to the actor multiple times)
  if (userID != game.user.id) { return;}
```

## Removing a hook
Sometimes it can be usefull (and not only good practice) to unregister a hook.<br/>
⚠ As a matter of fact, the 'Hooks._hooks' object keeps a reference to every function instances you registered it with, thus preventing any [garbage collection](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management#references).<br/>
ie : If you've declared a hook in an Application instance, **closing said application will not free up the memory** for there is still a reference to it somewhere in the Hooks._hooks (and the app instance will still respond to it's calls after being closed).<br/>
Hence the need for **proper hook disposal** :  
### removing by index
It's as easy as `Hooks.off('updateActor', myHookIndex);` but it obviously requires having stored the hookIndex on registration.<br/>
One could imagine storing pairs of {hookName, hookIndex} in order to later unregister multiple hooks in a row.<br/>

### removing by function
This way might seem easier (and cleaner, codewise), but in some instances, it can prove challenging. Basic syntax, using previous example would be `Hooks.off('updateActor', this.onUpdateActor);`.<br/>
But, remember how we declared said hook [earlier](#registering-a-hook) using a bind(this) ? Well this is going to make things harder.<br/>
In this instance, `Hooks.off('updateActor', this.onUpdateActor);` will fail to remove the hook, as well as `Hooks.off('updateActor', this.onUpdateActor.bind(this));`.<br/>
A solution to this, courtesy of @Calego is to declare the callback as an arrow function like so :<br/>
```javascript
  onUpdateActor = (actor, data, options, userId) => {
    if ( actor.id !== this.diceThrow.actor.id ) { return ; }
    //...
  }
```
Using the arrow declaration implies a 'this' being in a broader context than just the function itself, in that case, our class instance. This allows for a cleaner declaration : 
```javascript
 export default class DiceDialogue extends Application {
  
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
    this.diceThrow = null;
    
    return super.close(options);
  }
}
```

# Calling : When there's no hook to hold on to
At some point, you might want to create your own calls, be it to enrich the API your module provides or just to facilitate your own coding implementation.<br/>
As and example, I'll leave you with a convenient snippet of my own that adds a callAll() when the user changes it's rollMode using the dropDown list from the chatLog side panel : 
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
    game.settings.settings.get('core.rollMode', rollModeSetting);
});
``` 
