---
title: CloudAtCost
description: A Canadian virtual server provider, where you buy the resources permanently, and can build as much or as beefy as you like.
published: true
date: 2022-05-09T04:35:16.907Z
tags: 
editor: markdown
dateCreated: 2021-02-12T06:10:46.995Z
---

>This article was automatically imported from the previous wiki and may contain inaccurate or outdated information.
{.is-warning}

# CloudAtCost

## NOTE: CloudAtCost Servers can be volitile!  Do not depend on uptime!  Build and tear down often!

This guide is a single script that is mostly from the [Ubuntu VM](/en/setup/Ubuntu-VM) solution, without the Proxy function, and with some newer information for LetsEncrypt.

First, as noted above, using CloudAtCost, it is highly suggested to use a local copy of Foundry to build and prepare all of your content, and moving that data up to your server near to game-time.

1. Create the server instance by clicking the Build Server button in the top-right menu section
1. Select your infrastructure location
1. Select your CPU count, memory, storage, and other options (HA is High Availability)
1. Select Ubuntu LTS on the right-hand side (v18.04 as of 20210211)
1. Allow the server to build, grab the IPv4 address and default password.

Configure DNS for your domain.  Ensure you can ping the server NAME, not just the IP.  This is required for the Certbot/LetsEncrypt tool.

Open either WinSCP or Putty, and SSH into the server.

OPTIONAL: Before beginning, update the server to the latest LTS build (some packages may need to be manually upgraded to get there).  This will take some time.  If you do this afterwards, you will have to reinstall Node.js and PM2, plus setting up the PM2 configuration for Foundry.  Not terrible, but an unncecssary step.

In all cases with this instruction set, replace "`vtt.domain.com`" with your own FQDN (`foundry.domain.com`, `fvtt.geocities.com`, `MyPlayersHateMe.rpg`, etc...).

## Primary Installs

Start out with updating and upgrading existing packages:
```
sudo apt-get update; sudo apt-get upgrade -y
```

Set up our new packages that are needed, similar to the Ubuntu VM article, starting with the Node.js portion:
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs; sudo npm install pm2 -g
```

Grab nginx and unzip:
```
sudo apt-get install -y nginx unzip
```

Finally, put in the latest [Certbot (LetsEncrypt) agent for nginx](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx), and configure it:
```
sudo snap install core; sudo snap refresh core; sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

## The Good Stuff

Now we get to the actionable items.  Make some directories, and pull down the Foundry files:
```
mkdir -p ~/.local/share/FoundryVTT
mkdir ~/foundry
cd ~/foundry
wget -O foundryvtt.zip "<foundry-nodejs-zip-download-url>"
unzip foundryvtt.zip
rm foundryvtt.zip
```

OPTIONAL: If you have an existing Foundry session with the data/scenes/characters you have prepared, upload it to a file share location that you can get to.  In this example, OneDrive is used.

1. Zip the FoundryVTT directory of your desktop version
 - Option for Windows users is to use this Powershell command: `Compress-Archive -Path $env:LOCALAPPDATA\FoundryVTT\ -DestinationPath $env:OneDrive\FoundryVTT-Data.zip -Update`
2. Upload to OneDrive, and open the web interface
3. Right-click the zip file, and choose Embed
4. Take the `"https://onedrive.live.com/embed?xxxxxxxxx&resid=xxxxxxxx&authkey=xxxxxx"` string, and replace "embed" with "download"

```
wget -O foundrydata.zip "https://onedrive.live.com/download?xxxxxxxxx&resid=xxxxxxxx&authkey=xxxxxx" --no-check-certificate
unzip foundrydata.zip -d ~/.local/share
rm foundrydata.zip
```

