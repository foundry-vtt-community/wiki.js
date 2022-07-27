---
title: Archlinux
description: Foundry VTT setup in Archlinux
published: true
date: 2022-05-19T13:24:32.852Z
tags: tutorial, hosting, setup, linux, self-hosting
editor: markdown
dateCreated: 2021-02-28T01:56:56.967Z
---

>This article was automatically imported from the previous wiki and may contain inaccurate or outdated information.
{.is-warning}

These instructions show one of the possible ways to install Foundry VTT in an Archlinux system. At the end of the process, the service should be accessible from a browser using [a secure connection](https://wiki.archlinux.org/index.php/SSL).

## Requirements

Before installing Foundry VTT, you should have a working installation of [nginx](https://wiki.archlinux.org/index.php/Nginx) and [Node.js](https://wiki.archlinux.org/index.php/Node.js).

## Creating directories for Foundry VTT

First of all, create these directories to install the software and its data. You can create these directories under /home, for example:

```
# mkdir -p /home/foundry/{foundryvtt,foundrydata}
```

## Creating a system user

Foundry VTT will be run by a system user. First, [create a system user](https://wiki.archlinux.org/index.php/Users_and_groups#Example_adding_a_system_user) with the name '''foundry''' or any other name.

```
# useradd -r -s /usr/bin/nologin ''foundry''
```

## Downloading Foundry VTT

Once you have registered in https://foundryvtt.com and have purchased a license, you need to go to the "Purchased Software Licenses" section of your profile page. There you will see different packages for the software. You need to download the Node.js version.

You may notice a small 'chain' link icon next to the download links on the download page. Clicking this chain icon generates a temporary link which can be used to download Foundry VTT via a terminal or shell interface using wget.

When downloading the link using a command line utility such as wget it's important to wrap the link in double-quotes. This ensures that the link is read correctly by the command. For example:

```
# wget -O foundryvtt.zip "https://your-download-link-from-foundry-vtt.com-here/"
```

The downloaded file must be uncompressed to the *foundryvtt* directory that you created before. Once this directory is populated, you need to set the proper permissions:

```
# unzip foundryvtt.zip -d /home/foundry/foundryvtt
# chown -R foundry:foundry /home/foundry/foundryvtt
```

## Running the software without a proxy

Now you can test if the software works. Run the server as the user *foundry*:

```
# su - foundry -s /bin/bash -c "node foundryvtt/resources/app/main.js --dataPath=/home/foundry/foundrydata"
```

While it's running, you should be able to connect to the server from a browser, using port 30000. If you wish, you can now set your admin password and save your license key.

Now stop the server using Ctrl-c.

## Creating a service with systemd

One of the ways to run the software as a daemon is using [Systemd](https://wiki.archlinux.org/index.php/Systemd). You can create a simple service for Foundry VTT:

```
# nano /etc/systemd/system/foundryvtt.service
```

This is an example of this systemd unit:

```
 [Unit]
 Description=Foundry VTT
 
 [Service]
 Type=simple
 ExecStart=node /home/foundry/foundryvtt/resources/app/main.js --dataPath=/home/foundry/foundrydata
 Restart=on-failure
 User=foundry
 
 [Install]
 WantedBy=multi-user.target
```

As suggested in the [installation instructions](https://foundryvtt.com/article/nginx/), edit the software options to prepare it to be accessed through a proxy server:

```
# nano /home/foundry/foundrydata/Config/options.json
```

Set the following options, keeping the other ones:

```
 "hostname": "your.hostname.com",
 "routePrefix": null,
 "sslCert": null,
 "sslKey": null,
 "port": 30000,
 "proxyPort": 443,
 "proxySSL": true
```

## Configuring nginx

To configure a nginx proxy server for Foundry VTT you can use this example from the official documentation:

```
 # This goes in a file within /etc/nginx/sites-available/. By convention,
 # the filename would be either "your.domain.com" or "foundryvtt", but it
 # really does not matter as long as it's unique and descriptive for you.
 
 # Define Server
 server {
 
     # Enter your fully qualified domain name or leave blank
     server_name             your.hostname.com;
 
     # Listen on port 443 using SSL certificates
     listen                  443 ssl;
     ssl_certificate         "/etc/letsencrypt/live/your.hostname.com/fullchain.pem";
     ssl_certificate_key     "/etc/letsencrypt/live/your.hostname.com/privkey.pem";
 
     # Sets the Max Upload size to 300 MB
     client_max_body_size 300M;
 
     # Proxy Requests to Foundry VTT
     location / {
 
         # Set proxy headers
         proxy_set_header Host $host;
         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
         proxy_set_header X-Forwarded-Proto $scheme;
 
         # These are important to support WebSockets
         proxy_set_header Upgrade $http_upgrade;
         proxy_set_header Connection "Upgrade";
 
         # Make sure to set your Foundry VTT port number
         proxy_pass http://localhost:30000;
     }
 }
 
 # Optional, but recommend. Redirects all HTTP requests to HTTPS for you
 server {
     if ($host = your.hostname.com) {
         return 301 https://$host$request_uri;
     }
 
     listen 80;
     listen [::]:80;
 
     server_name your.hostname.com;
     return 404;
 }
```

You can get your certificates using [certbot](https://wiki.archlinux.org/index.php/Certbot).

## Running the service

Now you can [start](https://wiki.archlinux.org/index.php/Systemd#Basic_systemctl_usage) and [enable](https://wiki.archlinux.org/index.php/Systemd#Basic_systemctl_usage) both Foundry VTT and nginx:

```
 # systemctl start foundryvtt
 # systemctl enable foundryvtt
 # systemctl start nginx
 # systemctl enable nginx
```

At this point you should be able to access your server from a browser using https, with the default port 443 (example: `https://your.hostname.com`).
