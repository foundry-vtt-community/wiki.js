---
title: systemd
description: Alternative instructions for using systemd within an unbuntu install
published: true
date: 2022-05-19T13:26:44.372Z
tags: hosting, ubuntu, systemd
editor: markdown
dateCreated: 2021-09-29T13:59:30.180Z
---

# Systemd
For admins wishing to use systemd instead of pm2 use the following systemd script.

The script assumes that you are running under the user "foundry" and that your foundry install is at "/srv/foundry"

> [Unit]
> Description=FoundryVTT Service
> 
> [Service]
> Type=simple
> User=foundry
> ExecStart=/usr/bin/node /srv/foundry/foundryvtt/resources/app/main.js --dataPath=/srv/foundry/foundrydata
> Restart=on-failure
> 
> [Install]
> WantedBy=multi-user.target

For more information on how to configure systemd please visit

https://www.shubhamdipt.com/blog/how-to-create-a-systemd-service-in-linux/