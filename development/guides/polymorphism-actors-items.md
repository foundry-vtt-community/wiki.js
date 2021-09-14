---
title: Using Polymorphism with Actors and Items in Foundry
description: You cannot use polymorphism for Actors and Items in Foundry. Except you can.
published: true
date: 2021-09-14T01:59:43.308Z
tags: development
editor: markdown
dateCreated: 2021-08-14T19:09:54.861Z
---

# Using Polymorphism with Actors and Items

If you're reading this, it's probably not news to you but Foundry makes it easy to use your own, custom classes for system-specific Actor and Item!

```js
//Somewhere in your initialization code
CONFIG.Actor.documentClass = MySystemActor;
CONFIG.Item.documentClass = MySystemItem;
```

BOOM! Just plug in your own subclasses and you're done. In most cases, that's great, because Actors or Items are all pretty much the same.

Of course there will be differences. For simple cases, checking the `type` of your `Entity` would do the trick. Handle a handful of exceptions and you're all right.

In some cases, however, some kinds of `Actor`s or `Item`s could be very, very different from one another. Some examples?

- In the Cypher RPG system (which also powers Numenera, The Strange, etc.), PCs and NPCs are mechanically VERY different. While PCs have 3 different stats, NPCs have none of them. NPCs also have a target level, something which PCs do not have.
- Minor NPCs in the d00Lite system (Barebones Fantasy, Art of Wuxia, etc.) don't get a full character sheet: they get a title (eg. "Soldier"), a single percentage related to their title and HP. That's it.

There are certainly many more.

In such cases, checking for `type` willy-nilly would make for code that is fairly very hard to read and, especially, to maintain.

Here, clearly, using polymorphism seems to be the key, except that Foundry will not let us specify more than one subclass for `Actor` or `Item`. Is there no solution, I hear you crying?

...why of course there's one because otherwise I wouldn't have written this, would I?

## Target Audience

This is mostly aimed at system developers. I expect you're familiar with polymorphism and ES6, although everything written here could be adapter to good ol' prototype-based Javascript. It could also easily be translated to Typescript.

## Disclaimer

This is obviously working around the (very reasonable) constraints of the base Foundry system - ie. this is a hack. It works - and has been working for me for quite some time - but there is no guarantee it might not break in the future. YMMV, *caveat emptor* and all that.

## Your new best friend: the `Proxy` object

This is not meant as a tutorial on the `Proxy` object: check out the MDN docs: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy

But, in a nutshell, proxies wrap another object and can intercept pretty much any call or operator action made on a object. It has a ton of uses and is easily abused.

So the plan, here, is to use a `Proxy` to intercept any call that would make Foundry stop and yell at you for doing something you should totally NOT be doing.

## How to use it?

Here are the basic steps.

### Create the subclasses you need

**In the following example code, I'll focus on a subclass for PC and NPC `Actor`s, but the same principles would apply to `Item`s.**

First, as the title says, create any and all `Actor` subclasses you need. 

```js
//Should be in different files but let's keep this simple

class MySystemPCActor extends Actor { }

class MySystemNPCActor extends Actor { }
```

Notice they both simply inherit from `Actor`.

Let's leave them as simple class declarations for now. Later on you'll be able to add any and all methods or properties you'll need.

### Create a `Proxy` for them

This is where most of the magic will happen.

Basically, we need to create a `Proxy` that will act as a fake `Actor` or `Item` subclass but will actually intercept some of the calls made by Foundry to make it behave like a regular class. It will then act as a kind of factory.

The following needs to be overridden:

- the `new` operator
- calls to the `create` and `createDocuments` methods
- uses of the `instanceof` operator

... yes, that's it! It's surprisingly easy. Here's what the outline of that code could look like:

```js
import { MySystemNPCActor } from "./MySystemNPCActor.js";
import { MySystemPCActor } from "./MySystemPCActor.js";

//Provide a type string to class object mapping to keep our code clean
const actorMappings = {
  npc: MySystemNPCActor,
  pc: MySystemPCActor,
};

export const MySystemActorProxy = new Proxy(function () {}, {
  //Will intercept calls to the "new" operator
  construct: function (target, args) {
    const [data] = args;

    //Handle missing mapping entries
    if (!actorMappings.hasOwnProperty(data.type))
      throw new Error("Unsupported Entity type for create(): " + data.type);

    //Return the appropriate, actual object from the right class
    return new actorMappings[data.type](...args);
  },
  
  //Property access on this weird, dirty proxy object
  get: function (target, prop, receiver) {  
    switch (prop) {
      case "create":
      case "createDocuments":
        //Calling the class' create() static function
        return function (data, options) {
          if (data.constructor === Array) {
            //Array of data, this happens when creating Actors imported from a compendium
            return data.map(i => NumeneraActor.create(i, options));
          }

          if (!actorMappings.hasOwnProperty(data.type))
            throw new Error("Unsupported Entity type for create(): " + data.type);

          return actorMappings[data.type].create(data, options);
        };
        
      case Symbol.hasInstance:
        //Applying the "instanceof" operator on the instance object
        return function (instance) {
          return Object.values(actorMappings).some(i => instance instanceof i);
        };

      default:
        //Just forward any requested properties to the base Actor class
        return Actor[prop];
    }
  },
});
```

And that's it. The `default` inside the `get` switch really is the key to "fake" polymorphism: any call to any other property is sent to the base class. `Error`s are thrown if any unknown behavior is detected.

### The final step: set your `Proxy` as your `documentClass`

Come back to your initialization code and set your newly created `Proxy` object as the "class" to use:

```js
CONFIG.Actor.documentClass = MySystemActorProxy;
```

... aaaaand you're done. You may now cackle gleefully for having fooled Foundry.

Afterwards, anytime Foundry needs to create an `Actor` it will call `new CONFIG.Actor.documentClass(...)` which gets intercepted by your `Proxy`: in turn, it creates the right object according to your class mapping and that object is returned transparently.

You can now come back to your `MySystemPCActor` and `MySystemNPCActor` classes and add whatever features you need. The ~~world~~ `Actor` or `Item` is now your oyster.