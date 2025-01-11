---
title: Recommended Linux Installation Guide
description: Sets up Foundry on linux with Caddy as reverse proxy.
published: true
date: 2025-01-11T15:10:35.635Z
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
* (Optional) A swapfile set up to help in low-RAM (2GB) instances.

## Important Information and Requirements

This guide assumes that you have already installed linux, and have at least a basic understanding of linux and how to use a commanbd line. 

The following is required to complete this guide:

1. An existing server with a modern linux distribution installed with a minimum of 2GB of RAM, for example:
a. A linux PC on your home network. *See note on [Port Forwarding](#port-forwarding)*
b. A linux virtual machine on any host.
c. A linux virtual private server or dedicated server in the cloud.
2. A basic understanding of using a terminal that includes the ssh utility, such as:
a. Powershell in Windows.
b. Terminal in Linux or macOS. 
3. Valid domain name, such as:
a. A purchased domain name from a registrar like [Namecheap](https://namecheap.com) or [gandi.net](https://gandi.net).
b. A free subdomain from a free domain name service like [Duck DNS](http://duckdns.org).

>The Foundry [minimum requirements](https://foundryvtt.com/article/requirements/) are 2GB RAM with 4GB recommended. Note that if you have other applications/services/etc that are using RAM in addition to Foundry, please consider more than 2GB. Essentially any CPU with at least one core should work. {.is-info}

## Port Forwarding
If you are planning to host on a PC on your home network, please double check that you are able to port forward. This guide assumes that you have already confirmed the ability to host on your home network, and set up port forwarding (if needed). 

>If hosting on a PC on your home network, confirm your ability to host before continuing with this guide. {.is-info}

See the Foundry documentation on [Port Forwarding](https://foundryvtt.com/article/port-forwarding/) for more info. You'll need to forward port 30000 (TCP) for just Foundry access, or 80 (TCP) and 443 (TCP + UDP) if using Caddy reverse proxy.

This guide does not cover port forwarding, nor is it an alternative to port forwarding. If hosting on your PC at home over IPv4, you will need to port forward.

## Preferred Linux Distribution

This guide includes instructions for a number of linux distributions, as described below. If you do not have a strong reason for another distribution, we recommend **Ubuntu 24.04**. 

This guide supports most distributions based on Debian 11 or 12, including:

1. Debian
2. Ubuntu
3. 64 bit Raspberry Pi OS 


Any distrition that uses the `apt` package managers *should* be compatible with this guide, as long as the [Distributions and Installations Types to Avoid](#distributions-and-installation-types-to-avoid) section is followed. 

## Distributions and Installation Types to Avoid

- **Amazon Linux 2**
This AWS-specific distribution does not support Node 18+, and also includes a number of changes and customizations that are specific to AWS and are out of scope of a simple Foundry hosting setup. 

- **NixOS** or similar containerized / declarative distributions
These distributions *may* work to host Foundry, but the instructions below will not work. They require specialized setup.

- Legacy or 32-bit **Raspberry Pi OS/Raspbian** 
Current 64-bit Raspberry Pi OS works and is recommended, but older or 32-bit versions have limited support for newer Node versions or could bump into limitations down the line. 

- Any distribution or setup that pre-installs a graphical web interface, admin management console, or other server management system
This guide is **NOT** compatible with setups that include **Plesk**, **Webmin**, **cPanel**, **DirectAdmin** or similar locked down systems intended to be configured by GUI only. 

>This guide requires 64 bit Debian 11 or 12 based distributions or higher. Using lower versions may not function properly. 32 bit OSs will have issues with RAM and NodeJS heap size. {.is-info}

## Getting Help
If you get stuck on a particular step, please first ensure that all commands in black text quotes entered *exactly* as they appear. If multiple commands are included in the black text quotes, copy and paste each line individually and ensure it completes before moving on to the next line.

Troubleshooting assistance for this guide can be found on the official Foundry Discord. Copy the link from the specific step number (ie: C5) you are having difficulty with and then post in the **#install-and-connection** channel on the [Foundry Discord](https://discord.gg/foundryvtt).

<a id="B" />

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

  
```
adduser foundry
usermod -aG sudo foundry

```


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

* `nodejs`, required to run Foundry itself
* `caddy`, the webserver that will be used as a reverse proxy
* `pm2`, the process manager that will keep Foundry running
* `unzip`, the utility used to decompress the Foundry installation zip archive
* `nano`, the text editor used to edit configuration files

>To continue, you must be using a non-root user with `sudo` access. If that is not the case, please review the steps in [User Setup](#user-setup). {.is-info} 

<a id="B5" href="#B5">B5.</a> First, let's update the system to make sure we have everything as up-to-date as possible. This may take a few minutes.


  
```
sudo apt update
sudo apt upgrade -y

```



>You may be asked for a password the first time you use a `sudo` command. This is normal. Enter the password for the user. {.is-info}

>If after entering the correct password, you receive an error: `<user> is not in the sudoers file` or similar, then you must login as **root** and complete ther [User Setup](#user-setup). {.is-warning}

<a id="B6" href="#B6">B6.</a> Add the `nodejs` v22 repository to the system package manager:


  
```
sudo apt install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
```



<a id="B7" href="#B7">B7.</a> Add the `caddy` repository to the system package manager:


  
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
```



<a id="B8" href="#B8">B8.</a> Install `nodejs`, `caddy`, `unzip`, and `nano`:

  
```
sudo apt update
sudo apt install nodejs caddy unzip nano -y
```



<a id="B9" href="#B9">B9.</a> Check that `nodejs` and `npm` are installed and the correct versions:

```
node --version
npm --version
```
`node` should return a version of 20 or greater. The `npm` version doesn't matter, but should return something. 

<a id="B10" href="#B10">B10.</a> Install `pm2`:

```
sudo npm install pm2 -g
```
<a id="B11" href="#B11">B11.</a> Add `pm2` to startup as the current user. Be sure to carefully read the `blue notice` and follow all instructions given:

```
pm2 startup
```
>***REQUIRED STEP*** 
>You will need to carefully review the output of the `pm2 startup` command. It will include a specific instruction on how to enable pm2 startup on your particular distribution. Copy and paste this command exactly. {.is-info}

<a id="C" />

# C. Foundry and Reverse Proxy Setup
## Objective
At the end of this section you will have a functional installation of Foundry using HTTPS and Caddy as a reverse proxy. Foundry will be set to restart any time the server is restarted, managed by pm2.

## Download, Install, and Test Foundry

<a id="C1" href="#C1">C1.</a> Login to [FoundryVTT](https://foundryvtt.com) and navigate to the **Purchased Licenses** page. 

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

>If you are installing on a Raspberry Pi, an ARM device or VM, or potentially some other UNIX OS and are seeing a GLIBC or DLOPEN error, see [section H](#H) in this guide. 
>
>You may also see a Deprecation Warning such as `[DEP0040] DeprecationWarning: The punycode module is deprecated. Please use a userland alternative instead.`. This warning can be safely ignored. {.is-warning}

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

>We recommend that you have a valid domain name with an A record pointing to `<public IP address>` to complete this section. If you do not have a domain name, you can use a service like [Duck DNS](http://duckdns.org) to get a free domain and point it to `<public IP address>`.  (see [guide](https://foundryvtt.wiki/en/setup/hosting/ddns) if you are hosting on a home network)
>
>Having a valid domain name results in an HTTPS connection without insecure connection warnings from your and your players' browsers. 
>
>If you do not have a domain name, you may continue with this section which will set up self-signed certificates for the bare IP connection to your host, allowing an HTTPS connection but one that will prompt browsers to show a warning before clicking through to connect.{.is-info}


<a id="C13" href="#C13">C13.</a> Run the following command to begin editing the Caddyfile:
  
  ```
  sudo nano /etc/caddy/Caddyfile
  ```
  

<a id="C14" href="#C14">C14.</a> Delete all the text, and replace it with (making sure to replace the `your.hostname.com` portion with your actual domain name, do **NOT** put `http://` or `https://` in front of it). If you do not have a domain name, leave the text as-is without modification. 

>You can delete text in `nano` by using the arrow keys to move the cursor and pressing the <kbd>delete</kbd> or <kbd>backspace</kbd> keys to delete text. {.is-info} 

>Do not modify the `https:// { ... }` block at all, whether you have a domain name or not. {.is-warning}
  
  ```
  # This replaces the existing content in /etc/caddy/Caddyfile

  # A CONFIG SECTION FOR YOUR HOSTNAME
  
  your.hostname.com {
      reverse_proxy localhost:30000
      encode zstd gzip
  }
  
  https:// {
      tls internal {
      		on_demand
      }
      
      reverse_proxy localhost:30000
      encode zstd gzip
  }

  # Refer to the Caddy docs for more information:
  # https://caddyserver.com/docs/caddyfile
  ```
  


<a id="C15" href="#C15">C15.</a> Press <kbd>ctrl</kbd>-<kbd>x</kbd> then <kbd>y</kbd> and <kbd>enter</kbd> to save your changes.

<a id="C16" href="#C16">C16.</a>	Restart Caddy to pick up the new settings by:
```
sudo service caddy restart
```
>Caddy handles all forwarding to HTTPS as well as the encryption certificates. No further configuration is needed to get those working. {.is-info}

<a id="C17" href="#C17">C17.</a> Tell Foundry that we are running behind a reverse proxy by changing the `options.json` file. Open the file for editing by:
```
nano ~/foundryuserdata/Config/options.json
```
<a id="C18" href="#C18">C18.</a> Find the `proxySSL` and `proxyPort` parameters, and change them as below. Leave all other options as they are. The `hostname` parameter will tell Foundry to use a hostname in the Internet Invite Link. Replace `<your.domain.name>` with your actual domain name, if you have one. If not then do not modify the `hostname` field.
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

<a id="C19" href="#C19">C19.</a> Press <kbd>ctrl</kbd>-<kbd>x</kbd> then <kbd>y</kbd> and <kbd>enter</kbd> to save your changes.

<a id="C20" href="#C20">C20.</a> Restart Foundry to pick up the changes to configuration: 

```
pm2 restart foundry
```

<a id="C21" href="#C21">C21.</a>	Test your site by opening a new browser tab to `http://your.domain.name` or `http://server.internal.IP.address`. Your browser may show a warning when connecting to the bare IP though you should be able to click through it. If everything is working, you will see Foundry load and the site will have the encrypted lock icon. It is now ready for use and no further configuration is needed. 

>Sometimes DNS records can take a few minutes and up to a couple hours to be recognized across the internet. If you receive an error along the lines of `server IP address could not be found` or `having trouble finding that site` then the DNS records may just need more time. Wait a few minutes and try again. 
> 
>If the connection is refused, you may need to open ports 80 and 443 in your OS firewall and then either your provider's network security settings (if cloud hosted) or through port forwarding on your router (if hosting at home). {.is-warning}

>If you are hosting on your home network, you **must** use an external device to test the connection to the domain name. You can only test the connection to the internal IP address from within the network. {.is-warning}

> This concludes the portion of the guide that sets Foundry up and running. You may now continue using Foundry this way without issue going forward. {.is-info}
<a id="D" />

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
<a id="E" />

# (Optional) E. Creating Swapfile
## Objective
The minimum RAM requirement for hosting Foundry is 2GB (4GB recommended), however some systems or modules may use more than the minimum RAM. If your linux host has 2GB of RAM you can create a swapfile to prevent out-of-memory errors when using heavier modules, systems, or large compendiums. 

## Create and Enable Swapfile
The instructions below are compatible with the <a href="#preferred-linux-distribution">preferred linux distributions</a>.

All commands below are assumed to be entered by a non-root sudoer user, such as the `foundry` user created in <a href="#B1">B1</a> to B4. 

<a id="E1" href="#E1">E1.</a> Create a file to be used as swap:
```
sudo fallocate -l 2G /swapfile
```
>This will create a 2GB swapfile which is a recommended size for hosts with 2GB of RAM. You can increase this size as you'd like, but it is not recommended to create a smaller swapfile. {.is-info}

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
<a id="F" />

# (Optional) F. Updating NodeJS
## Objective

As Foundry VTT is updated, the minimum requirements for NodeJS are also updated. If you've received a message stating that you must update NodeJS and you have used this guide (or similar guides, such as the Oracle Always Free guide, that use pm2 and the nodesource repo) then this section will describe how to update NodeJS to the latest version. 

## Updating NodeJS
This section assumes that you have set Foundry VTT to be managed by pm2 and have installed NodeJS through their repo as this guide describes. Please copy and paste the instructions carefully. 

<a id="F1" href="#F1">F1.</a> Stop any managed pm2 processes.

```
pm2 stop all
```

<a id="F2" href="#F2">F2.</a> Remove the current pm2 from startup to allow for the upgrade. 

```
pm2 unstartup
```

<a id="F3" href="#F3">F3.</a> Add the new NodeJS version repository and update the installed version of NodeJS.

```
sudo apt install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt update
sudo apt upgrade
```

<a id="F4" href="#F4">F4.</a> Set pm2 to use the upgraded version of NodeJS and set it to run on start again.

```
npm rebuild -g pm2
pm2 startup
```

<a id="F5" href="#F5">F5.</a> pm2 will list a command to run after the `pm2 startup` was run. Copy and paste that into the commandline and run it to ensure pm2 will launch on startup. 

<a id="F6" href="#F6">F6.</a> Restart any previously running pm2 managed processes. 

```
pm2 start all
```

<a id="F7" href="#F7">F7.</a> Check that Foundry is online. The output should show a green `online` indicator beside the Foundry process. 

```
pm2 list
```

<a id="F8" href="#F8">F8.</a> Save the current pm2 configuration.

```
pm2 save
```

You've now successfully updated NodeJS and should be good to go! 

You may want to reboot your instance to apply all updates, such as a kernel update. 
<a id="G" />

# (Optional) G. Performing a Clean Reinstall/Update
## Objective

This will guide you through the steps needed to clear a current installation, and download the install a fresh version of Foundry - either an update or the same version. Your userdata will not be touched or affected at all.

## Archiving Current Install and New Install
Assuming that you have Foundry installed in `~/foundry` and your userdata in a separate location (likely `~/foundryuserdata`) and is managed by `pm2`. 

<a id="G1" href="#G1">G1.</a> Once you have connected and navigated to your home directory `~`, stop Foundry using pm2. 

```
pm2 stop foundry
```

<a id="G2" href="#G2">G2.</a> Create a backup/archive of the current installation by moving the folder to a new location. You may need to name the destination folder something like `foundry-archive-2023-06-02` in the case of multiple updates. 

```
mv foundry foundry-archive
```

<a id="G3" href="#G3">G3.</a> Create the installation directory and download desired Foundry version using wget. You must use the Timed URL, and the Linux version. Be sure to wrap the URL with quotes below.

```
mkdir ~/foundry
wget --output-document ~/foundry/foundryvtt.zip "<download url>"
```

<a id="G4" href="#G4">G4.</a>	Once downloaded, extract Foundry and cleanup the zip file:

```
unzip ~/foundry/foundryvtt.zip -d ~/foundry/
rm ~/foundry/foundryvtt.zip
```

<a id="G5" href="#G5">G5.</a> Restart Foundry using pm2. 

```
pm2 start foundry
```

<a id="G6" href="#G6">G6.</a> Check your node version using `node -v` against the [minimum requirements](https://foundryvtt.com/article/requirements/#dedicated-server). Head to [section F. Updating NodeJS](#F) if you need to update Node.js.

>If you are installing on a Raspberry Pi, an ARM device or VM, or potentially some other UNIX OS and are seeing a GLIBC or DLOPEN error, see [section H](#H) in this guide. {.is-warning}

You should now have the new version of Foundry running and accessible as before!
<a id="H" />

# (Optional) H. GLIBC Error Fix
## Objective

This section addresses a common error on ARM or other non-x86/x64 architectures, including some Raspberry Pis. If you encounter the GLIBC error listed below, please read through this section to correct it and have Foundry launch. 

## Foundry Fails to Launch with GLIBC Error

This section of the guide will correct the error where Foundry fails to launch, and you see this error (exact GLIBC version listed may be different):
```
Error: /lib/arm-linux-gnueabihf/libstdc++.so.6: version `GLIBCXX_3.4.29' not found (required by /home/foundry/foundry/resources/app/node_modules/classic-level/prebuilds/linux-arm/node.napi.armv7.node)
```

## Rebuild Classic-Level Database Engine

The GLIBC error is caused by the `classic-level` node package not having a compiled version distributed for glibc versions on some ARM or non-x86/x64 archictectues. You may also see this error if you are using a non-linux POSIX OS like FreeBSD or are using an older linux distribution. 

To correct this error, we need to build `classic-level` ourselves manually. 

The steps below assume you've followed the guide above to install and run Foundry, and you are logged in as the `foundry` user. If you didn't folow this guide, adjust accordingly to your own installation.

<a id="H1" href="#H1">H1.</a> Login as the user Foundry is running as and stop Foundry from running. 

```
pm2 stop foundry
```

<a id="H2" href="#H2">H2.</a> Change to the installation `resources/app/` directory. 

```
cd ~/foundry/resources/app
```

<a id="H3" href="#H3">H3.</a> Run the `npm` command to build `classic-level` under the current glibc version and architecture.

```
npm install classic-level --build-from-source
```

>You MUST execute that command in the `resources/app/` directory in the Foundry installation. This fix will fail if you don't execute it in that directory.. {.is-warning}

<a id="H4" href="#H4">H4.</a> Relaunch Foundry.

```
pm2 start foundry
```

Foundry should now start properly without the GLIBC error!