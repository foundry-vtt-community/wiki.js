---
title: TrueNAS CE
description: Deploying Foundry on TrueNAS SCALE Docker
published: true
date: 2025-05-15T17:14:12.550Z
tags: 
editor: markdown
dateCreated: 2023-11-26T13:13:16.296Z
---

# Overview

TrueNAS CE runs Linux and Docker Compose under the hood. It offers a GUI and deploying Foundry is relatively simple.

These instructions assume that you are at least somewhat-familar with TrueNAS and ZFS. Start there, make sure you have TrueNAS CE installed and at least one ZFS pool configured. Tested on CE 25.04 in May 2025.

- Create two ZFS datasets, one for the Foundry app, one for its data
- Download and unzip Foundry node.js code
- Deploy a custom Node app that runs FoundryVTT
- If desired, configure TLS and CloudFlare domain
- If desired, configure DDNS for CloudFlare domain

## Create datasets

In the UI under Datasets, create two datasets. For example `apps/foundry-app` and `apps/foundry-data`, under the pool name. Do not place it under `ix-applications`, that is a system-managed dataset.

Preset has to be `Generic` or `Apps` for `foundry-app`, to avoid permissions errors.

You can use `SMB` for `foundry-data`.

These can be under a common parent dataset. For example, I have `apps` as a case-sensitive, Generic, not shared dataset, then `foundry-app` under it the same, and `foundry-data` under it as SMB, case-insensitive.

## Download and unzip Foundry node.js code

Go to the official Foundry site, log in, and under Purchased Licenses download for Operating System `Node.js` - not `Linux`. This gives you a ZIP file.

On TrueNAS CE, enable the SSH service under System Settings -> Services. If you aren't using SSH keys, edit it and configure it to `Allow password authentication`.

Using either WinSCP or scp directly, copy the ZIP file to the admin user of TrueNAS CE. If using scp from PowerShell this is `scp <FoundryVTT-version.zip> <admin-use>r@<IP-of-TrueNAS>:`.

Using either PuTTY or ssh directly, log into your TrueNAS server. If using ssh from PowerShell this is `ssh <admin-user>@<IP-of-TrueNAS>`. Once in, `sudo unzip <FoundryVTT-version.zip> -d /mnt/<POOLNAME>/apps/foundry-app`. This unzips the file you copied to the server into the dataset you created, as user `root`.

That was the hardest part, particularly if you were not familiar with scp/ssh yet.

## Deploy a custom Node app that runs FoundryVTT

In the TrueNAS UI, go to Apps, click on Discover Apps in the top right and then Custom App in the top right.

Any settings I do not mention stay at default, which is most of them.

- Give it an `Application Name`, e.g. `foundry`
- "Image Configuration" `Repository` is `node`
- "Image Configuration" `Tag` is `22`, or whatever [node version](https://nodejs.org/en/about/previous-releases) is Active LTS
- "Image Configuration `Pull Policy` is `Always`

- "Container Configuration" `Hostname` is `foundry`. This makes sure the license stays active across restarts. Any hostname will do, here.
- "Container Configuration" `Entrypoint`, hit the Add button 1 time, and enter:
  - `node`
- "Container Configuration" `Command`, hit the Add button 4 times, and enter:
	- `/app/main.js`
  - `--port=30000`
  - `--headless`
  - `--dataPath=/data`
- "Container Configuration" `Restart Policy` is `Unless Stopped`

- "Network Configuration" `Ports`, hit the Add button, and enter:
	- `Container Port` to `30000`
  - `Node Port` to `30000`

- `Portal Configuration`, click "Add":
  - `Name` to `Foundry`
  - `Protocol for Portal` leave at `HTTP Protocol` for now
  - Check `Use Node IP`
  - `Port` to `30000`
  - `Path` to `/`

- "Storage Configuration", under `Storage` hit Add button twice, and enter:
  - `Type` 1st entry, set to `Host Path`
  - `Mount Path` 1st entry, set to `/data`
  - `Host Path` 1st entry, navigate to `/mnt/<POOLNAME>/apps/foundry-data`
  - `Type` 2nd entry, set to `Host Path`
  - `Mount Path` 2nd entry, set to `/app`
  - `Host Path` 2nd entry, navigate to `/mnt/<POOLNAME>/apps/foundry-app`


And that's it, deploy your app!

If everything worked, the app should eventually show `Running`, and you can see its logs under `Workloads`. If there are issues, the logs will show you what they are.

You can now connect to Foundry at `http://<IP-of-TrueNAS>:30000` and give it a license.

## Remote access via https and CloudFlare

> The instructions below assume that the only app you want to access via 443 is this single Foundry instance. If you have multiple apps, you'd need to place a reverse proxy like Nginx between. That is described in the [TrueNAS forums](https://forums.truenas.com/t/nginx-proxy-manager-configuration-tips-especially-if-you-are-stuck-at-deploying/14653)

You can of course at this point forward `30000` at your router and have your users connect to that. But how cool would it be if they could use a domain name, and TLS worked for video/voice chat?

- Create a login at CloudFlare
- Register a domain name with them
- Under the domain name, DNS, click Add Record and add an `A` record with your external (public) IP. Make this Proxied, which is the default.
- Click on SSL/TLS on the left, then Origin Server. Under Origin Certificate click Create Certificate. This gives you contents for a pem and a key file. 
- SSH into your TrueNAS server as before, `cd /mnt/<POOLNAME>/apps/foundry-data/Config`, and first `nano cloudflare.pem`, paste the PEM contents and Ctrl-X to save, then `nano cloudflare.key`, paste the key contents and Ctrl-X to save.
- From the FoundryVTT UI, choose Settings and set the `Certificate` to `cloudflare.pem` and the `Key` to `cloudflare.key`
- From the TrueNAS UI, under Application Info for foundry, Edit and go down to the Portal settings, change the `Protocol for Portal` to `HTTPS Protocol`. Update to save changes.
- The app should restart but if it doesn't, use the Stop then Start buttons that show to the right of Updates on the Application entry
- Test that you can get to FoundryVTT at `https://<IP-of-TrueNAS>:30000`
- On your router, create a port forwarding entry for TCP port `443` to go to `<IP-of-TrueNAS>` and port `30000`.
- In CloudFlare, under SSL/TLS and Overview, choose either Full or Full (strict)
- Test that you can get to your FoundryVTT instance via your domain name for it

## Dynamic DNS

Use the [DDNS-Updater](https://www.truenas.com/docs/scale/scaletutorials/apps/communityapps/ddns-updater/) community app to set up dynamic DNS.

## Manual Update

A new major version of FoundryVTT may require a manual upgrade, instead of using the built-in updater. This was the case for Foundry 13.

- Verify you have a snapshot for your `foundry-data` volume. If not, make one now. This will allow you to roll back if there are issues with the upgrade
- Create a new dataset, preset `Generic` or `Apps`. Give it a name, e.g. `foundry-13`
- Download the Node.js ZIP file for the new version, scp it over to the TrueNAS CE server, and unzip it into the new dataset
- Edit your Foundry custom app, and under "Storage Configuration", point the existing `/app` Host Path entry to the new dataset you created

This will start the new version, and you can then proceed to upgrade modules, migrate Worlds, and so on.

Once you are happy with the upgrade, the old Foundry app (but not Foundry data!) dataset can be deleted.