This section replaces pieces of the `options.json` file to handle HTTPS, and point to the right data directory.  The command `sed` is used, which is operating with Regex replacement commands (`'s/abc/XYZ/g'` will take "abcdef" and make it "XYZdef"), and the extra `\` before the standard `*nix` pathing is an escape caracter to make sure our paths are right.
```
sed -i -e 's/30000/443/g' ~/.local/share/FoundryVTT/Config/options.json
sed -i -e 's/<UserAppDataLocal>\/FoundryVTT/\/root\/.local\/share\/FoundryVTT/g' ~/.local/share/FoundryVTT/Config/options.json
sed -i -e 's/"hostname": null,/"hostname": "vtt.domain.com",/g' ~/.local/share/FoundryVTT/Config/options.json
sed -i -e 's/"sslCert": null,/"sslCert": "\/etc\/letsencrypt\/live\/vtt.domain.com\/cert.pem",/g' ~/.local/share/FoundryVTT/Config/options.json
sed -i -e 's/"sslKey": null,/"sslKey": "\/etc\/letsencrypt\/live\/vtt.domain.com\/privkey.pem",/g' ~/.local/share/FoundryVTT/Config/options.json
```

To make everything work nice with Certbot and nginx for now, configure the nginx site, and create the log directory:
```
sudo echo -e "server {\n    listen 80;\n    server_name vtt.domain.com;\n    access_log /var/log/nginx/vtt.domain.com/access.log;\n    error_log /var/log/nginx/vtt.domain.com/error.log;\n}" >> /etc/nginx/sites-available/vtt.domain.com
sudo ln -s /etc/nginx/sites-available/vtt.domain.com /etc/nginx/sites-enabled
mkdir -p /var/log/nginx/vtt.domain.com
```

Restart the nginx service:

```
sudo service nginx restart
```

Next we get to the Certbot and LetsEncrypt configuration.  It's a simple one-liner, replace `<youremail@domain.com>` with your actual email:

```
sudo certbot --agree-tos -m <youremail@domain.com> -d vtt.domain.com --no-eff-email --nginx
```

This will generate and install the LetsEncrypt certs in `etc/letsencrypt/live/vtt.domain.com/`.

Now stop the nginx service.  We don't need it anymore.

```
sudo service nginx stop
sudo update-rc.d -f nginx disable
```

Configure PM2 to start up Foundry:

```
pm2 start "~/foundry/resources/app/main.js" --name "foundry"
```

And check your server with your browser, using HTTPS.  You should be presented with the License Agreement page, and if so, you are good to go!

Make sure to save your PM2 process list:

```
pm2 save --force
```

## TL;DR (whole script)
```
# Foundry VTT - Ubuntu Build SH
# run this in PS on local copy before game night
# Compress-Archive -Path $env:LOCALAPPDATA\FoundryVTT\ -DestinationPath $env:OneDrive\FoundryVTT-Data.zip -Update
sudo apt-get update; sudo apt-get upgrade -y
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs; sudo npm install pm2 -g
sudo apt-get install -y nginx unzip
sudo snap install core; sudo snap refresh core; sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo rm /etc/nginx/sites-enabled/default; sudo service nginx restart
mkdir -p ~/.local/share/FoundryVTT
mkdir ~/foundry
cd ~/foundry
wget -O foundryvtt.zip "<foundry-nodejs-zip-download-url>" --no-check-certificate
unzip foundryvtt.zip
rm foundryvtt.zip
wget -O foundrydata.zip "https://onedrive.live.com/download?xxxxxxxxx&resid=xxxxxxxx&authkey=xxxxxx" --no-check-certificate
unzip foundrydata.zip -d ~/.local/share
rm foundrydata.zip
sed -i -e 's/30000/443/g' ~/.local/share/FoundryVTT/Config/options.json
sed -i -e 's/<UserAppDataLocal>\/FoundryVTT/\/root\/.local\/share\/FoundryVTT/g' ~/.local/share/FoundryVTT/Config/options.json
sed -i -e 's/"hostname": null,/"hostname": "vtt.domain.com",/g' ~/.local/share/FoundryVTT/Config/options.json
sed -i -e 's/"sslCert": null,/"sslCert": "\/etc\/letsencrypt\/live\/vtt.domain.com\/cert.pem",/g' ~/.local/share/FoundryVTT/Config/options.json
sed -i -e 's/"sslKey": null,/"sslKey": "\/etc\/letsencrypt\/live\/vtt.domain.com\/privkey.pem",/g' ~/.local/share/FoundryVTT/Config/options.json
sudo echo -e "server {\n    listen 80;\n    server_name vtt.domain.com;\n    access_log /var/log/nginx/vtt.domain.com/access.log;\n    error_log /var/log/nginx/vtt.domain.com/error.log;\n}" >> /etc/nginx/sites-available/vtt.domain.com 
sudo ln -s /etc/nginx/sites-available/vtt.domain.com /etc/nginx/sites-enabled
mkdir -p /var/log/nginx/vtt.domain.com
sudo service nginx restart
sudo certbot --agree-tos -m <youremail@domain.com> -d vtt.domain.com --no-eff-email --nginx
sudo service nginx stop
sudo update-rc.d -f nginx disable
pm2 start "node ~/foundry/resources/app/main.js" --name "foundry"
pm2 save --force
```

## Updating Data on Server
```pm2 stop "foundry"
wget -O foundrydata.zip "https://onedrive.live.com/download?xxxxxxxxx&resid=xxxxxxxx&authkey=xxxxxx" --no-check-certificate
unzip -u foundrydata.zip -x options.json -d ~/.local/share
rm foundrydata.zip
pm2 start "foundry"
```

## Extract Data from Server
```
pm2 stop "foundry"
tar -czvf ~/foundrydata.tar.gz ~/.local/share/FoundryVTT
```
Download, unpack where appropriate for your OS with the appropriate tool.