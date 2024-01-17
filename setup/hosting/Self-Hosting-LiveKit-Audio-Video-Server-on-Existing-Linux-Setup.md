---
title: Self-Hosting LiveKit Audio/Video Server on Existing Linux Setup
description: Configure your existing self-hosted Linux FoundryVTT server to also self-host your LiveKit A/V server to use within FoundryVTT
published: true
date: 2024-01-17T03:34:16.194Z
tags: linux, self-hosting, cloud, cloudflare, cloud host, a/v service, cloud hosting
editor: markdown
dateCreated: 2024-01-16T22:18:00.531Z
---

# Overview
This guide will show you how to setup a LiveKit A/V server on your existing Linux server for FoundryVTT. This guide assumes you already have a Linux server self-hosted or cloud-hosted and will NOT go over the setup of a FoundryVTT Linux server. I would like to note that I created this guide because all the other guides I found didn't work for my situation. All guides I have come across for the LiveKit server hosting did serve as a foundation for this one. They will be credited and linked in the [Additional Guides & Credits](#additionaal-guides--credits) section of this guide.

> Note: I have found that while LiveKit is significantly better than the built in A/V, there is occasional instability. You can try refreshing the page and enabling the 'Refresh room ID' setting in the LiveKit A/V server settings tab.
{.is-info}


## Requirements
- Linux self or cloud hosted machine
- Nginx Reverse Proxy Service (If you are currently using Caddy as per the Always Free Oracle Cloud Hosting Guide for Foundry, switching to Nginx will be covered in this guide)
- Certbot
- Docker
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

## Docker
To install Docker on your device run the install command (depending on your Linux distro):
- **REHL Linux Distros:** `sudo yum install docker-ce`
- **Debian & Ubuntu:** `sudo apt-get install docker`

## Certbot & Certbot Cloudflare Plugins
First you will need to install snapd if it is not pre-installed on your distro (you can check with the `sudo snap --version` command).
- **REHL Linux Distros:** `sudo yum install snapd`
- **Debian & Ubuntu:** `sudo apt install snapd`

