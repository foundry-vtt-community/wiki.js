---
title: Linux Installation Guide
description: Sets up Foundry on linux with Caddy as reverse proxy. 
published: false
date: 2021-05-05T23:16:12.802Z
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




<details>
  <summary>
Some description
  </summary>
  

# Lots of content

```
Your content here
```

</details>




