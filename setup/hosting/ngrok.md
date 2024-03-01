---
title: ngrok
description: A guide to allow your players to connect to your Foundry server
published: true
date: 2024-03-01T23:19:10.443Z
tags: hosting, networking, free, self-hosting, https, server
editor: markdown
dateCreated: 2024-03-01T22:30:04.263Z
---

# Hosting with ngrok
ngrok is a free tool that allows you to easily put applications on the internet with https. You can use ngrok on your home desktop or laptop (Windows, Mac, or Linux), a cloud-hosted server (like AWS, Azure, GCP, etc.), a home server, or even a [Raspberry Pi](/en/setup/hosting/raspberry-pi).

At the end of this guide, you'll be able to access your Foundry VTT from any device in the world, no matter where you've installed Foundry, with a URL like:

`https://giraffe-welcomed-instantly.ngrok-free.app`

Optionally, you can run ngrok as a service, so that it starts up whenever your server or computer restarts, inside a docker container for further isolation from your host machine, or with kubernetes as an ingress controller (ngrok is currently working to support the new "Gateway" spec as well).

> ngrok can be used for free with any number of players able to connect to your Foundry server. There are some additional features you can unlock with a paid version of ngrok, such as configurable domains.
{.is-info}

> Recently ngrok has introduced bandwidth limits for the free tier. If you play a lot, have a lot of players, or have many large assets in your game, you may encounter these limits and need to upgrade to a paid plan.
{.is-warning}

In this guide, we'll assume you've installed Foundry VTT on a machine somewhere and that you do not have an ngrok account. We'll cover how to install and configure ngrok, how to have ngrok automatically start so you don't need to remember, and how to configure Foundry to use ngrok as a proxy.

> **Make sure Foundry VTT is running on your computer or server and has a Game World launched before you start.**
{.is-success}

## Installing ngrok

