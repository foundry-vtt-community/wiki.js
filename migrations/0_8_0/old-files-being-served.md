---
title: Old versions of files are being served by Foundry's Server
description: 
published: true
date: 2021-02-18T20:46:27.771Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-18T20:13:36.827Z
---

# Summary

After Foundry is loaded for the first time, changes to files, including file name changes, don't seem to be picked up by the Foundry server, even after a full restart.

The old file is (sometimes?) still served, preventing development changes from being reliably reflected

## What changed

The server now sets an explicit max-age for static files

## What you need to change

During development enable the "Disable cache" setting in dev tools

![](https://media.discordapp.net/attachments/808018896980410378/812060860758360064/unknown.png)

