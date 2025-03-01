---
title: FreeBSD
description: 
published: true
date: 2025-02-19T19:28:59.042Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:25:57.469Z
---

>This article was automatically imported from the previous wiki and may contain inaccurate or outdated information.
{.is-warning}

# Foundry 11 and later

The officially supported platforms for Foundry VTT are Linux, macOS and Windows.

As of Foundry 11, it no longer works on FreeBSD out of the box. This is likely because FreeBSD does not ship with `glibc`, it uses `libc` instead.

> In testing on TrueNAS Core, the below resolution did not work. Foundry v11+ might just not work on FreeBSD.

> Conversely, testing this for Foundry v12 on FreeBSD 14.2, the instructions worked.

This can be resolved by:
1. Shut down your currently hosted Foundry VTT server instance. 
2. In a terminal, access the folder or directory where your Foundry VTT executable is located. 
3. Access the `resources/app/` subdirectory. 
4. run `npm install classic-level --build-from-source`
5. Relaunch your Foundry VTT server following your normal procedure.
6. Enjoy FreeBSD. :)

# Base install directly on FreeBSD, not for NAS

Download the Linux version of Foundry and place it in a suitable location, such as **~/FoundryVTT**. Extract the contents (you may need to install zip using `pkg install zip`). The zip does not contain a top-level directory, so be sure you extract into an empty directory.

In order to run the Foundry server we need to install `node.js`. In a terminal run `pkg install node`.

With `node.js` installed we can now run the Foundry server. Assuming you placed and named the extracted Foundry folder in your home directory and named it **FoundryVTT** you can type the following command replacing the folder names if needed
```
cd ~/FoundryVTT
```
Press the `return` key.

Now that we are inside the Foundry folder we can run the Foundry server by running this command
```
node resources/app/main.js
```
Press the `return` key.

You should now see something similar to this in your terminal
```
Foundry VTT | 2019-10-22 09:00:40 | [info] Foundry Virtual Tabletop - Version 0.3.8
Foundry VTT | 2019-10-22 09:00:40 | [info] Application Options:
{
  "port": 30000,
  "fullscreen": false,
  "isElectron": false,
  "isNode": true,
  "isSSL": false,
  "world": null,
  "background": false,
  "debug": false,
  "demo": false
}
Foundry VTT | 2019-10-22 09:00:40 | [info] Requesting UPnP port forwarding to destination 30000
Foundry VTT | 2019-10-22 09:00:40 | [info] Server started and listening on port 30000
```

With the Foundry server running you can now access it via your browser by typing `localhost:30000` in the URL bar.

To shut the Foundry server down press both the `control` + `c` keys at the same time with the terminal in focus.

You can save on some disk space by deleting the Linux-specific binary files.  You can delete the unneeded ones with the following command:

<!--- {% raw %} --->
```
rm -rf chrome* *.dat *.so locales *.bin *.pak swiftshader
```
<!--- {% endraw %} --->

# Alternative: Install in a jail, with a reverse proxy and SSL certificates

While running Foundry on the base OS works, there are advantages to having it in a jail, with a reverse proxy in front for https:// access, and the ability to run more than one Foundry instance on the same server.

For the following, I am describing my own setup. Adjust these instructions to your setup as needed.
- I am using FreeNAS / TrueNAS Core as the server. Where you see references to "UI", the same steps can be done from command line on a base FreeBSD server.
- I am running this in my home, which means my IP address is dynamic, and so is the DNS for it. Because of this, I am using dynu as a DNS provider. Any other DNS provider will, of course, work.
- Assuming you, too, will run this in your home, verify that you are not behind CGNAT, *first*.
- I am providing instructions for installing a traefik reverse proxy, and link to instructions for caddy and nginx. A reverse proxy is optional: It does have the advantage of being able to run more than one Foundry instance - each with its own license - in your home, and it does make it easy to enable https, which is a requirement for audio and video.
- I am assuming use of the JitsiRTC module, and thus do not have any instructions for running the A/V server in your home. With Jitsi, you'd either use their public server or set up a private Jitsi server in the cloud. This does have the advantage of being light on your upstream bandwidth, your players will have better quality.
- I did not get IPv6 to work with the dynu update client. While I reference IPv6 here and there, all instructions assume that your home is reachable via IPv4 and not behind Carrier Grade NAT.
- Where I type something in all upper case, it is a place holder, to be replaced with a real value from your environment.

## Checking for CGNAT if hosting at home / on consumer ISP

