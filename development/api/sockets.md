---
title: Sockets
description: API documentation for the Socket functionality available to packages.
published: true
date: 2025-05-20T15:14:23.837Z
tags: development, api, documentation, docs
editor: markdown
dateCreated: 2021-11-17T14:06:05.915Z
---

# Sockets

![Up to date as of v13](https://img.shields.io/static/v1?label=FoundryVTT&message=v13&color=informational)

Sockets provide a way for different clients connected to the same server to communicate with each other. This page covers both directly using `game.socket` as well as the v13 feature of interacting via registering queries.

*Official Documentation*

- [Socket](https://socket.io/docs/v4/client-api/#socket)
- [SocketInterface](https://foundryvtt.com/api/classes/client.SocketInterface.html)

**Legend**

```js
SocketInterface.dispatch // `.` indicates static method or property
Socket#on // `#` indicates instance method or property
```

## Overview

Foundry Core uses socket.io v4 behind the scenes for its websocket connections between Server and Client. It exposes the active socket.io connection directly on `game.socket`, allowing packages to emit and respond to events they create. As such, most of the [socket.io documentation](https://socket.io/docs/v4/) is directly applicable to foundry's usage.

Alternatively, one can register functions in `CONFIG.queries`, providing predefined handlers for inter-client communication. Foundry includes two queries by default; `dialog` and `confirmTeleportToken`; the former of these is especially versatile and obviates many previous needs for sockets.

This is useful in cases where a package wants to send information or events to other connected clients directly without piggybacking on some other [Document operation](/en/development/api/document#event-cycles), such as creating a chat message or an update to an item.

---

## Key Concepts

For the purposes of this article, using `game.socket` will be referred to as *direct sockets*. Registering functions in `CONFIG.queries` will instead be reffered to as the *query system*.

#### Socket Data

Socket data *must* be JSON serializable; that is to say, it must consist only of [values valid in a JSON file](https://www.json.org/json-en.html). Complex data structures such as a Data Model instance or even Sets must be transformed back to simpler forms; also keep in mind that if possible you should keep the data in the transfer as minimal as possible. If you need to reference a document, just send the UUID rather than all of its data, as the other client can just fetch from the UUID.

### Direct Sockets vs. Queries

By default, direct sockets are emitted to *every* client, and it is the responsibility of the handler to perform any necessary filtering. By contrast, queries are always targeted, from one user to another, and so any mass-message system will need to call query each user separately. The upside is that each of those queries is its own promise that is fully and properly awaited for response by the queried user, unlike direct sockets which only returns a promise that confirms receipt by the *server*. 

It's also worth noting that direct sockets do not have any built-in permission controls, while queries have the `QUERY_USER` permission which is available to all players by default but can be taken away by stricter GMs.

---

## API Interactions

These are common ways to interact with the Foundry socket framework.

### Direct Socket Prerequisities

Before a package can directly send and receive socket events, it must request a socket namespace from the server. This is done by putting `"socket": true` in the manifest json.

All socket messages from a package must be emitted with the event name `module.{module-name}` or `system.{system-id}` (e.g. `module.my-cool-module`).

### Query Registration

Registering a query is as simple as adding a new entry to `CONFIG.queries`. The key should be prefixed by your package ID, e.g. `my-module.someEvent`. Your function must return JSON-serializable data, as whatever it returns will be passed back to the querying client.

```js
async someEventHandler(queryData, {timeout}) {
	// do stuff
  return jsonData;
}

CONFIG.queries["my-module.someEvent"] = someEventHandler;
```

The first argument, queryData, is whatever JSON-serializable info you want to provide to the queried client. The second argument, `queryOptions`, currently *only* may have `timeout` information. It is destructured in every usage, so you can't use it to pass any futher arbitrary options; the correct spot for community developers to add more info is into that `queryData` object.

### Using Queries

Invoking a query involves using the `User#query` method.

```js
// Your business logic will determine how to pick the user to query
// The activeGM is a common choice for delegating permission-intensive actions
const user = game.users.activeGM;

// Your business logic will also dictate what you need to include in the payload
// to deliver to the other client
const queryData = { foo: "bar" };

// timeout is optional and is in *milliseconds*. 
// Inline multiplication is an easy way to make sure your intended duration is more readable.
const queryValue = await user.query("my-module.someEvent", queryData, { timeout: 30 * 1000 });

// Now queryValue will be the return of whatever function you ran, if relevant.
```

### Simple emission of a direct socket event

Note that this socket event does not get broadcast to the emitting client.

```js
socket.emit('module.my-module', 'foo', 'bar', 'bat');
```

### Listen to a socket event

All connected clients other than the emitting client will get an event broadcast of the same name with the arguments from the emission.

```js
socket.on('module.my-module', (arg1, arg2, arg3) => {
  console.log(arg1, arg2, arg3); // expected: "foo bar bat"
})
```

### Promise wrapped emission of a socket event

It can be useful to know when a socket event was processed by the server. This can be accomplished by wrapping the `emit` call in a Promise which is resolved by the [acknowledgement](https://socket.io/docs/v4/emitting-events/#acknowledgements) callback.

```js
new Promise(resolve => {
  // This is the acknowledgement callback
  const ackCb = response => {
    resolve(response);
  };

  socket.emit('module.my-module', arguments, ackCb);
});
```

The arguments of the acknowledgement callback are the same arguments that all other connected clients would get from the broadcast. Note that this is *not* the same as being able to fully `await` any actions taken on the other clients - you would need a *second* socket event, sent by the other clients, to handle that.

## Specific Use Cases

Here are some helpful tips and tricks when working with sockets.

### Handling many kinds of events from one package

Since packages are only allotted one event name, using an pattern which employs an object as the socket event with a `type` and `payload` format can help overcome this limitation.

```js
socket.emit('module.my-module', {
  type: 'ACTION',
  payload: 'Foo'
});
```

```js
function handleAction(arg) {
  console.log(arg); // expected 'Foo'
}

function handleSocketEvent({ type, payload }) {
  switch (type) {
    case "ACTION":
      handleAction(payload);
      break;
    default:
      throw new Error('unknown type');
  }
}

socket.on('module.my-module', handleSocketEvent);
```

#### Using a helper class

You can encapsulate this strategy by defining a helper class; the following example is inspired by [SwadeSocketHandler](https://gitlab.com/peginc/swade/-/blob/develop/src/module/SwadeSocketHandler.ts):

```js
class MyPackageSocketHandler {
	constructor() {
  	this.identifier = "module.my-module" // whatever event name is correct for your package
    this.registerSocketListeners()
  }
  
  registerSocketHandlers() {
  	game.socket.on(this.identifier, ({ type, payload }) => {
    	switch (type) {
        case "ACTION":
          this.#handleAction(payload);
          break;
        default:
          throw new Error('unknown type');
      }
    }
  }
                   
  emit(type, payload) {
    return game.socket.emit(this.identifier, { type, payload })
  }
                   
  #handleAction(arg) {
    console.log(arg);
  }
}
```

This helper class is then instantiated as part of the `init` hook:

```js
Hooks.once("init", () => {
  const myPackage = game.modules.get("my-module") // or just game.system if you're a system
  myPackage.socketHandler = new MyPackageSocketHandler()
});

// Emitting events works like this
game.modules.get("myPackage").socketHandler.emit("ACTION", "foo")
```

#### Pretend the Emitter was called

The expectation is to be able to call whatever method locally at the time of socket emission in addition to calling it in response to a broadcast.


```js
// called by both the socket listener and emitter
function handleEvent(arg) {
  console.log(arg);
}

// not triggered when this client does the emit
socket.on('module.my-module', handleEvent);


function emitEventToAll() {
  const arg = 'foo';
  
  socket.emit('module.my-module', arg);

  handleEvent(arg);
}
```

**Socket#emitWithAck:** This method, despite being available as of v11, does not appear to be useful in the context of Foundry because the server acts as a middle-man for all socket events.

#### Handling the event on the emitter

> [`socketlib`](https://github.com/manuelVo/foundryvtt-socketlib#socketexecuteforeveryone) has a handy abstraction for this pattern.
{.is-info}

### Doing something on one GM client (aka. GM Proxy)

This is a common way to get around permission issues when player clients want to interact with Documents they do not typically have permission to modify (e.g. deducting the health of a monster after an attack).

```js
function handleEvent(arg) {
  if (game.user !== game.users.activeGM) return;

  // do something
  console.log(arg);
}

socket.on('module.my-module', handleEvent);
```

> [`socketlib`](https://github.com/manuelVo/foundryvtt-socketlib#socketexecuteasgm) has a handy abstraction for this pattern. This snippet is derived from its solution.
{.is-info}

### Doing something on one specific client

Some applications require a specific user to be targeted. This cannot be accomplished by the `emit` call and instead must happen in the handler.

**Emitter:**
```js
socket.emit('module.my-module', {
  targetUserId: 'some-user-id',
  payload: "Foo"
})
```

**Socket Handler:**
```js
function handleEvent({ targetUserId, payload }) {
  if (!!targetUserId && game.userId !== targetUserId) return;

  // do something
  console.log(payload);
}

socket.on('module.my-module', handleEvent);
```

> [`socketlib`](https://github.com/manuelVo/foundryvtt-socketlib#socketexecuteasuser) has a handy abstraction for this pattern.
{.is-info}

---

## Troubleshooting

### No socket event is broadcast

Run through this checklist of common issues:
1. Does your manifest include the `"socket": true` property mentioned in the [Prerequisites](#prerequisites)?
2. Have you restarted the world since modifying the manifest JSON?
3. Are you broadcasting with the correct namespace on your event? (Also mentioned in the Prerequisites section)
4. Are you trying to respond to the broadcast from the emitting client? (Emitters do not recieve the broadcast, some strategies above for handling this.)


## Architectural Notes

> The following information comes directly from Atropos on the Mothership Discord's `dev-support` channel. [*^[Link]^*](https://discord.com/channels/170995199584108546/811676497965613117/903652184003055677)
{.is-info}

> We use a pattern for the socket workflow that differentiates between the initial requester who receives an acknowledgement and all other connected clients who receive a broadcast.
> 
> This differentiation allows us to have handling on the initial requester side that can enclose the entire transaction into a single Promise. The basic pattern looks like this:
> 
> On the Server Side
> ```js
> socket.on(eventName, (request, ack) => {
>   const response = doSomethingWithRequest(request) // Construct an object to send as a response
>   ack(response); // This acknowledges completion of the task, sent back to the requesting client
>   socket.broadcast.emit(eventName, response);
> });
> ```
> 
> For the Requesting Client
> ```js
> new Promise(resolve => {
>   socket.emit(eventName, request, response => {
>     doSomethingWithResponse(response); // This is the acknowledgement function
>     resolve(response); // We can resolve the entire operation once acknowledged
>   });
> });
> ```
> 
> For all other Clients
> ```js
> socket.on(eventName, response => {
>   doSomethingWithResponse(response);  // Other clients only react to the broadcast
> });
> ```
> 
> Note in my example that both the requesting client and all other clients both `doSomethingWithResponse(response)`, but for the requesting client that work happens inside the acknowledgement which allows the entire transaction to be encapsulated inside a single Promise. 
> 