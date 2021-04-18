---
title: Raspberry Pi
description: how to install Foundry VTT on a Raspberry PI 
published: true
date: 2021-04-18T20:39:07.408Z
tags: hosting, self-hosting, raspberry pi
editor: markdown
dateCreated: 2021-04-18T20:02:58.536Z
---

Raspberry Pi OS (previous called Raspbian) it's a Debian-based image for Raspberry Pi.

To install your Raspberry Pi OS follow the official documentation [here](https://www.raspberrypi.org/documentation/installation/installing-images/).

## Requirements

### Installing node.js and it's Package Manager


This guide is for debian-based Raspberry Pi, if you are using other distribution, the comands may change. (Feel free to add your favorite flavor system here too)

```bash 
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```
This installs both nodejs and the node package manager npm. Check both with `node --version` and `npm --version`, at this time of writing node.js is at version `v14.16.1` and npm at version `6.14.12`

## Installing Foundry VTT

You need to download the Node.js version.

You may notice a small `chain` link icon next to the download links on the download page. Clicking this chain icon generates a temporary link which can be used to download Foundry VTT via a terminal or shell interface using `curl`.

```bash
cd ~
mkdir -p foundryvtt foundrydata
cd foundryvtt
curl -o foundryvtt.zip "https://your-download-link-from-foundry-vtt.com-here/"
```

> Remember to add the `chain` URL between double-quotes or the download will fail. 

Checking that the download is complete.

```bash
pi@follower-1:~/foundryvtt $ ls -lah
total 116M
drwxr-xr-x  2 pi pi 4.0K Apr 18 21:11 .
drwxr-xr-x 18 pi pi 4.0K Apr 18 21:09 ..
-rw-r--r--  1 pi pi 116M Apr 18 21:11 foundryvtt.zip
```

You need no to extract the foundry content from the zip file.

```bash
unzip foundryvtt.zip
```

The directory will look like this now.

```bash
pi@follower-1:~/foundryvtt $ ls
chrome_100_percent.pak  foundryvtt      libEGL.so     libvk_swiftshader.so  LICENSES.chromium.html  resources.pak      v8_context_snapshot.bin
chrome_200_percent.pak  foundryvtt.zip  libffmpeg.so  libvulkan.so          locales                 snapshot_blob.bin  vk_swiftshader_icd.json
chrome-sandbox          icudtl.dat      libGLESv2.so  LICENSE.electron.txt  resources               swiftshader
```

Almost there we just need to test it:

```bash
node ~/foundryvtt/resources/app/main.js
```

Now go to your favorite browser and type `http://ip:30000`!

We are rolling, now let's do the final touches.

## Configuring the Installation

First we need to ensure that we have a folder that we can access to easily mantain our assets. By default on linux-based systems the folder is `/home/USER/.local/share/FoundryVTT` and we can change that on 2 ways.

* Editing the config.json file 
* Passing as a entry during the node command.

The second approach is more feasible as we can use it to take advanced of the system configuration. So let's define it as `--dataPath=/home/<USER>/foundrydata`.
 
With the latest version of Rapberry OS the system manager is *systemd* and we can configure it with the following file. 

Create it on `sudo nano /etc/systemd/system/foundryvtt.service`
 
```
[Unit]
Description=Foundry VTT

[Service]
Type=simple
ExecStart=node /home/<USER>/foundryvtt/resources/app/main.js --dataPath=/home/<USER>/foundrydata
Restart=on-failure
User=<USER>

[Install]
WantedBy=multi-user.target
```

> Remember to change the `<USER>` for the user that Raspberry PI user.

Now we can manage our Foundry

```
sudo systemctl daemon-reload
sudo systemctl start foundryvtt
sudo systemctl enable foundryvtt
```