CGNAT, Carrier Grade NAT, means you do not have your own public IPv4 address, and players outside your home won't be able to reach your Foundry server via IPv4. If that is your situation, use a hosting service such as https://forge-vtt.com/ instead or host your own instance. One set of good instructions is at https://benprice.dev/posts/fvtt-docker-tutorial/.

You are *not* behind CGNAT if the outside address of your Internet gateway is also the public address shown to the Internet. There are three ways to check this.

From Windows Powershell, run the following command. You expect to see only one hop for the first traceroute, and several for the traceroute to 8.8.8.8. If you see several hops for the first traceroute, you either have an unusual home setup with multiple routers, or you are behind CGNAT.

```
tracert (curl https://ifconfig.me/ip -UseBasicParsing); tracert 8.8.8.8
```

If you are on MacOS or Linux or BSD, same idea, but use this command in a Terminal instead

```
traceroute $(curl https://ifconfig.me/ip); traceroute 8.8.8.8
```

An alternate way to verify this is to log into the configuration dashboard of your Internet gateway, and check that the external IP address matches the address you see when you browse to https://ifconfig.me/ip .

If you are behind CGNAT and you as well as *all* of your players have IPv6 configured, then these instructions will still work. You will however need to find a replacement for ddclient to update your dynamic DNS, until ddclient becomes fully v6-capable.

## Install FoundryVTT in a jail
### Initial jail creation
Create a new jail from UI, assuming internal network is IPv4 or dual-stack IPv4/IPv6

Give the jail a name, make it a base jail, choose a release, and hit next.

For options in this screen:
- VNET checked
- DHCP, NAT and Berkeley Packet Filter unchecked
- IPV4 interface blank
- IP address, netmask, ipv4 default router
- Autoconfigure IPv6 won't be used if you are going to use a reverse proxy, leave unchecked
- Auto-start

And create the jail.

### Install PM2 in the jail so Foundry can auto-start on jail start

SSH into FreeNAS, using ssh from WSL or PowerShell, or PuTTY, or your SSH client of choice.

From the FreeNAS CLI:
```
iocage console FOUNDRY-JAIL-NAME
```

From the jail CLI:

```
pkg install node unzip npm ca_root_nss
```

For an overview of PM2 see https://pm2.keymetrics.io/docs/usage/quick-start/

```
npm install pm2@latest -g
mkdir -p /usr/local/etc/rc.d
pm2 startup
```

Let's make sure packages are kept up to date once a week on Sunday

```
crontab -e
```

Hit "i" for insert, and copy this line and right-click to paste it:

```
18 3 * * 0 pkg upgrade -y && npm install pm2@latest -g && pm2 update
```

Hit ESC to exit insert mode

Type :x and Enter to save the changes

Verify that the changes are in:

```
crontab -l
```

### Install FoundryVTT itself

#### Optional but recommended, keep foundry data and config in its own dataset.

From the jail CLI:
```
mkdir /config
```

From UI:
- Stop jail
- Create dataset (share type generic, case sensitivity insensitive) for foundry config and data
- Create mountpoint in jail where new dataset is mounted into /config
- Start jail

#### Download and transfer the FoundryVTT application

Back into the jail CLI via
```
iocage console foundry-jail-name
```

Create a foundry user

```
pw user add -n foundry -c 'Foundry VTT' -d /home/foundry -m -s /usr/srbin/nologin
```

If you chose to have configuration in /config, make the foundry user owner

```
chown -R foundry:foundry /config
```

Now let's install and start Foundry. Click the link (chain icon) for the node.js package from your downloads at foundryvtt.com, under your login and then "Purchased Licenses". 

And then from the jail CLI as below. Note the quotes around PASTE-FOUNDRY-LINK, they are required.

```
cd /home/foundry
mkdir fvtt
fetch "PASTE-FOUNDRY-LINK" -o foundryvtt.zip
unzip foundryvtt.zip -d fvtt
chown -R foundry:foundry fvtt
```

Now let's start Foundry, then adjust the config file.

```
pm2 start fvtt/resources/app/main.js -u foundry -- --headless --dataPath=/config
```

Make sure Foundry starts when the jail starts

```
pm2 save
```

Foundry is now up and running, you can enter a license by navigating to http://FOUNDRY-JAIL-IP:30000/, and create a World if you like.

If you'd like your users to be able to reach you via a URL, and have the option of using Video - as well as Audio if desired - read on for reverse proxy configuration.

## Optional but recommended: Configure FoundryVTT for use with a reverse proxy such as traefik, caddy or nginx
### Prepare FoundryVTT to use a reverse proxy

