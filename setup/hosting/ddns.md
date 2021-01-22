---
title: Using Dynamic DNS
description: 
published: true
date: 2021-01-22T03:18:50.280Z
tags: hosting, dns, ddns, networking
editor: markdown
dateCreated: 2021-01-22T03:18:50.280Z
---

Dynamic DNS provides a fantastic way to circumvent the static-IP address restrictions that come from using a residential IP address configuration. Below are some steps for differing hosting providers that can be used to provide simple configurable subdomain urls.

# DuckDNS
[Duck DNS](/en/https://duckdns.org) is a free service which will point a Domain Name (specifically, sub domains of duckdns.org) to an IP of your choice. It supports updates through a variety of cross-platform clients and is extremely user-friendly to maintain.

The steps to configure DuckDNS for automatic updates vary based on which OS you are using.

## DuckDNS for Windows 10
1. [Download and install the DuckDNS Update Client for Windows](http://www.etx.ca/products/windows-applications/duckdns-update-client/)
1. log in to https://duckdns.org using whichever sign in method you prefer (such as Google or Github)
1. On the DuckDNS domains page, register a subdomain address with a name of your choosing
1. Open the **DuckDNS Update Client** from the Windows start menu 
1. Right click the Duck in your system tray and choose 'DuckDNS settings'
1. Using the information shown on your https://duckdns.org/domains page, fill out the DuckDNS settings page.

## DuckDNS for macOS
1. Download and install IP Monitor from the App Store: 
1. https://itunes.apple.com/us/app/ip-monitor/id1050307950?mt=8
1. log in to https://duckdns.org using whichever sign in method you prefer (such as Google or Github)
1. On the DuckDNS domains page, register a subdomain address with a name of your choosing
1. Open **IP Monitor** and choose to edit DuckDNS - AAAA from the 'update the following records' section 
1. Enter your Token from your DuckDNS domain page in the 'password/client key' section and choose A:IPV4
1. Click update and then save

## DuckDNS for Linux or Other Operating Systems
1. Head over to [The DuckDNS Installation Page](https://www.duckdns.org/install.jsp) and follow the installation instructions for whichever method you wish to use to auto-update your DNS entries.
1. log in to https://duckdns.org using whichever sign in method you prefer (such as Google or Github)
1. On the DuckDNS domains page, register a subdomain address with a name of your choosing
1. Open the configuration system for the DuckDNS update client of your choosing.
1. Use the information shown on your https://duckdns.org/domains page to complete the configuration of the method you chose.

