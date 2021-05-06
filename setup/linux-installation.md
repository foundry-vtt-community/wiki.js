---
title: Linux Installation Guide
description: Sets up Foundry on linux with Caddy as reverse proxy. 
published: false
date: 2021-05-06T00:50:16.973Z
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

1. Debian 11 based, such as (but not limited to):
a. Debian
b. Ubuntu
c. Raspberry Pi OS
2. CentOS 8 based, such as (but not limited to):
a. CentOS
b. Red Hat Linux
c. Fedora

Any distrition that uses the `apt` or `yum` package managers *should* be compatible with this guide. Any differentiation in instructions for the distributions will be clearly indicated where necessary. 

>This guide requires Debian 11 or CentOS 8 based distributions or higher. Using lower versions may not function properly. {.is-info}

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

<a name="B2" href="#B2">B2.</a> The `<user>` field will show you which user you are connected as. If it shows `root` then we need to create a new user and add them to sudoers. If the user is **NOT** `root` then you can skip and continue to step [B5](#system-setup).

<a name="B3" href="#B3">B3.</a> Click the heading for your linux distribution to expand the commands to create a new user. We will create a user named `foundry` and add them to sudoers. Choose a strong password for the user that you will remember.

You can leave all other fields blank or fill with whatever info you'd like.


<details><summary>Ubuntu/Debian/Raspberry Pi OS</summary>
  
```
adduser foundry
usermod -aG sudo foundry

```
</details>

<details><summary>CentOS/Red Hat/Fedora</summary>
  
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


## System Setup
We will now install the necessary software to run and manage Foundry behind a reverse proxy. This includes:

* nodejs 14+, Required to run Foundry itself
* caddy 2+, the webserver that will be used as a reverse proxy
* pm2, the process manager that will keep Foundry running
* unzip, the utility used to decompress the Foundry installation zip archive
* nano, the text editor used to edit configuration files

>To continue, you must be using a non-root user with `sudo` access. If that is not the case, please review the steps in [User Setup](#user-setup). {.is-info} 

<a name="B5" href="#B5">B5.</a> First, let's update the system to make sure we have everything as up-to-date as possible:

<details><summary>Ubuntu/Debian/Raspberry Pi OS</summary>
  
```
sudo apt update
sudo apt upgrade -y

```
</details>

<details><summary>CentOS/Red Hat/Fedora</summary>
  
```
sudo dnf update -y

```
</details>

>You may be asked for a password the first time you use a `sudo` command. This is normal. Enter the password for the user. {.is-info}

>If after entering the correct password, you receive an error: `<user> is not in the sudoers file` or similar, then you must login as **root** and complete ther [User Setup](#user-setup). {.is-warning}

<a name="B6" href="#B6">B6.</a> Add the nodejs 14 repository to the system package manager:

<details><summary>Ubuntu/Debian/Raspberry Pi OS</summary>
  
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
```
</details>

<details><summary>CentOS/Red Hat/Fedora</summary>
  
```
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
```
</details>

<a name="B7" href="#B7">B7.</a> Add the caddy repository to the system package manager:

<details><summary>Ubuntu/Debian/Raspberry Pi OS</summary>
  
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo apt-key add -
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee -a /etc/apt/sources.list.d/caddy-stable.list
```
</details>

<details><summary>CentOS/Red Hat/Fedora</summary>
  
```
dnf install 'dnf-command(copr)'
dnf copr enable @caddy/caddy
```
</details>

<a name="B8" href="#B8">B8.</a> Install nodejs, caddy, unzip, and nano:

<details><summary>Ubuntu/Debian/Raspberry Pi OS</summary>
  
```
sudo apt update
sudo apt install nodejs caddy unzip nano -y
```
</details>

<details><summary>CentOS/Red Hat/Fedora</summary>
  
```
dnf install nodejs caddy unzip nano -y
```
</details>

<a name="B9" href="#B9">B9.</a> Check that nodejs and npm are installed and the correct versions:

```
node --version
npm --version
```
Node should return a version of 14 or greater. The npm version doesn't matter, but should return something. 

<a name="B10" href="#B10">B10.</a> Install pm2:

```
sudo npm install pm2 -g
```
<a name="B11" href="#B11">B11.</a> Add pm2 to startup as the current user. Be sure to carefully read the `blue notice` and follow all instructions given:

```
pm2 startup
```
>***Continued*** 
>You will need to carefully review the output of the `pm2 startup` command. It will include a specific instruction on how to enable pm2 startup on your particular distribution. Copy and paste this command exactly. {.is-info}

# C. Foundry and Reverse Proxy Setup




