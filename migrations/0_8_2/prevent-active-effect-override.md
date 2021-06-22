---
title: Prevent submitting overridden values via an ActorSheet submission by removing fields which currently have an active effect override from the submitted data structure
description: 
published: true
date: 2021-05-01T02:59:57.004Z
tags: 
editor: markdown
dateCreated: 2021-05-01T02:59:52.894Z
---

# Summary
https://gitlab.com/foundrynet/foundryvtt/-/issues/4934

## What changed

ActorSheet will by default prevent submitting updates for fields currently modified by an active effect.

## What you need to change

You may need to provide more sophisticated logic in `ActorSheet#_getSubmitData`.

* [Unverified] 

### Research Notes

*