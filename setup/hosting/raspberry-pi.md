---
title: Raspberry Pi
description: how to install Foundry VTT on a Raspberry PI 
published: true
date: 2021-04-18T20:02:58.536Z
tags: hosting, self-hosting, raspberry pi
editor: markdown
dateCreated: 2021-04-18T20:02:58.536Z
---




Raspberry Pi OS (previous called Raspbian) it's a Debian based image for Raspberry Pi.

To install your Raspberry Pi OS follow the official documentation [here]().

## Requirements

### Installing node.js and it's Package Manager


This guide is for debian-based Raspberry Pi, if you are using other distribution, the comands may change. (Feel free to add your favorite flavor system here too)

```bash 
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```

This installs both nodejs and the node package manager npm. Check both with `node --version` and `npm --version`, at this time of writing node.js is at version `v14.16.1` and npm at version `6.14.12`

