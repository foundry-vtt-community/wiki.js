---
title: Node hosts on macOS
description: 
published: false
date: 2025-02-04T17:07:13.242Z
tags: 
editor: markdown
dateCreated: 2025-02-03T02:31:40.052Z
---

# A. Overview & Brief Instructions
This guide will walk you through the process of spinning up one or more Node instances of Foundry on a macOS host. It assumes you're running macOS 15 (Sequoia), but everything should still work under other recent versions, just some small UI details may be different.

If you *really* know what you're doing, you can skip to the [TL;DR](#X1) list of brief steps at the very end.

# B. Folder Setup
>For each of the terminal commands in this guide, copy the whole line, paste it into the Terminal app at the prompt, and then hit Return or Enter. {.is-info}

<a id="B1" href="#B1">B1:</a> First, locate and open the Terminal app. You can find it inside the Applications/Utilities folder, or by searching for it (⌘-Space). We'll be doing almost everything inside the Terminal app.

Copy, paste, and run this command in Terminal:
```
mkdir -p ~/Applications/Foundry/userdata
```

>If you decide to spin up more than one Foundry instance in this way, you'll want to *modify* these folder names so that it is clear which is which. For example, you may want to use `/Foundry12/userdata` to indicate that this is a Foundry v12 instance. If you're doing this, you'll just need to watch for other times in the guide where these folder names are used, and tweak them as needed.

This will create a few new folders inside your home directory, where we are going to store both the application and the userdata for this new Node instance.

<a id="B2" href="#B2">B2:</a> Let's verify that the folders were created, and open them up for the next steps.

In the Finder, open your home directory (⌘-Shift-H, or use the Go menu > `Home`).

Open the `Applications` folder, then you should be able to see and open the `Foundry` folder.

>The `Applications` folder inside your home directory is where it's best to store your own personal apps (as opposed to the "main" Applications folder at the root of your drive).{.is-info}

You should now be seeing something like this:

![macos-node-userdata.webp](/setup/hosting/macos-node-userdata.webp)

# C. Foundry Download

<a id="C1" href="#C1">C1:</a> Go to your [Purchased Licenses](https://foundryvtt.com/me/licenses) page on the Foundry website (you'll need to log in first). Choose the version of the app you want, and then choose **Linux/NodeJS** in the Operating System dropdown. (We're not downloading a Mac app here, we're getting a Node binary.)

Click the Download button. When it's complete, find the zip file that was downloaded, and double-click it to unzip it. You will now have a folder named something like `FoundryVTT-12.331`. You can delete the zip file.

<a id="C2" href="#C2">C2:</a> Move this new folder into the `Foundry` folder we created earlier, which should still be open.

<a id="C3" href="#C3">C3:</a> Rename this folder to `foundryapp`, so that we can refer to it more easily from now on.

You should now have a `foundryapp` folder and a `userdata` folder, inside the `Foundry` folder, like this:

![macos-node-foundryapp.webp](/setup/hosting/macos-node-foundryapp.webp)

# D. Install Homebrew
<a id="D1" href="#D1">D1:</a> Since macOS doesn't ship with Node, we need to install it. The simplest way to do that is via the [Homebrew](https://brew.sh/) package manager, which we'll download and install with this command:
```
sudo curl -o- https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh | bash
```
>This command needs to be run with `sudo` — elevated permissions — so you'll be prompted to enter your Mac user's password here.

This will take a minute or two to complete, with lots of activity scrolling by in Terminal.

<a id="D2" href="#D2">D2:</a> When that is complete, you will see a **Next steps:** line, and a bunch of commands. Select all three commands, from the first `echo` to the last `"`, like so:

![macos-node-nextsteps.webp](/setup/hosting/macos-node-nextsteps.webp)

Then copy that text (⌘C), paste it at the Terminal prompt (⌘V), and hit Return/Enter to run all three commands. They should complete immediately.

# E. Install Node
<a id="E1" href="#E1">E1:</a> Now we can install Node. We'll be using Node 22 here, which will work for Foundry v11, v12, and v13.

>If you're installing some other version, check the [Release Notes](https://foundryvtt.com/releases/) for the first Prototype build of that version to see what Node versions it supports.

Enter this into the terminal:
```
brew install node@22
```
<a id="E2" href="#E2">E2:</a> When that install is complete, we will again need to copy/paste a command that is shown at the end. Select the line starting with `echo` > copy > paste at the prompt > Return/Enter.

![macos-node-nodepath.webp](/setup/hosting/macos-node-nodepath.webp)

<a id="E3" href="#E3">E3:</a> After that, we need to refresh your Terminal session. Close the Terminal window, then go to the Shell menu and choose "New Window" (or hit ⌘N). Or, just quit the Terminal app and launch it again.

# F. Install PM2
<a id="F1" href="#F1">F1:</a> Now that Node is installed, we can use its `npm` package manager to install `pm2`, which will be what we use to start & stop this Foundry instance, and allow it to run in the background.

```
npm install pm2@latest -g
```
Ignore the `npm notice` lines that show when this is complete.

# G. Launch Foundry



# X. TL;DR
<a id="X1" href="#X1">X1:</a> If you generally know what you're doing with the Terminal and file management on macOS, here's a no-guide list of the steps to take:

>Please follow the actual full guide above if you don't already know what every one of these commands does.{.is-warning}

```
mkdir -p ~/Applications/Foundry/userdata
curl -o ~/Applications/Foundry/foundryvtt.zip "Timed URL"
unzip ~Applications/Foundry/foundryvtt.zip -d ~/Applications/Foundry/foundryapp
rm ~/Applications/Foundry/foundryvtt.zip
sudo curl -o- https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh | bash
brew install node@22
npm install pm2@latest -g
```