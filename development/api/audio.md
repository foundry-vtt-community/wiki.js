---
title: Audio
description: Music and other sounds
published: true
date: 2024-08-28T00:04:36.843Z
tags: documentation
editor: markdown
dateCreated: 2024-08-28T00:04:33.276Z
---

![Up to date as of v12](https://img.shields.io/badge/FoundryVTT-v12-informational)

Foundry provides a diverse range of audio options, from global music playlists to environmental sound effects to individual sound file clips.

*Official Documentation*
- [Knowledge Base: Playlists](https://foundryvtt.com/article/playlists/)
- [Knowledge Base: Ambient Sounds](https://foundryvtt.com/article/ambient-sound/)
- [API Docs: Playlist](https://foundryvtt.com/api/classes/client.Playlist.html)
- [API Docs: AmbientSoundDocument](https://foundryvtt.com/api/classes/client.AmbientSoundDocument.html)
- [API Docs: AudioHelper](https://foundryvtt.com/api/classes/foundry.audio.AudioHelper.html)

**Legend**

```
AudioHelper.play // `.` indicates static method or property
AudioHelper#play // `#` indicates instance method or property
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

General [document](/en/development/api/document) and [canvas](/en/development/api/canvas) manipulations are covered on their respective pages.

### Playing a sound

Playing a sound for the current user is done with the singleton AudioHelper class available at `game.audio` with the [play method](https://foundryvtt.com/api/classes/foundry.audio.AudioHelper.html#play). If you want to trigger a sound for *multiple* users, the [*static* play method](https://foundryvtt.com/api/classes/foundry.audio.AudioHelper.html#play-2) that is available at  `foundry.audio.AudioHelper.play` and `game.audio.constructor.play` (Both paths work equally well). Keep in mind that if you're calling these functions within a hook, that the hook may be called on only one client or on all clients and so it may be more appropriate to use the instance or static method.

```js
const src = 'some/audio/path.wav'

// play locally
game.audio.play(src);

// play on all connected clients
game.audio.constructor.play({ src }, true);
```

---
## Specific Use Cases

> Stub
> This section is a stub, you can help by contributing to it.

---
## Troubleshooting
> Stub
> This section is a stub, you can help by contributing to it.