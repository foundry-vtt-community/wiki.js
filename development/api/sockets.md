---
title: Sockets
description: API documentation for the Socket functionality available to packages.
published: true
date: 2021-11-17T14:06:05.915Z
tags: development, api, documentation, docs
editor: markdown
dateCreated: 2021-11-17T14:06:05.915Z
---

# Sockets

![Up to date as of v8](https://img.shields.io/static/v1?label=FoundryVTT&message=v8&color=informational)

## Overview

> [`socketlib`](https://github.com/manuelVo/foundryvtt-socketlib) is a library module which provides useful abstractions for many common Foundry specific socket patterns.
{.is-info}

Foundry Core uses socket.io v4 behind the scenes for its websocket connections between Server and Client. It exposes the active socketio connection directly on `game.socket`, allowing packages to emit and respond to events they create. As such, most of the [socket.io documentation](https://socket.io/docs/v4/) is directly applicable to foundry's usage.

This is useful in cases where a package wants to send information or events to other connected clients directly without piggybacking on some other Document update cycle event.

## Key Concepts

### Prerequisites

Before a package can send and receive socket events, it must request a socket namespace from the server. This is done by putting `socket: true` in the manifest json.

All socket messages from a package must be emitted with the event name `module.{module-name}` or `system.{system-id}` (e.g. `module.my-cool-module`).

### Architectural Notes

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

## API Interactions

### Simple emission of a socket event

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

The arguments of the acknowledgement callback are the same arguments that all other connected clients would get from the broadcast.

## Specific Use Cases

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
    case "ACTION": {
      handleAction(payload);
    }
    default:
      throw new Error('unknown type');
  }
}

socket.on('module.my-module', handleSocketEvent);
```

### Handling the event on the emitter

> [`socketlib`](https://github.com/manuelVo/foundryvtt-socketlib#socketexecuteforeveryone) has a handy abstraction for this pattern.
{.is-info}

#### With the Acknowledgement Callback

<details>
  <summary>This does not appear to work in 0.8.x</summary>

The emitting client does not recieve a broadcast with the event it emits. As a result, it is recommended to use the Acknowledgement callback pattern described above to handle firing an event on the emitting client as well as all other clients.

```js
// called by both the socket listener and emitter's acknowledgement
function handleEvent(arg) {
  console.log(arg);
}

// not triggered when this client does the emit
socket.on('module.my-module', handleEvent);

socket.emit('module.my-module', 'foo', handleEvent);
```
</details>


#### Pretend the Emitter was called

If the Acknowledgement Callback method doesn't work, the expectation is to be able to call whatever method locally at the time of socket emission in addition to calling it in response to a broadcast.


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

### Doing something on one GM client (aka. GM Proxy)

> [`socketlib`](https://github.com/manuelVo/foundryvtt-socketlib#socketexecuteasgm) has a handy abstraction for this pattern. This snippet is derived from its solution.
{.is-info}

This is a common way to get around permission issues when player clients want to interact with Documents they do not typically have permission to modify (e.g. deducting the health of a monster after an attack).

The tricky part here is to ensure that only one active GM is selected arbitrarily based on logic that is consistent on all connected clients.

```js
function handleEvent(arg) {
  if (!game.user.isGM) return;

  // if the logged in user is the active GM with the lowest user id
  const isResponsibleGM = game.users
    .filter(user => user.isGM && user.isActive)
    .some(other => other.data._id < game.user.data._id);

  if (!isRepsponsibleGM) return;

  // do something
  console.log(arg);
}

socket.on('module.my-module', handleEvent);
```

### Doing something on one specific client

> [`socketlib`](https://github.com/manuelVo/foundryvtt-socketlib#socketexecuteasuser) has a handy abstraction for this pattern.
{.is-info}

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

## Troubleshooting

### No socket event is broadcast

Run through this checklist of common issues:
1. Does your manifest include the `socket: true` property mentioned in the [Prerequisites](#Prerequisites)?
2. Have you restarted the world since modifying the manifest JSON?
3. Are you broadcasting with the correct namespace on your event? (Also mentioned in the Prerequisites section)
4. Are you trying to respond to the broadcast from the emitting client? (Emitters do not recieve the broadcast, some strategies above for handling this.)