Once snapd is installed, you can install Certbot:
```
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

### Certbot Cloudflare Setup
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
7. Create a file `/etc/letsencrypt/cloudflare.ini` (if the directory doesn't exist, make it with `mkdir /etc/letsencrypt`) with the following text (replace YOUR_TOKEN_HERE with your generated API token):
```
# Cloudflare API token for Certbot
dns_cloudflare_api_token = YOUR_TOKEN_HERE
```
> When the API token expires, you will need to update this file with the new token each time.
{.is-info}
8. Make sure the file has the correct permissions: `sudo chmod 666 /etc/letsencrypt/cloudflare.ini`
9. Get the cert (replace 'your-domain.com' with your domain):
```
sudo certbot certonly --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini -d your-domain.com
```
10. Test to make sure auto-renewal is setup with the command `sudo certbot renew --dry-run`
11. We will want to set up a simple bash script to make sure Nginx reloads when a new cert is generated. 
	a. Create a new script in the `/etc/letsencrypt/renewal-hooks/deploy/` directory: `nano /etc/letsencrypt/renewal-hooks/deploy/reload-nginx.sh`
  b. Enter the following into the file:
  ```
  #!/bin/bash
  systemctl reload nginx
  ```
  c. Make the script executable: `sudo chmod +x /etc/letsencrypt/renewal-hooks/deploy/reload-nginx.sh`
  

# Setup Firewall
The following ports will need to be opened:
- 22/tcp - For SSH
- 80/tcp - For listening on HTTP and TLS issuance
- 443/tcp - For listening on HTTPS and TURN/TLS packets
- 7880-7881/tcp - For WebRTC over TCP
- 3478/udp - For TURN server UDP
- 50000-60000/udp - For WebRTC over UDP
- 8080/tcp - For Foundry connections (TURN Redis server uses port 30000 internally and causes errors sometimes)

If using ufw, enter the follwoing:
```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 7880-7881/tcp
sudo ufw allow 3478/udp 
sudo ufw allow 443/udp
sudo ufw allow 50000:60000/udp
sudo ufw allow 8080/tcp
sudo ufw enable
```

If using iptables, enter the following:
```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 7880:7881 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 3478 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 443 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 50000:60000 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
sudo iptables-save | sudo tee /etc/iptables/rules.v4
```

# Setup Nginx Reverse-Proxy
## Switch From Caddy
If you followed the instructions for the Always Free Oracle Foundry server, you will want to open your caddyfile at `/etc/caddy/Caddyfile` and comment out the lines for your hostname:
```
your.hostname.com {
    reverse_proxy localhost:30000
    encode zstd gzip
}
```
Should become:
```
#your.hostname.com {
#    reverse_proxy localhost:30000
#    encode zstd gzip
#}
```

Commenting this out will allow for you to return to it easily in the event of an issue. 

Since we need to move to port 8080 instead of port 30000 for Foundry (as explained in the previous step). Open the options.json file (located at `/home/ubuntu/foundryuserdata/Config/options.json` or where ever your userdata folder for Foundry is stored) and make sure the port is changed to 8080.

Yours should have the following configurations:
```json
{
  "port": 8080,
  "upnp": true,
  ...
  "hostname": your.domain.com,
  ...
  "proxySSL": true,
  "proxyPort": 443,
  ...
}
```

## Create Directories
Create log directories for the different Nginx config files:
```bash
sudo mkdir /var/log/nginx/livekit
sudo mkdir /var/log/nginx/livekit-turn
sudo mkdir /var/log/nginx/foundry
```

## Setup Nginx Config Files
You will need to create 3 config files in Nginx. One for Foundry, one for LiveKit and one for the LiveKit TURN server (this helps connection stability).

Before we start, you will need to modify the Nginx config file. 
Open the file: `sudo nano /etc/nginx/nginx.conf`
In the `http { ... }` block:
```
http {
		# Other Settings
  	client_max_body_size 10m; #ADD TO THE END OF THE HTTP SECTION
}
```

### Foundry Config File
In the folder `/etc/nginx/sites-available`, create a new file `foundry`:
```
cd /etc/nginx/sites-available
nano foundry
```
Add the following (replacing the example domain with your own) to the file:
```
server {
        listen 80;
        listen [::]:80;
        listen 443 ssl;
        listen [::]:443 ssl;


        server_name                     foundry.your-domain.com;

        ssl_certificate 								/etc/letsencrypt/live/your-domain.com/fullchain.pem;
        ssl_certificate_key 						/etc/letsencrypt/live/your-domain.com/privkey.pem;

        # Additional SSL settings
        ssl_session_cache 							shared:le_nginx_SSL:1m;
        ssl_session_timeout 						1440m;
        ssl_protocols 									TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers 			on;
        ssl_ciphers 										"ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384";


        access_log                      /var/log/nginx/foundry/access.log;
        error_log                       /var/log/nginx/foundry/error.log;

        location / {
                proxy_set_header        Host $host;
                proxy_set_header        X-Real-IP $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        X-Forwaaarded-Proto $scheme;

                proxy_pass              http://127.0.0.1:8080;

                proxy_http_version      1.1;
                proxy_set_header        Upgrade $http_upgrade;
                proxy_set_header        Connection "Upgrade";
                proxy_read_timeout      90;

                proxy_redirect          https:127.0.0.1:8080 http://foundry.your-domain.com;
        }
}

