---
title: Old versions of files are being served by Foundry's Server
description: 
published: true
date: 2021-02-18T20:33:48.272Z
tags: 0.8.0
editor: markdown
dateCreated: 2021-02-18T20:13:36.827Z
---

# Summary

After Foundry is loaded for the first time, changes to files, including file name changes, don't seem to be picked up by the Foundry server, even after a full restart.

The old file is (sometimes?) still served, preventing development changes from being reliably reflected

## What changed



## What you need to change



## Research Notes

* F5 doesn't work
* CTRL+F5 doesn't work
* Restarting Foundry doesn't work
* Reinstalling Foundry doesn't work
* Restarting the PC doesn't work
* > I hit ctrl-F5 and I also completely restarted Foundry, but the code browser is still showing the old version. - Skimble
* > Okay, it's finally updated to use the actual version of the file and it's back to no error after ctrl-F5 twice more. I wonder if there's some sort of diff going on. - Skimble