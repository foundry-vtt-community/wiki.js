---
title: Linux Installation Guide
description: Sets up Foundry on linux with Caddy as reverse proxy. 
published: false
date: 2021-05-06T00:04:32.532Z
tags: 
editor: markdown
dateCreated: 2021-05-05T21:54:44.555Z
---

# General Linux Installation and Usage Guide

# A. Overview
## Objective

This guide provides easy to follow steps for installing and using Foundry as a headless server in a linux environment. It is written to account for a number of linux common linux distributions and installation scenarios. 

At the end of this guide you will have:

* Foundry server running 24/7 including automatic restarts
* (Optional) A domain name pointing to the server with HTTPS
* (Optional) Cyberduck, a file transfer program, configured to manage user data of the Foundry server

## Important Information and Requirements

This guide assumes that you have linux already installed on a server of some sort, either a PC on a local network or a VPS/VM in the cloud without a webserver or Foundry already installed an running. 

The following is required to complete this guide:

1. A basic understanding of using a terminal that includes the ssh utility, such as:
a. Powershell in Windows.
b. Terminal in Linux or MacOS. 
2. Valid domain name, such as:
a. A purchased domain name from a registrar like [namecheap](https://namecheap.com) or [gandi.net](https://gandi.net).
b. A free subdomain from a free domain name service like [Duck DNS](http://duckdns.org).


## Preferred Linux Distribution

This guide includes instructions for a number of linux distributions, as described below. If you do not have a strong reason for another distribution, we recommend **Ubuntu 20.04**. 

This guide supports the following distributions:

1. Debian-based, such as (but not limited to):
a. Debian
b. Ubuntu
c. Raspberry Pi OS
2. CentOS-based, such as (but not limited to):
a. CentOS
b. Red Hat Linux
c. Amazon Linux

Any distrition that uses the `apt` or `yum` package managers *should* be compatible with this guide. Any differentiation in instructions for the distributions will be clearly indicated where necessary. 

# B. User and General System Setup
## Objective

At the end of this section, you will have a non-root user setup to run Foundry as well as the necessary software to run Foundry managed behind a reverse proxy. 

>This section assumes that you are connected via terminal to your linux server. This can be either throught direct local access to a terminal or through ssh. {.is-info}

>How to connect to your server is out of scope for this guide. You should have terminal access if you installed linux on a local pc, or if you are using a cloud VPS/VM provider then review their documentation for how to get `ssh` or `terminal` access to your instance. {.is-info}

## User Setup

We must use a non-root user that is part of the `sudoers` group to properly continue and manage Foundry. 

>If you are certain that you have a non-root user that has access to `sudo` then you may skip the next steps and continue to [System Setup](#system-setup). 
{.is-warning}

<a name="B1" href="#B1">B1.</a> Firstly, check your terminal to determine which user you are logged in as. Your terminal should look like:

```
<user>@<servername>:_   
```

<a name="B2" href="#B2">B2.</a> The `<user>` field will show you which user you are connected as. If it shows `root` then we need to create a new user and add them to sudoers. If the user is **NOT** `root` then you can skip and continue to step [B4](#B4).

<a name="B3" href="#B3">B3.</a> Click the heading for your linux distribution to expand the commands to create a new user. We will create a user named `foundry` and add them to sudoers. Choose a strong password for the user that you will remember.

You can leave all other fields blank or fill with whatever info you'd like.


<details><summary>Ubuntu/Debian/Raspberry Pi OS</summary>
  
```
adduser foundry
usermod -aG sudo foundry

```
</details>

<details><summary>CentOS/Red Hat/Amazon Linux</summary>
  
```
adduser foundry
usermod -aG wheel foundry

```
</details>

<a name="B4" href="#B4">B4.</a> Assume the new user by:

```
su - foundry
```
You should now see the terminal look like:
```
foundry@<servername>:_
```

