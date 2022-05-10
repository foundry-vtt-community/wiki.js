---
title: Local play
description: 
published: true
date: 2022-05-10T03:19:41.787Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:34:46.297Z
---

# Local Play
![Up to date as of 09/05/2022](https://img.shields.io/static/v1?label=Last%20Updated&message=09/05/2022&color=informational)

## Dual screens

<small>Original source (French): http://www.lahiette.com/leratierbretonnien/foundryvtt-pour-table-reelle</small>

Open your gamemaster account in your browser normally and then launch an Incognito/private window for your players to log in. Move the private window on to a screen facing the players and press F11 for fullscreen!

You can use the "Hot Seat Observer" module to allow all tokens to be controlled by that single player account. See the instructions below to add a second cursor and/or keyboard for this account.

## Dual computers

Another option for local play is to have multiple computers, possibly even one per player.

If you only have two computers, one of them will be dedicated to the GM, and the other will be to show the scenes to the players. To do this, create a new player account with the "Observer" ownership over all player character actors.

## Multiple pointers/keyboards (Windows/Linux)

<small>Original source (French): http://www.lahiette.com/leratierbretonnien/foundryvtt-pour-table-reelle</small>

You can use this to give them control over an independant pointer (or even keyboard) to control a common player account.

### Linux

<small>Source: https://wiki.archlinux.org/index.php/Multi-pointer_X</small>

* Create a second pointer with `xinput create-master aux`. This will create a new mouse pointer called "aux".
* Connect a second mouse (USB preferably) to your computer. You may also connect a second keyboard.
* List the available input devices with `xinput list`
* Find the ID of the new mouse/keyboard, and the ID of the "aux" pointer/keyboard
* Enter the command `xinput reattach <mouse-id> <aux-pointer-id>` to attach the pointer and `xinput reattach <keyboard-id> <aux-keyboard-id>` to attach the keyboard.
* Give the second mouse/keyboard to your players

## Windows

Install [MouseMux](https://mousemux.com/) to manage multiple cursors on the same Windows computer,

## Using a VM

<small>Original source (French): http://www.lahiette.com/leratierbretonnien/foundryvtt-pour-table-reelle</small>

> This section is deprecated! Use the solutions above instead.
{.is-warning}


This solution is using a VM (Virtual Machine) to handle the player session, hence it works with Linux, Windows and probably Mac.

Hardware setting : 

* One laptop, used by the GM
* One screen, connected to the laptop, and oriented to the players

Software setting :

* One VM hypervisor of your choice (VMWare, KVM, Proxmox, Virtualbox). Note that VMware should be prefered, as the sharing of the GPU is more easy to setup with this tool.
* On VM, running in the hypervisor, with Chrome installed. I selected Lubuntu 20.04, because its light and fast, but you can choose the OS of your choice, of course.
* Inside the Guest OS, attach a USB mouse (ie detaching from host), in order for the players to navigate in the scenes/maps on their own.

Running :

* Launch Foundry and connect to it with your browser (laptop GM side)
* Launch the guest OS in the VM
* In the guest OS, launch Chrome and connect to Foundry using the observer player created previously
* Setup fullscreen and provide the VM attached mouse to the players 
