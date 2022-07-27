---
title: Recommended Linux Installation Guide
description: Sets up Foundry on linux with Caddy as reverse proxy. 
published: true
date: 2022-07-04T12:04:17.909Z
tags: 
editor: markdown
dateCreated: 2021-05-05T21:54:44.555Z
---

# Recommended Linux Installation and Usage Guide

# A. Overview
## Objective

This guide provides easy to follow steps for installing and using Foundry as a headless server in a linux environment. It is written to account for a number of linux common linux distributions and installation scenarios. 

At the end of this guide you will have:

* Foundry server running 24/7 behind a reverse proxy providing HTTPS, including automatic restarts
* A domain name pointing to the server
* (Optional) Cyberduck, a file transfer program, configured to manage user data of the Foundry server

## Important Information and Requirements

This guide assumes that you have linux already installed on a server of some sort, either a PC on a local network or a VPS/VM in the cloud without a webserver or Foundry already installed an running. 

The following is required to complete this guide:

1. A basic understanding of using a terminal that includes the ssh utility, such as:
a. Powershell in Windows.
b. Terminal in Linux or MacOS. 
2. Valid domain name, such as:
a. A purchased domain name from a registrar like [Namecheap](https://namecheap.com) or [gandi.net](https://gandi.net).
b. A free subdomain from a free domain name service like [Duck DNS](http://duckdns.org).


## Preferred Linux Distribution

This guide includes instructions for a number of linux distributions, as described below. If you do not have a strong reason for another distribution, we recommend **Ubuntu 20.04**. 

This guide supports the following distributions:

1. Debian 11 based, such as (but not limited to):
a. Debian
b. Ubuntu
c. Raspberry Pi OS (***not*** Raspberry Pi Desktop which is based on 32-bit Debian and does not support Nodejs 14+)
2. CentOS 8 based, such as (but not limited to):
a. CentOS
b. Red Hat Linux
c. Fedora

Any distrition that uses the `apt` or `dnf` package managers *should* be compatible with this guide. Any differentiation in instructions for the distributions will be clearly indicated where necessary. 

>This guide requires Debian 11 or CentOS 8 based distributions or higher. Using lower versions may not function properly. {.is-info}

## Getting Help
If you get stuck on a particular step, please first ensure that all commands in black text quotes entered *exactly* as they appear. 

Troubleshooting assistance for this guide can be found on the official Foundry Discord. Copy the link from the specific step number (ie: C5) you are having difficulty with and then post in the **#install-and-connection** channel on the [Foundry Discord](https://discord.gg/foundryvtt).

# B. User and General System Setup
## Objective

At the end of this section, you will have a non-root user setup to run Foundry as well as the necessary software to run Foundry managed behind a reverse proxy. 

>This section assumes that you are connected via terminal to your linux server. This can be either throught direct local access to a terminal or through ssh. {.is-info}

>How to connect to your server is out of scope for this guide. You should have terminal access if you installed linux on a local pc, or if you are using a cloud VPS/VM provider then review their documentation for how to get `ssh` or `terminal` access to your instance. {.is-info}

## User Setup

We must use a non-root user that is part of the `sudoers` group to properly continue and manage Foundry. 

>If you are certain that you have a non-root user that has access to `sudo` then you may skip the next steps and continue to [System Setup](#system-setup). 
{.is-warning}

<a id="B1" href="#B1">B1.</a> Firstly, check your terminal to determine which user you are logged in as. Your terminal should look like:

```
<user>@<servername>:_   
```

<a id="B2" href="#B2">B2.</a> The `<user>` field will show you which user you are connected as. If it shows `root` then we need to create a new user and add them to sudoers. If the user is **NOT** `root` then you can skip and continue to step [B5](#system-setup).

>Any references of `<user>` in the rest of the guide should be replaced with `foundry` in this case. {.is-info}

<a id="B3" href="#B3">B3.</a> Click the heading for your linux distribution to expand the commands to create a new user. We will create a user named `foundry` and add them to sudoers. Choose a strong password for the user that you will remember.

You can leave all other fields blank or fill with whatever info you'd like.


<details><summary>Ubuntu/Debian/Raspberry Pi OS ▼</summary>
  
```
adduser foundry
usermod -aG sudo foundry

```
</details>

<details><summary>CentOS/Red Hat/Fedora ▼</summary>
  
```
adduser foundry
passwd foundry
usermod -aG wheel foundry

```
</details>

<a id="B4" href="#B4">B4.</a> Assume the new user by:

```
su - foundry
```
You should now see the terminal look like:
```
foundry@<servername>:_
```


## System Setup
We will now install the necessary software to run and manage Foundry behind a reverse proxy. This includes:

* nodejs 14+, Required to run Foundry itself
* caddy 2+, the webserver that will be used as a reverse proxy
* pm2, the process manager that will keep Foundry running
* unzip, the utility used to decompress the Foundry installation zip archive
* nano, the text editor used to edit configuration files

>To continue, you must be using a non-root user with `sudo` access. If that is not the case, please review the steps in [User Setup](#user-setup). {.is-info} 

<a id="B5" href="#B5">B5.</a> First, let's update the system to make sure we have everything as up-to-date as possible. This may take a few minutes.

<details><summary>Ubuntu/Debian/Raspberry Pi OS ▼ </summary>
  
```
sudo apt update
sudo apt upgrade -y

```
</details>

<details><summary>CentOS/Red Hat/Fedora ▼ </summary>
  
```
sudo dnf update -y

```
</details>

>You may be asked for a password the first time you use a `sudo` command. This is normal. Enter the password for the user. {.is-info}

>If after entering the correct password, you receive an error: `<user> is not in the sudoers file` or similar, then you must login as **root** and complete ther [User Setup](#user-setup). {.is-warning}

<a id="B6" href="#B6">B6.</a> Add the nodejs 16 repository to the system package manager:

<details><summary>Ubuntu/Debian/Raspberry Pi OS ▼ </summary>
  
```
curl -sL https://deb.nodesource.com/setup_16.x | sudo bash -
```
</details>

<details><summary>CentOS/Red Hat/Fedora ▼ </summary>
  
```
curl -sL https://rpm.nodesource.com/setup_16.x | sudo bash -
```
</details>

<a id="B7" href="#B7">B7.</a> Add the caddy repository to the system package manager:

<details><summary>Ubuntu/Debian/Raspberry Pi OS ▼ </summary>
  
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
```
</details>

<details><summary>CentOS/Red Hat/Fedora ▼ </summary>
  
```
sudo dnf install 'dnf-command(copr)'
sudo dnf copr enable @caddy/caddy -y
```
</details>

<a id="B8" href="#B8">B8.</a> Install nodejs, caddy, unzip, and nano:

<details><summary>Ubuntu/Debian/Raspberry Pi OS ▼ </summary>
  
```
sudo apt update
sudo apt install nodejs caddy unzip nano -y
```
</details>

<details><summary>CentOS/Red Hat/Fedora ▼ </summary>
  
```
sudo dnf install nodejs caddy unzip nano -y
```
</details>

<a id="B9" href="#B9">B9.</a> Check that nodejs and npm are installed and the correct versions:

```
node --version
npm --version
```
Node should return a version of 14 or greater. The npm version doesn't matter, but should return something. 

<a id="B10" href="#B10">B10.</a> Install pm2:

```
sudo npm install pm2 -g
```
<a id="B11" href="#B11">B11.</a> Add pm2 to startup as the current user. Be sure to carefully read the `blue notice` and follow all instructions given:

```
pm2 startup
```
>***REQUIRED STEP*** 
>You will need to carefully review the output of the `pm2 startup` command. It will include a specific instruction on how to enable pm2 startup on your particular distribution. Copy and paste this command exactly. {.is-info}

# C. Foundry and Reverse Proxy Setup
## Objective
At the end of this section you will have a functional installation of Foundry using HTTPS and Caddy as a reverse proxy. Foundry will be set to restart any time the server is restarted, managed by pm2.

## Download, Install, and Test Foundry

<a id="C1" href="#C1">C1.</a> Login to [FoundrVTT](https://foundryvtt.com) and navigate to the **Purchased Licenses** page. 

<a id="C2" href="#C2">C2.</a>	Select the recommended version and **Linux/NodeJS** in the downloads options. Click on the :link:`Timed URL` button to copy a download url. 

> Be sure to click the :link:`Timed URL` and not the :download:`Download` button to copy and authenticated temporary download link. This link will expire in 5 minutes, after which it will need to be copied again from the gear. {.is-info}

<a id="C3" href="#C3">C3.</a>	Run the following commands, pasting the download url where you see `<download url>`. In most terminals, you can right click to paste the copied url.
```
mkdir ~/foundry
wget --output-document ~/foundry/foundryvtt.zip "<download url>"
```
> Make sure to include the quote symbols before and after the `<download url>` or the file may not download properly. {.is-info}

<a id="C4" href="#C4">C4.</a>	Once downloaded, extract Foundry and cleanup the zip file:
```
unzip ~/foundry/foundryvtt.zip -d ~/foundry/
rm ~/foundry/foundryvtt.zip
```

> If you get an error when unzipping Foundry, please ensure you've downloaded the **Linux/NodeJS** version and if not, repeat step [C2](#C2). {.is-warning}

<a id="C5" href="#C5">C5.</a>	Create the User Data folder for Foundry to store data:
```
mkdir -p ~/foundryuserdata
```
<a id="C6" href="#C6">C6.</a>	Test that Foundry runs successfully by running the following command. Replace the `<user>` portion with the name of the user currently being used.
```
cd ~
node foundry/resources/app/main.js --dataPath=/home/<user>/foundryuserdata
```

<a id="C7" href="#C7">C7.</a>	You should see these <span style="color:green">info</span> lines at the end of the output, indicating that Foundry is successfully running. 

![Foundry Launched](/images/oracle/image29.webp)

>If you do not see the above output ending with `Server started and listening on port 30000`, review step [C6](#C6) to ensure you replaced `<user>` with the current user. {.is-info}

<a id="C8" href="#C8">C8.</a>	Test the connection to Foundry by opening `http://<IP address>:30000` in a new browser tab, where `<IP address>` is the either the external IP address of your cloud server or server internal IP address in your home network. 

>If you are setting up a server on your local network, use the local/internal IP address of the server. If you are setting up a server in the cloud, use the public IP address of the server. {.is-info}

>You should see a Foundry screen asking for a license key at this point. If you do not see a Foundry screen at this point likely the your linux distribution or cloud provider has a firewall enabled that is blocking port 30000, or an incorrect IP address was used. Check the IP address carefully and otherwise review the documentation for your linux distribution or cloud provider for how to open port 30000 in the firewall. {.is-warning}

<a id="C9" href="#C9">C9.</a>	In the terminal window, press <kbd>ctrl</kbd>-<kbd>c</kbd> to stop the Foundry test. You should see the last few lines as below, and a blinking cursor at `<user>@<server>:~$`.

<a id="C10" href="#C10">C10.</a>	We will now set Foundry to be managed by pm2 so that Foundry will always be running, even in the case where the instance has been restarted. To do so, run the following command. Be sure to replace `<user>` with the name of the actual user. There are two replacements:
```
pm2 start "node /home/<user>/foundry/resources/app/main.js --dataPath=/home/<user>/foundryuserdata" --name foundry
```
<a id="C11" href="#C11">C11.</a>	Double check pm2 has launched Foundry correctly:
```
pm2 list
```
![pm2 List](/images/oracle/image1.webp)
>If the **status** column does not show <span style="color:green">online</span> then review step [C10](#C10) above before continuing. {.is-warning}

<a id="C12" href="#C12">C12.</a> Save the current pm2 configuration so that it can manage and restart Foundry as necessary:

```
pm2 save
```


## Set up the Caddy Reverse Proxy

> This section assumes that you have a valid domain name with an A record pointing to your `<public IP address>`. If you do not have a domain name. you can use a service like [Duck DNS](http://duckdns.org) (see [guide](https://foundryvtt.wiki/en/setup/hosting/ddns) if you are hosting on a home network) to get a free domain and point it to `<public IP address>`. Having a domain name is required for this section. {.is-info}


<a id="C13" href="#C13">C13.</a> Run the following command to begin editing the Caddyfile:
  
  ```
  sudo nano /etc/caddy/Caddyfile
  ```
  
<a id="C14" href="#C14">C14.</a> Follow the steps below according to your relevant setup:

<details><summary>Cloud-hosted server ▼ </summary>


  <a id="C15a" href="#C15a">C15a.</a> Delete all the text and replace with the following, making sure to replace the `your.hostname.com` portion with your actual domain name:
  
  ```
  # This replaces the existing content in /etc/caddy/Caddyfile

  # A CONFIG SECTION FOR YOUR HOSTNAME
  
  your.hostname.com {
      # PROXY ALL REQUEST TO PORT 30000
      reverse_proxy localhost:30000
      encode zstd gzip
  }

  # Refer to the Caddy docs for more information:
  # https://caddyserver.com/docs/caddyfile
  ```
  
</details>

<details><summary>Home network server ▼ </summary>
  
  <a id="C15b" href="#C15b">C15b.</a> Delete all the text and replace with the following, making sure to replace the `your.hostname.com` and `your.internal.ip.address` portions with your actual domain name and server internal IP address:
  
  ```
  # This replaces the existing content in /etc/caddy/Caddyfile

  # A CONFIG SECTION FOR YOUR IP AND HOSTNAME
  
  {
      default_sni your.internal.ip.address
  }
  
  your.internal.ip.address {
      # PROXY ALL REQUEST TO PORT 30000
      tls internal
      reverse_proxy localhost:30000
      encode zstd gzip
  }
  
  your.hostname.com {
      # PROXY ALL REQUEST TO PORT 30000
      reverse_proxy localhost:30000
      encode zstd gzip
  }

  # Refer to the Caddy docs for more information:
  # https://caddyserver.com/docs/caddyfile
  ```
</details>

<a id="C16" href="#C16">C16.</a> Press <kbd>ctrl</kbd>-<kbd>x</kbd> then <kbd>y</kbd> and <kbd>enter</kbd> to save your changes.

<a id="C17" href="#C17">C17.</a>	Restart Caddy to pick up the new settings by:
```
sudo service caddy restart
```
>Caddy handles all forwarding to HTTPS as well as the encryption certificates. No further configuration is needed to get those working. {.is-info}

<a id="C18" href="#C18">C18.</a> Tell Foundry that we are running behind a reverse proxy by changing the `options.json` file. Open the file for editing by:
```
nano ~/foundryuserdata/Config/options.json
```
<a id="C19" href="#C19">C19.</a> Find the `proxySSL` and `proxyPort` parameters, and change them as below. Leave all other options as they are. The `hostname` parameter will tell Foundry to use a hostname in the Internet Invite Link. Replace `<your.domain.name>` with your actual domain name.
```
...
"proxyPort": 443,
...
"proxySSL": true,
...
"hostname": "<your.domain.name>",
...
```
>Make sure your hostname is in **quotes** as above, and be sure not to delete any commas or other JSON elements while editing this file. Change ONLY the values afer the `:` {.is-warning}

<a id="C20" href="#C20">C20.</a> Press <kbd>ctrl</kbd>-<kbd>x</kbd> then <kbd>y</kbd> and <kbd>enter</kbd> to save your changes.

<a id="C21" href="#C21">C21.</a> Restart Foundry to pick up the changes to configuration: 

```
pm2 restart foundry
```

<a id="C22" href="#C22">C22.</a>	Test your site by opening a new browser tab to `http://your.domain.name` or `http://server.internal.IP.address`. If everything is working, you will see Foundry load and the site will have the encrypted lock icon. It is now ready for use and no further configuration is needed. 

>Sometimes DNS records can take a few minutes and up to a couple hours to be recognized across the internet. If you receive an error along the lines of `server IP address could not be found` or `having trouble finding that site` then the DNS records may just need more time. Wait a few minutes and try again. {.is-warning}

>If you are hosting on your home network, you **must** use an external device to test the connection to the domain name. You can only test the connection to the internal IP address from within the network. {.is-warning}

> This concludes the portion of the guide that sets Foundry up and running. You may now continue using Foundry this way without issue going forward. {.is-info}

# (Optional) D. Accessing Userdata Files with Cyberduck
## Objective
At the end of this optional section, you will be able to directly access the files in your userdata directory with Cyberduck. This is useful for moving, deleting, or bulk uploading assets for Foundry. 

## Install and Setup Cyberduck

<a id="D1" href="#D1">D1.</a> Download and install Cyberduck for your platform from the [Cyberduck website](https://cyberduck.io/download/).

<a id="D2" href="#D2">D2.</a> Once installed, open Cyberduck and click **Open Connection**:

![Cyberduck - Open Connection](/images/generic-linux/cyberduck1.webp)

<a id="D3" href="#D3">D3.</a> In the Open Connection window, click the dropdown menu and select **SFTP (SSH File Transfer Protocol)**

![Cyberduck - Choose SFTP](/images/generic-linux/cyberduck2.webp)

<a id="D4" href="#D4">D4.</a> Enter the following information in the corresponding fields, replacing any values in `<>` with the values as earlier in the guide:

* Server: `<your.domain.name>` or `<server internal IP address>` if hosting in a home network
* Username: `<user>` 
* Password: `<password>` (Leave blank if your existing user needs an ssh private key file to connect)
* SSH Private Key: Click `Browse` and select your SSH Private Key file. You may need to change the file type to **All Files**. Leave blank if using a password only. 

<a id="D5" href="#D5">D5.</a> Click **Connect**

<a id="D6" href="#D6">D6.</a> Double click on the `foundryuserdata` directory, then the `Data` directory.

<a id="D7" href="#D7">D7.</a> Click the **Bookmark** menu, then **New Bookmark**. Close the window that pops up. 

>You now have a bookmarked connection in Cyberduck to the location of your Foundry userdata directory. Simply launch Cyberduck and double click the bookmark to connect and manage your files. {.is-info}

# (Optional) E. Creating Swapfile
## Objective
The minimum RAM requirement for hosting Foundry is 1GB, however some systems or modules may use more than the minimum RAM. If your linux host has less than 2GB of RAM you can create a swapfile to prevent out-of-memory errors when using heavier modules, systems, or large compendiums. 

## Create and Enable Swapfile
The instructions below are compatible with the <a href="#preferred-linux-distribution">preferred linux distributions</a>.

All commands below are assumed to be entered by a non-root sudoer user, such as the `foundry` user created in <a href="#B1">B1</a> to B4. 

<a id="E1" href="#E1">E1.</a> Create a file to be used as swap:
```
sudo fallocate -l 2G /swapfile
```
>This will create a 2GB swapfile which is a recommended size for hosts with 1GB of RAM. You can increase this size as you'd like, but it is not recommended to create a smaller swapfile. {.is-info}

<a id="E2" href="#E2">E2.</a> Change the permissions to prevent regular users from accessing the swapfile:
```
sudo chmod 600 /swapfile
```

<a id="E3" href="#E3">E3.</a> Mark the swapfile as a linux swap area:
```
sudo mkswap /swapfile
```

<a id="E4" href="#E4">E4.</a> Ensure that the swapfile is enabled permanently by editing the `/etc/fstab` file. 

```
sudo nano /etc/fstab
```

<a id="E5" href="#E5">E5.</a> Paste the following line at the end of the fstab file while **making sure the rest of fstab file is not modified**. Press <kbd>ctrl</kbd>-<kbd>x</kbd> and then <kbd>y</kbd>, and then <kbd>enter</kbd> to save the changes to the file. 
```
/swapfile swap swap defaults 0 0
```

<a id="E6" href="#E6">E6.</a> Enable the swapfile specified in `fstab`:
```
sudo swapon -a
```

<a id="E7" href="#E7">E7.</a> Verify the swapfile exists and is enabled:
```
sudo swapon --show
```

You should see an output like this:
```
NAME      TYPE      SIZE  USED PRIO
/swapfile file        2G    0B   -1
```


You now have a swapfile enabled and should be protected against out-of-memory errors.


