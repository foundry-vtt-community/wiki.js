---
title: systemd
description: Alternative instructions for using systemd within an unbuntu install
published: true
date: 2021-09-29T13:59:30.180Z
tags: hosting, ubuntu, systemd
editor: markdown
dateCreated: 2021-09-29T13:59:30.180Z
---

# Systemd
For admins wishing to use systemd instead of pm2 use the following systemd script.

> [Unit]
> Description=FoundryVTT Service
> 
> [Service]
> Type=simple
> User=USERNAME
> ExecStart=/usr/bin/node /home/userver/software/games/foundry/resources/app/main.js --dataPath=PATH_TO_FOUNDRY --port=30000
> Restart=on-failure
> 
> [Install]
> WantedBy=multi-user.target

For more information on how to configure systemd please visit

https://www.shubhamdipt.com/blog/how-to-create-a-systemd-service-in-linux/