```


### LiveKit Config File
In the same directory, create a config file for LiveKit:
```
nano livekit.your-domain.com.conf
```

Add the following (replacing the example domain with your own) to the file:
```
server {
        listen 80;
        listen [::]:80;
        listen 443 ssl;
        listen [::]:443 ssl;

        server_name                 livekit.your-domain.com;

        ssl_certificate 						/etc/letsencrypt/live/your-domain.com/fullchain.pem;
        ssl_certificate_key 				/etc/letsencrypt/live/your-domain.com/privkey.pem;

        # Additional SSL settings
        ssl_session_cache 					shared:le_nginx_SSL:1m;
        ssl_session_timeout 				1440m;
        ssl_protocols 							TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers 	on;
        ssl_ciphers 								"ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384";


        access_log                  /var/log/nginx/livekit/access.log;
        error_log                   /var/log/nginx/livekit/error.log;

        location / {
                proxy_set_header        Host $host;
                proxy_set_header        X-Real-IP $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        X-Forwarded-Proto $scheme;

                proxy_pass              http://127.0.0.1:7880;

                proxy_http_version      1.1;
                proxy_set_header        Upgrade $http_upgrade;
                proxy_set_header        Connection "Upgrade";
                proxy_read_timeout      90;

                proxy_redirect          https://127.0.0.1:7880 http://livekit.your-domain.com;
        }
}
```

### LiveKit TURN Config File
In the same directory, create a config file for the LiveKit TURN server:
```
nano livekit-turn.your-domain.com.conf
```

Add the following (replacing the example domain with your own) to the file:
```
server {
        listen 80;
        listen [::]:80;
        listen 443 ssl;
        listen [::]:443 ssl;

        server_name                 livekit-turn.your-domain.com;

        ssl_certificate 						/etc/letsencrypt/live/your-domain.com/fullchain.pem;
        ssl_certificate_key 				/etc/letsencrypt/live/your-domain.com/privkey.pem;

        # Additional SSL settings
        ssl_session_cache 					shared:le_nginx_SSL:1m;
        ssl_session_timeout 				1440m;
        ssl_protocols 							TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers 	on;
        ssl_ciphers 								"ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384";


        access_log                  /var/log/nginx/livekit-turn/access.log;
        error_log                   /var/log/nginx/livekit-turn/error.log;

        location / {
                proxy_set_header        Host $host;
                proxy_set_header        X-Real-IP $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        X-Forwarded-Proto $scheme;

                proxy_pass              http://127.0.0.1:5349;

                proxy_http_version      1.1;
                proxy_set_header        Upgrade $http_upgrade;
                proxy_set_header        Connection "Upgrade";
                proxy_read_timeout      90;

                proxy_redirect          https://127.0.0.1:5349 http://livekit-turn.your-domain.com;
        }
}
```


### Enable Config Files
First, make sure there are no files in the `/etc/nginx/sites-enabled` folder. If there are files, remove them with thr `rm` command. Then we are going to make a symbolic link from the 'sites-available' folder to the 'sites-enabled' folder. Replace all instances of 'your-domain.com' with your domain in the file names:

```
sudo ln -s /etc/nginx/sites-available/livekit.your-domain.com.conf /etc/nginx/sites-enabled/livekit.your-domain.com.conf
sudo ln -s /etc/nginx/sites-available/livekit-turn.your-domain.com.conf /etc/nginx/sites-enabled/livekit-turn.your-domain.com.conf
sudo ln -s /etc/nginx/sites-available/foundry /etc/nginx/sites-enabled/foundry
```

Test the Nginx configuration with the `sudo nginx -t` command. If you get an error about too many levels of symbolic links, make sure you aren't using relative paths (e.g. `./sites-available`) and are using the correct absolute path (`/etc/nginx/sites-available/`).

If all works, restart Nginx: `sudo systemctl restart nginx`
# Install & Setup LiveKit
1. First you'll need to get the configuration and installation files: `sudo docker run --rm -it -v$PWD:/output livekit/generate`
	- Input primary domain name: livekit.your-domain.com
	- Input TURN server domain name: livekit-turn.your-domain.com
	- Select latest version of LiveKit
	- Select no to use bundled copy of Redis
	- Select Startup Shell Script
**Copy down the API Key and API Secret displayed on screen. You will need these in order to connect to the LiveKit server.** You can also find the Key and Secret in the file livekit.yaml in the generated installation directory.

2. Next, you'll need to change to the generated installation directory: `cd livekit.your-domain.com`

3. Then make the install script executable: `sudo chmod +x init_script.sh`

4. Install LiveKit using the generated installation script: `sudo ./init_script.sh`

5. Comment out the caddy section of the services in the docker-compose file: `sudo nano /opt/livekit/docker-compose.yaml`:
	
  ```
  # LiveKit requires host networking, which is only available on Linux
# This compose will not function correctly on Mac or Windows
version: "3.9"
services:
  #  caddy:
  #    image: livekit/caddyl4
  #    command: run --config /etc/caddy.yaml --adapter yaml
  #    restart: unless-stopped
  #    network_mode: "host"
  #    volumes:
  #      - ./caddy.yaml:/etc/caddy.yaml
  #      - ./caddy_data:/data
  livekit:
    image: livekit/livekit-server:latest
    command: --config /etc/livekit.yaml
    restart: unless-stopped
    network_mode: "host"
    volumes:
      - ./livekit.yaml:/etc/livekit.yaml
  redis:
    image: redis:6-alpine
    command: redis-server /etc/redis.conf
    network_mode: "host"
    volumes:
      - ./redis.conf:/etc/redis.conf
  ```
# Install LiveKit Module for Foundry
1. Install the LiveKit AVClient module from the Add-on Modules Tab in Foundry

2. Open a world file and enable the LiveKit AVClient module in the module configuration tab.

3. Open the Core Configuration Settings and click on Configure Audio/Video.

4. Click on the Server tab and input the following settings:
```
LiveKit Server Address:  livekit.your-domain.com
LiveKit API Key:         <value from livekit.yaml file>
LiveKit API Secret:      <value from livekit.yaml file>
```
5. Enable Audio/Video on the General tab to connect to LiveKit.

6. If you want players to have voice activiation rather than push to talk, each player will need to configure that in their own A/V settings. Additionally, to talk and have video, you will need to enable that ability for players in the permissions settings.


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