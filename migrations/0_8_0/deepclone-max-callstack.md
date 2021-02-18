---
title: deepClone - Maximum call stack size exceeded
description: 
published: true
date: 2021-02-18T20:32:34.918Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-18T20:06:10.734Z
---

# Summary

> deepClone - Maximum call stack size exceeded
{.is-danger}


When rendering sheets, `deepClone` seems to be called cyclically forever, causing overflow errors.

## What changed



## What you need to change



## Research Notes

* Occurs on character sheet rendering (Skimble#8601), (Bithir#2454)
* Not explictly called, seems to be internal