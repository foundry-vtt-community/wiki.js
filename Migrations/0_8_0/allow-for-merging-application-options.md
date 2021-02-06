---
title: Allow for merging Application options when calling Application#render(force, options) to conveniently assign or toggle application options when the interface is re-rendered.
description: 
published: true
date: 2021-02-06T20:14:08.986Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-06T20:10:52.660Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4577

## What changed

You can now do:
```
actor.sheet.render(false, {editable: false}); // sheet re-renders but is now non-editable
actor.sheet.render(false, {editable: true}); // sheet re-renders and is editable again
```

Internally, the Application class no longer passes around options object provided to the render() method, instead the render method updates this.options which is internally referenced by instance methods.

## What you need to change

* [Unverified] Handle any side effects of the passed-in options now updating the Application options

### Research Notes

* This shouldn't be an impact for the average caller - the signature isn't changing, just the behavior