First, you'll want to create an ngrok account if you don't have one already. To do so, visit [https://dashboard.ngrok.com/signup](https://dashboard.ngrok.com/signup) and enter your information or sign up with a provider like Google or Github.

![ngrok-sign-up-page.png](/setup/hosting/ngrok/ngrok-sign-up-page.png)

Once you've signed up, you should be in ngrok's Dashboard. You should see instructions for how to install ngrok's Agent on MacOS, Windows, Linux, Docker, a RaspberryPi, FreeBSD, etc. Rather than repeating those instructions here where they will become outdated, please follow the installation instructions in the ngrok Dashboard for the system where you have Foundry running.

![ngrok-welcome-page.png](/setup/hosting/ngrok/ngrok-welcome-page.png)

> You only need to follow the _Installation_ instructions (downloading ngrok and configuring it with your auth token. You do not need to follow the additional instructions for "deploying your app" and "securing your endpoint". We'll walk through configuring ngrok and Foundry in the remainder of this guide.
{.is-info}

## Configuring your ngrok domain

This will be the domain that you and your players can use to access your Foundry server from the internet. ngrok allows you to have a single domain for free, but first you need to claim it. In the navigation bar to the left of the screen, click on "**Domains**" (under "**Cloud Edge**"). You should see something like:

![ngrok-claim-domain.png](/setup/hosting/ngrok/ngrok-claim-domain.png)

Click the "**+ Create Domain**" button, which will automatically create a static domain for you.  It will be three random words, followed by one of ngrok's free domains. For example:

`giraffe-welcomed-instantly.ngrok-free.app`

> If you want to customize your domain name you need to pay for ngrok. More details on ngrok's pricing can be found on their [pricing page](https://ngrok.com/pricing).
{.is-info}

Once you claim your domain, ngrok will show you how to "Start a Tunnel" for that domain. We'll close out of that for now. It will also show more information about the Domain that was created, such as an ID, Region, when it was created, etc. If you want, you can add a Description such as "Foundry", but that's optional.

![ngrok-domain-info.png](/setup/hosting/ngrok/ngrok-domain-info.png)

We'll close out of that information for now too.

## Starting ngrok

Next, we'll start ngrok and create a "tunnel". An ngrok tunnel is like a pipe from your computer or server to the internet. 

First we'll go back to that "Start a Tunnel" screen (yes we just closed it, but now you see how to open it whenever you want).

![ngrok-domain-start-tunnel.png](/setup/hosting/ngrok/ngrok-domain-start-tunnel.png)

Next, you'll want to determine how you want ngrok to run. There are a few options available depending on your desired outcome:

1. **Start a tunnel from the command line**
  a. Simple and straightforward
  b. You will need to do this every time your computer or server restarts
2. **Start a tunnel from a config file**
  a. You can enable ngrok to start automatically even if your computer or server restarts
  b. A little trickier, but still straightforward
3. **Start a tunnel with a Docker container**
  a. Lots of optionality with Docker
  b. Only recommended if you're using [Foundry with Docker](/en/setup/hosting/Docker) as well
  
> For this guide, we'll walk through the second option, "starting from a config file," and configure ngrok to automatically run when the host machine restarts. 
{.is-info}

### Starting a tunnel from a config file

Select the "Start a tunnel from a config file" option in the dropdown. You'll see some instructions, which we'll mostly follow here. You'll need to use your command line (terminal on MacOS, command prompt on Windows).

#### 1. Run `ngrok config edit` in your command line

> If you get an error, you want to make sure ngrok is installed properly.
{.is-info}

This will open a default editor and edit the default ngrok configuration file. You should see a configuration file with your authtoken:

```yaml
version: "2"
authtoken: <DO-NOT-EDIT-YOUR-AUTHTOKEN-HERE>
```

Do not edit your authtoken. If it is not there, you did not configure ngrok yet. You can find your authtoken in the Dashboard under "**Getting Started**" > "**Your Authtoken**" in the lefthand navigation.

#### 2. Add a tunnel for Foundry by adding the following to the end of your configuration file

```yaml
tunnels:
  foundry:
    proto: http
    hostname: <your-ngrok-domain-here>
    addr: 127.0.0.1:30000
```

Replace `<your-ngrok-domain-here>` with whatever [domain you claimed above](#configuring-your-ngrok-domain).

This configuration tells ngrok to start a tunnel called "foundry" using the "http" protocol with your hostname. The "addr" field is where your foundry server lives and where ngrok will send the Internet traffic. `127.0.0.1`	is a special IP address that means "this computer" so you don't need to change that. `:30000` is the port number to connect to and is Foundry's default port.

> If you have configured Foundry to run on a different port number, you'll need to also configure ngrok with that port number instead.
{.is-info}

For example, your configuration might look like this:

```yaml
version: "2"
authtoken: 2PsxcYDJ0NAI75eF4JPbOe5tYG5_3xjxUJiU7tRqrm8QyoUnw
tunnels:
  foundry:
    proto: http
    hostname: giraffe-welcomed-instantly.ngrok-free.app
    addr: 127.0.0.1:30000
```

Save your configuration file with your tunnel configuration and exit your editor. Back on your command line, you should see something like:

`Valid configuration file saved at`

which indicates things are successful. If you see an `ERROR` instead, you can run `ngrok config edit` again to open the file and make sure it looks like the above.

#### 3. Start ngrok to test everything

Before we start ngrok as a service, which allows ngrok to restart whenever your computer does, we will test everything to make sure it works. 

Run `ngrok start foundry` to start your tunnel. You should see something like this if everything was successful:

![ngrok-running.png](/setup/hosting/ngrok/ngrok-running.png)

Now you can open a browser and go to your domain. The first time each person visits your domain, they will see this ngrok abuse warning:

![ngrok-abuse-interstitial.png](/setup/hosting/ngrok/ngrok-abuse-interstitial.png)

> If you wish to get rid of this page, you can upgrade to a paid ngrok account. As it only happens once for each person, however, it isn't very disruptive.
{.is-info}

You can click on the "**Visit Site**" button to continue. If everything was configured correctly, you should see your Foundry world login screen:

![foundry-login.png](/setup/hosting/ngrok/foundry-login.png)

#### 4. Start ngrok as a service

Remembering to start Foundry *and* ngrok can be annoying. Instead, you can configure ngrok to start as a "service" which means it will run whenever your computer starts or restarts. ngrok is a very lightwieght process so it will not cause your computer to slow down and it is very safe.

First, we can quit ngrok where it is running in your command line. Usually, this is by holding the "Control" key and pressing "C" on your keyboard. ngrok should have a hint for how to quit in the top right of the window.

Next, let's find the path where the configuration file is. You can do this by running:

`ngrok config check`

and seeing the output. For example, on MacOS:

```
❯ ngrok config check
Valid configuration file at /Users/foundry/Library/Application Support/ngrok/ngrok.yml
```

Copy your path (for example, "/Users/foundry/Library/Application Support/ngrok/ngrok.yml) and run

`ngrok service install --config "YOUR PATH HERE"`

Make sure to put your path in quotes (`"`). 

> You also may have to run that command as the "root" user if you get an error like "permission denied". On Linux and MacOS, you can do this by prefixing the command above with `sudo` (with a space after). You will be prompted to enter the password you use to login to your computer.
{.is-info}

Lastly, start ngrok as a service by running:

`ngrok service start`

> If you needed to run the `install` command as root, you'll need to do that for the `start` command as well.
{.is-info}

Now you can go back to your browser and refresh the page. If everything is working, you should see your Foundry World load as it did in the test. And that's it!

## Configuring Foundry to use ngrok as a Proxy

This step is technically optional. However, it allows Foundry to know that it's on the Internet. This means that Foundry can provide you with accurate invitation links and other nice-to-have features.

We need to edit the `options.json` file because the proxy settings are not available in Foundry's UI. To determine where that file is, refer to [Foundry's documentation](https://foundryvtt.com/article/configuration/#:~:text=Where%20Is%20My%20Data%20Stored%3F).

Go to the folder on your system and then open the `Config` folder. Inside, you should see a file called `options.json`. Open that in a text editor (Notepad, TextEdit, vim/Emacs, etc.).

Find the following settings and change them to your values:

* hostname: edit to be your ngrok hostname that you'd type in a browser

If you are [running Foundry as a server](https://foundryvtt.com/article/installation/#:~:text=Hosting%20a%20Dedicated%20Server%20with%20NodeJS), you can edit these additional settings:

* proxyPort: `443`
* proxySSL: `true`

> If you are not running as a server, editing those proxy options will result in an error message to show up for admins on the Setup screen, but it will not cause any real problems.
{.is-warning}

These configuration options may be at different places in the file. The example below shows only the lines you need to edit with `//...` indicating where other options that may exist:

```javascript
{
  //...
  "hostname": "giraffe-welcomed-instantly.ngrok-free.app",
  //...
  "proxyPort": 443,
  "proxySSL": true,
  //...
}
```

> Notice that the `hostname` value has quotes but the others do not. There are other configuration options here that you do not need to change.
{.is-info}

Save the file and exit. You may need to restart Foundry to see these have an effect. But now your invitation links should work along with other systems that might create URLs to Foundry:

![foundry-invitation.png](/setup/hosting/ngrok/foundry-invitation.png)
