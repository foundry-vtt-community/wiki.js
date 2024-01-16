---
title: Self-Hosting LiveKit Audio/Video Server on Existing Linux Setup
description: Configure your existing self-hosted Linux FoundryVTT server to also self-host your LiveKit A/V server to use within FoundryVTT
published: true
date: 2024-01-16T22:18:00.531Z
tags: linux, self-hosting, cloud, cloudflare, cloud host, a/v service, cloud hosting
editor: markdown
dateCreated: 2024-01-16T22:18:00.531Z
---

# Overview
This guide will show you how to setup a LiveKit A/V server on your existing Linux server for FoundryVTT. This guide assumes you already have a Linux server self-hosted or cloud-hosted and will NOT go over the setup of a FoundryVTT Linux server. I would like to note that I created this guide because all the other guides I found didn't work for my situation. All guides I have come across for the LiveKit server hosting did serve as a foundation for this one. They will be credited and linked in the [Additional Guides & Credits](#additionaal-guides--credits) section of this guide.

## Requirements
- Linux self or cloud hosted machine
- Nginx Reverse Proxy Service (If you are currently using Caddy as per the Always Free Oracle Cloud Hosting Guide for Foundry, switching to Nginx will be covered in this guide)
- Certbot
- Free Cloudflare DNS Account
- Certbot Cloudflare Plugin
- CNAME or A Records (one or the other, NOT both) for livekit.domain.com and livekit-turn.domain.com.
	- CNAME records should point livekit.domain.com and livekit-turn.domain.com to your Foundry server domain name (e.g. foundry.domain.com)
  - A records should point to the same public IP as your Foundry server (e.g 123.45.6.789)
  
> Note: Cloudflare account and Certbot plugin is only necessary if you are having trouble with your own domain name and the `sudo certbot --nginx` command
{.is-info}

# Install & Setup Required Software
## Nginx
To install Nginx on your device run the install command (depending on your Linux distro):
- **REHL Linux Distros:** `sudo yum install nginx`
- **Debian & Ubuntu:** `sudo apt install nginx`

## Certbot & Certbot Cloudflare Plugins
First you will need to install snapd if it is not pre-installed on your distro (you can check with the `sudo snap --version` command).
- **REHL Linux Distros:** `sudo yum install snapd`
- **Debian & Ubuntu:** `sudo apt install snapd`

Once snapd is installed, you can install Certbot:
```
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

If you have troubles using Certbot on its own and are using your own domain name (not one provided by free services like [DuckDNS](www.duckdns.org)), you will need to create a free Cloudflare account and follow the steps to use their DNS servers vs your registar's servers. Cloudflare has detailed steps and will walk you through the whole process for a number of different registars. This can take up to 24hrs to occur.

To install the Cloudflare Certbot plugin, enter `sudo snap install certbot-dns-cloudflare` or `sudo apt-get install python3-certbot-dns-cloudflare` in the terminal. In order to link the Certbot plugin on your device to your Cloudflare account, you'll need to get an API token:
1. In your Cloudflare page for your domain, go to the Overview tab (found on the side panel). 
2. Scroll down until the API section on the right side of the page. 
3. Click the 'Get your API token' link
4. Click 'Create Token'
5. Scroll to the bottom and click 'Create Custom Token'
6. Fill out the form below
> If you have multiple domains, change the 'All zones' drop down to be 'Specific zone' and select the domain you wish to use to use for Foundry & the LiveKit server. It is recommended for security that you set the TTL to be 6 to 12 months long.
{.is-info}
![screenshot_2024-01-16_160503.png](/images/screenshot_2024-01-16_160503.png)
7. Create a file `/etc/letsencrypt/cloudflare.ini` with the following text (replace YOUR_TOKEN_HERE with your generated API token):
```
# Cloudflare API token for Certbot
dns_cloudflare_api_token = YOUR_TOKEN_HERE
```
> When the API token expires, you will need to update this file with the new token each time.
{.is-info}

# Additional Guides & Credits
There are several guides I have gone through that are extremely helpful. Several of them served as the foundation of this guide and I would like to give credit to them here. Thank you very much for your contributions and help!

**Credits:**
- HiroshiSakamoto: Writing the Ubuntu VM guide below
- @ShadowMorph: Contributing Nginx and SSL configs to the Ubuntu VM guide below
- threz_: Writing the Always Free Oracle Cloud Hosting Guide for Foundry guide below
- bekriebel: Writing the Installing LiveKit on an Existing Self Hosted Foundry Server guide below
- u/Aquatic_Melon, u/Tarilis, u/Visual_Fly_9638: Suggestions and troubleshooting on my original [Reddit thread](https://www.reddit.com/r/FoundryVTT/comments/193pl3b/livekit_av_integration/).

**Guides:**
- [Always Free Oracle Cloud Hosting Guide for Foundry](https://foundryvtt.wiki/en/setup/hosting/always-free-oracle)
- [Installing LiveKit on an Existing Self Hosted Foundry Server](https://github.com/bekriebel/fvtt-module-avclient-livekit/wiki/Installing-LiveKit-on-an-Existing-Self-Hosted-Foundry-Server)
- [Ubuntu VM](https://foundryvtt.wiki/en/setup/hosting/Ubuntu-VM)
- [Cloudflare Certbot Plugin](https://certbot-dns-cloudflare.readthedocs.io/en/stable/)