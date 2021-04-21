---
title: Redesign the game.audio.play utility method to resolve issues with incorrect howler syntax and to migrate to the new Web Audio SoundNode implementation.
description: 
published: true
date: 2021-04-21T15:10:12.264Z
tags: 
editor: markdown
dateCreated: 2021-04-14T12:55:26.524Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/3997

## What changed
The game.audio.play(src, options) method has had it's signature changed:

```js
  /**
   * Play a single Sound by providing its source.
   * @param {string} src            The file path to the audio source being played
   * @param {object} options]       Additional options passed to SoundNode#play
   * @returns {Promise<SoundNode>}  The created SoundNode which is now playing
   */
  async play(src, options) {
    const sound = new SoundNode(src);
    await sound.load();
    sound.play(options);
    return sound;
  }
```

- The method is now async and will return a Promise<SoundNode> when the sound is fully loaded and begins playback
- The method now accepts an object of options which are forwarded to the SoundNode#play method to customize playback



## What you need to change

* [Unverified] reconcile any previous code around game.audio.play with the new signature

### Research Notes

* 