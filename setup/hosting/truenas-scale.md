---
title: TrueNAS SCALE
description: Deploying Foundry on TrueNAS SCALE k3s
published: true
date: 2024-06-01T16:03:02.950Z
tags: 
editor: markdown
dateCreated: 2023-11-26T13:13:16.296Z
---

# Overview

TrueNAS SCALE runs Linux and k3s - a version of Kubernetes - under the hood. It offers a GUI and deploying Foundry is relatively simple.

These instructions assume that you are at least somewhat-familar with TrueNAS and ZFS. Start there, make sure you have TrueNAS SCALE installed and one ZFS pool configured. Tested on SCALE 24.04.1.1 in June 2024.

- Create two ZFS datasets, one for the Foundry app, one for its data
- Download and unzip Foundry node.js code
- Deploy a custom Node app that runs FoundryVTT
- If desired, configure TLS and CloudFlare domain
- If desired, configure DDNS for CloudFlare domain

## Create datasets

In the UI under Datasets, create two datasets. For example `apps/foundry-app` and `apps/foundry-data`, under the pool name. Do not place it under `ix-applications`, that is a system-managed dataset.

Preset should be `Generic` or `Apps` for `foundry-app`. You can use `SMB` for `foundry-data`.

## Download and unzip Foundry node.js code

Go to the official Foundry site, log in, and under Purchased Licenses download for Operating System `Linux/NodeJS`. This gives you a ZIP file.

On TrueNAS SCALE, enable the SSH service under System Settings -> Services. If you aren't using SSH keys, edit it and configure it to `Allow password authentication`.

Using either WinSCP or scp directly, copy the ZIP file to the app dataset you created, e.g. `/mnt/POOLNAME/apps/foundry-app`. If using scp from PowerShell this is `scp FoundryVTT-version.zip root@IP-of-TrueNAS:/mnt/POOLNAME/apps/foundry-app/`.

Using either PuTTY or ssh directly, log into your TrueNAS server. If using ssh from PowerShell this is `ssh root@IP-of-TrueNAS`. Once in, `cd /mnt/POOLNAME/apps/foundry-app`, and `unzip FoundryVTT-version.zip`.

That was the hardest part, particularly if you were not familiar with scp/ssh yet.

## Deploy a custom Node app that runs FoundryVTT

In the TrueNAS UI, go to Apps, click on Discover Apps in the top right and then Custom App in the top right.

> TrueNAS Scale 24.04 does not have a way to set the hostname in the UI. To work around this, give the container Privileged Mode and use the hostname command, see below. It's not pretty, but it keeps the license across restarts.

Any settings I do not mention stay at default, which is most of them.

- Give it an `Application Name`, e.g. `foundry`
- `Image repository` is `node`
- `Image Tag` is `20`
- `Image Pull Policy` is `Always`
- `Container Cmd`, hit the Add button 1 time, and enter:
  - `/bin/sh`
- `Container Args`, hit the Add button 2 times, and enter:
	- `-c`
  - `hostname foundry && node /app/resources/app/main.js --port=30000 --headless --dataPath=/data`
- `Port Forwarding`, hit the Add button, and enter:
	- `Container Port` to `30000`
  - `Node Port` to `30000`
- `Storage`, under `Host Path Volumes` hit Add button twice, and enter:
  - `Host Path` 1st entry, navigate to `/mnt/POOLNAME/apps/foundry-data`
  - `Mount Path` 1st entry, set to `/data`
  - `Host Path` 2nd entry, navigate to `/mnt/POOLNAME/apps/foundry-app`
  - `Mount Path` 2nd entry, set to `/app`
- `Workload Details`
  - Check `Privileged Mode`
- `Portal Configuration`, set:
  - Check `Enable WebUI Portal`
  - `Portal Name` to `Foundry`
  - `Protocol for Portal` leave at `HTTP Protocol` for now
  - Check `Use Node IP for Portal IP/Domain`
  - `Port` to `30000`

And that's it, deploy your app!

If everything worked, the app should eventually show `Running`, and you can see its logs under `Workloads`. If there are issues, the logs will show you what they are.

You can now connect to Foundry at `http://IP-of-TrueNAS:30000` and give it a license.

## Remote access via https and CloudFlare

You can of course at this point forward `30000` at your router and have your users connect to that. But how cool would it be if they could use a domain name, and TLS worked for video/voice chat?

- Create a login at CloudFlare
- Register a domain name with them
- Under the domain name, DNS, click Add Record and add an `A` record with your external (public) IP. Make this Proxied, which is the default.
- Click on SSL/TLS on the left, then Origin Server. Under Origin Certificate click Create Certificate. This gives you contents for a pem and a key file. 
- SSH into your TrueNAS server as before, `cd /mnt/POOLNAME/apps/foundry-data/Config`, and first `nano cloudflare.pem`, paste the PEM contents and Ctrl-X to save, then `nano cloudflare.key`, paste the key contents and Ctrl-X to save.
- From the FoundryVTT UI, choose Settings and set the `Certificate` to `cloudflare.pem` and the `Key` to `cloudflare.key`
- From the TrueNAS UI, under Application Info for foundry, Edit and go down to the Portal settings, change the `Protocol for Portal` to `HTTPS Protocol`. Update to save changes.
- The app should restart but if it doesn't, use the Stop then Start buttons that show to the right of Updates on the Application entry
- Test that you can get to FoundryVTT at `https://IP-of-TrueNAS:30000`
- On your router, create a port forwarding entry for TCP port `443` to go to `IP-of-TrueNAS` and port `30000`.
- In CloudFlare, under SSL/TLS and Overview, choose either Full or Full (strict)
- Test that you can get to your FoundryVTT instance via your domain name for it

> This assumes that the only app you want to access via 443 is this single Foundry instance. If you have multiple apps, you'd need to place a reverse proxy like Traefik between. That is out of scope for these instructions, but look for TrueCharts which includes a Traefik setup.

## Dynamic DNS

Use the [DDNS-Updater](https://www.truenas.com/docs/scale/scaletutorials/apps/communityapps/ddns-updater/) community app to set up dynamic DNS.

## Manual Update

A new major version of FoundryVTT may require a manual upgrade, instead of using the built-in updater. This was the case for Foundry 12.

- Verify you have a snapshot for your `foundry-data` volume. If not, make one now. This will allow you to roll back if there are issues with the upgrade
- Create a new dataset, preset `Generic` or `Apps`. Give it a name, e.g. `foundry-12`
- Download the NodeJS ZIP file for the new version, scp it over to the TrueNAS SCALE server, and unzip it into the new dataset
- Edit your Foundry custom app, and under `Storage` and `Host Path Volumes`, point the `/app` entry to the new dataset you created

This will start the new version, and you can then proceed to upgrade modules, migrate Worlds, and so on.

Once you are happy with the upgrade, the old Foundry app dataset can be deleted.