From the Foundry jail CLI:
```
pm2 stop 0
ee /config/Config/options.json
```
Change the following fields:
```
"upnp": false,
"hostname": "DOMAIN-FOR-FOUNDRY",
"proxySSL": true,
"proxyPort": 443,
```
Exit and save (ESC and Enter twice)

Restart Foundry
```
pm2 start 0
```

Foundry is still accessible on http://FOUNDRY-JAIL-IP:30000/ at this point, but of course not yet via reverse proxy / DNS. 
If you are already running a reverse proxy in a separate jail, simply add the DNS name and FOUNDRY-JAIL-IP:30000 to its configuration.

If you are not, read on for how to configure traefik, or use the guides for Caddy (https://www.ixsystems.com/community/resources/reverse-proxy-using-caddy-with-optional-automatic-tls.114/) or NGINX (https://www.ixsystems.com/community/resources/how-to-set-up-an-nginx-reverse-proxy-with-ssl-termination-in-a-jail.132/) respectively.

### Install traefik as a reverse proxy in a jail
#### Dynamic DNS setup

First, you will need a DNS name. Assuming you are running this at home, that'll be a dynamic DNS provider. I am using dynu in this example, but any other supported provider will work. In this example, I'll use a wildcard cert, which means DNS01 validation. Feel free to adjust this if that's not your plan. See https://docs.traefik.io/https/acme/ for a list of supported DNS providers. Pay attention to the Environment Variables there, you will need these when setting up traefik.

For this example, create an account at www.dynu.com. Under DDNS Services, create a domain name - in this example, I'll use one of theirs, but you could also use one you own yourself - then set it to have a Wildcard IPv4 alias and, if you are going to make traefik available via IPv6, a Wildcard IPv6 alias and Enable IPv6 Address. Save.

Now go back to Control Panel, choose API Credentials. If there is no API Key, Generate API Credentials. You will need this API key during traefik setup.

#### Traefik installation
To make installing traefik as easy as possible, I am going to use jailman from https://github.com/jailmanager/jailman

From FreeNAS command line via SSH, NOT the Foundry jail CLI:

```
cd
git clone git@github.com:jailmanager/jailman.git
cd jailman
```

As of 6/30/2020, traefik is still in the dev branch. Run ```ls blueprints```: If there is a traefik directory in there, you are good to go. If not, switch to the dev branch first.

```
git checkout dev
```

Now let's set parameters for the new traefik jail.

```
cp config.yml.example config.yml
ee config.yml
```

At the top in the ```global:``` section, set the ```config dataset``` location. Replace ```tank``` with the name of your pool. If the dataset doesn't exist, it will be created. You can also set ```media``` and ```downloads```, although these are not used for the traefik jail.

- Find the ```traefikjail:``` section and edit to fit your environment. 
- By default the jail will be named ```traefikjail```, you can change this name. 
- Set a static ```ip4_addr``` and a ```gateway```. 
- Set ```domain_name``` to the name the traefik dashboard should be reachable as - this is where that wildcard from earlier comes in handy. traefik.MY-DOMAIN is an easy choice. 
- Set ```dns_provider``` to the one you chose, ```dynu``` in this example.
- Set ```cert_staging``` to ```true```, so that you can verify that Let's Encrypt cert issue works and not run into rate limiting
- Set ```cert_email``` to the email address you want to use with Let's Encrypt
- Set ```cert_wildcard_domain``` to YOUR_DOMAIN if you chose to use a wildcard domain earlier, or delete it if you are not going to use a wildcard domain
- The entries in ```cert_env``` are for Cloudflare by default. Replace these with the right entries for your DNS provider. In the case of dynu, it will be ```DYNU_API_KEY:``` and the API Key from the dynu.com site.

Now you are ready to install traefik. If you renamed the jail, use that name instead:

```
./jailman.sh -i traefikjail
```

#### Initial Traefik configuration

Log into console of the new traefik jail
```
iocage console traefikjail
```

Update traefik2 so it supports dynu - optional if traefik is already 2.2 or later or you use a provider that doesn't require traefik2.2.

```
mkdir -p /usr/local/etc/pkg/repos/
cp /etc/pkg/FreeBSD.conf /usr/local/etc/pkg/repos/
ee /usr/local/etc/pkg/repos/FreeBSD.conf
```

Change quarterly to latest so it reads:

```
FreeBSD: {
   url: "pkg+http://pkg.FreeBSD.org/${ABI}/latest",
```

Hit ESC and then Enter twice to exit the editor and save the changes.

And bring in the new version of Traefik.

```
pkg upgrade -y
service traefik restart
```

Let's make sure packages are kept up to date once a week on Sunday.

```
crontab -e
```

Hit "i" for insert, then copy this line and right-click to paste it:
```
18 3 * * 0 pkg upgrade -y && service traefik restart
```

Hit ESC to exit insert mode

Type :x and Enter to save the changes

Verify that the changes are in:

```
crontab -l
```

Note that dynu does not work well with the way wildcard certificates are configured in ```dashboard.toml```. If you are using a wildcard domain and a DNS provider that does not work well with the default, swap it around thusly in that file:

```
        [http.routers.traefik.tls]
          certResolver = "default" # From static configuration
          [[http.routers.traefik.tls.domains]]
            main = "*.MYDOMAIN"
            sans = ["MYDOMAIN"]
```

Now verify that the certificate generation is working with the Let's Encrypt staging server.

```
cd /config
cat acme.json
```

It may take a minute or so, you expect the "Certificates" section to contain a "certificate" and "key".

If this isn't working you likely have an issue with the DNS provider you chose, assuming you are doing DNS verification. Since there is no http server running in this jail, I haven't looked into other forms of verification.

If you have issues, check traefik.log for error messages, and verify that the environment variables hold the right API keys and such for your provider: Check that with ```sysrc -a```.

#### Install a client to keep the dynamic DNS entry updated

If you are using a dynamic DNS provider, you'll want to ensure that the DNS records are updated with your IP address periodically. This will differ from provider to provider. You can of course run the client on a Windows or MacOS device in your network, but where's the sense in doing that when you have a server. Here are the steps for dynu.

Download the DDClient and its configuration file from https://www.dynu.com/en-US/Resources/Downloads .

Using WinSCP or command line, copy the downloaded files into your jail. Example using scp from WSL (Windows Subsystem for Linux):

```
scp /mnt/c/Users/USERNAME/Downloads/ddclient-VERSION.zip root@FREENAS-IP:/mnt/DATASTORE/iocage/jails/TRAEFIK-JAIL-NAME/root/root
scp /mnt/c/Users/USERNAME/Downloads/ddclientconf.zip root@FREENAS-IP:/mnt/DATASTORE/iocage/jails/TRAEFIK-JAIL-NAME/root/root
```

And get back into the traefik jail CLI:

```
iocage console traefikjail
```

Let's install perl5 and get ddclient running.

```
pkg install perl5 p5-Data-Validate-IP p5-IO-Socket-SSL p5-JSON-PP
unzip ddclient-VERSION.zip
unzip ddclientconf.zip
```

Let's install ddclient:

```
ee ddclient-VERSION/ddclient
```

Delete the first line that is looking for /usr/bin/perl, and save the file.

```
cp ddclient-VERSION/ddclient /usr/local/sbin/
mkdir /etc/ddclient
mkdir /var/cache/ddclient
cp ddclient.conf /etc/ddclient/ddclient.conf
```

Edit the configuration file:

```
ee /etc/ddclient/ddclient.conf
```

Set ```login=``` to your dynu username, ```password=``` to your dynu password, and replace ```mydomain.dynu.com``` with your domain. Set ```daemon=300``` so updates are run every 5 minutes, not every minute.

In my testing, I was not successful in updating both IPv4 and IPv6 addresses. I'll stay with IPv4 for this example.

Let's test that this is working:

```
ddclient -daemon=0 -debug -verbose -noquiet
```

Now let's set this to run automatically

```
ee /usr/local/etc/rc.d/ddclient
```

Paste this:
```
#!/bin/sh

# PROVIDE: ddclient
# REQUIRE: LOGIN
# KEYWORD: shutdown
#
# Add the following lines to /etc/rc.conf.local or /etc/rc.conf
# to enable this service:
#
# ddclient_enable (bool):       Set to NO by default.
#                               Set it to YES to enable ddclient.

. /etc/rc.subr

name=ddclient
rcvar=ddclient_enable
ddclient_conf="/etc/ddclient/ddclient.conf"

command="/usr/local/sbin/${name}"
load_rc_config $name

delay=$(grep -v '^\s*#' "${ddclient_conf}" | grep -i -m 1 "daemon" | awk -F '=' '{print $2}')

if [ -z "${delay}" ]
then
        ddclient_flags="-daemon 300"
else
        ddclient_flags=""
fi

run_rc_command "$1"
```

Esc and Enter twice to save.

Let's start the ddclient service we just created.

```
chmod 555 /usr/local/etc/rc.d/ddclient 
sysrc ddclient_enable=yes
service ddclient start
```

#### Test traefik

Before we go any further, let's test that the dashboard is working. Configure your router to port-forward ports 80 and 443 to the static IP of your traefik jail, and verify that you can access the dashboard by the name you gave it in config.yml, for example traefik.MYDOMAIN. How you do this on your router / firewall / Internet gateway is out of scope of this article, and if your router has trouble with "hair-pinning", you may need to test first from external - for example with a smartphone on LTE, not WiFi - and then resolve the hair-pinning issue.

I am aware that this is often one of the hairier parts of getting this to work, if you haven't forwarded a port before. It's a necessary step, and there will be forums for your router manufacturer that will guide you. See also "Players can't connect" at https://prezi.com/p/te97qzy1rlcs/foundry-vtt-troubleshooting/ .

Note there is no authentication on the dashboard yet, we'll come back to that.

#### Add Foundry to traefik

Now let's add the foundry server to traefik.

```
cd /config/dynamic
ee foundry.toml
```

Add all of this, taking care with the back ticks and that you use the right domain name and static IP of the foundry jail:

```
[http]
  [http.routers]
    [http.routers.foundry]
      rule = "Host(`DOMAIN-FOR-FOUNDRY`)"
      service = "foundry"
      entryPoints = ["web-secure"]
      [http.routers.foundry.tls]
  [http.services]
    [http.services.foundry.loadbalancer]
      [[http.services.foundry.loadbalancer.servers]]
        url = "http://IP-OF-FOUNDRY-JAIL:30000"
```

Esc, Enter twice to save.

And test that. You should be able to reach DOMAIN-FOR-FOUNDRY from a web browser. It should be encrypted, and you expect that the certificate is untrusted, since we're still on the Let's Encrypt staging server. This can be your base domain, by the way, if you used wildcards and you'd like this to be as short as possible.

Note that if you're going to run a second foundry jail for a buddy - of course you are, once you have this running - you'll want to create a separate toml file, with all instances of ```foundry``` replaced by something like ```foundry2``` or ```foundry-buddy```. That way, traefik knows which is which. If you used wildcards, then you can give any name to your buddy's foundry server, such as buddy.MY-BASE-DOMAIN. They'll need their own Foundry VTT license for this, of course.

#### Switch to production certificates

Everything's working, it's time to switch over to real Let's Encrypt certificates.

```
service traefik stop
rm /config/acme.json
ee /config/traefik.toml
```

Find the ```caServer``` line and add a ```#``` character before it to comment it out.

Esc and Enter twice to save.

```
service traefik start
```

Check ```/config/acme.json``` again, you expect it to populate with a real Let's Encrypt cert after a minute or so. If that's failing, check ```traefik.log``` for error messages related to Let's Encrypt.

That was a lot. **Pour yourself a beverage of choice, you deserve it**. You're not just a GM, but a GM with serious networking and server chops.

Note: You may notice there is no mention here of how to forward ports for video. That's because I'm assuming use of the JitsiRTC module, with either the public JITSI server or a private one you're hosting in the cloud somewhere. That way, you're not on the hook for having the bandwidth in your house to provide a good video experience to all your players.

#### Optional, recommended: Secure the traefik dashboard

Okay. What about that traefik dashboard? That's visible to everyone on the Internet who knows the URL now, and can it be secured? Of course it can.

Let's just add some basic auth for the dashboard.

You need a username, a password, and a bcrypt hash of that password. Since FreeBSD doesn't have htpasswd by default and installing apache for it is not 
desirable on a reverse proxy jail, let's do this with Python.

```
pkg install py37-pip
pip install bcrypt
python3.7 -c 'import bcrypt; print("\"USERNAME:", bcrypt.hashpw(b"PASSWORD", bcrypt.gensalt(rounds=10)).decode("ascii"),"\"",sep="")'
```

Copy the output of that into clipboard.

Create ```/config/dynamic/auth_middlewares.toml``` with ee or editor of choice, you know this drill by now.

```
[http.middlewares]
   [http.middlewares.auth.basicAuth]
     users = [
       "USERNAME:PASSWORDHASH"
    ]
```

Where ```"USERNAME:PASSWORDHASH"``` is what the Python command gave you earlier, so just paste that in.

You can, if so desired, add a ```,``` to the end of that line, and have a second user and password on the next line, and so on.

Then edit ```dashboard.toml``` and add, right below ```entryPoints```:
```
middlewares = ["auth"]
```

And done, you should be prompted now for username and password before you can access your dashboard. Save the username and password in your password vault of choice - I use Bitwarden - so you don't have to remember them.
