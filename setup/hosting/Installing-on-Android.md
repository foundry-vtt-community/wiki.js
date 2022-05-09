---
title: How to Install Foundry on an Android Phone
description: A guide for installing Foundry on Android.
published: true
date: 2022-05-09T04:37:57.820Z
tags: 
editor: markdown
dateCreated: 2021-03-21T22:25:01.724Z
---

>This article was automatically imported from the previous wiki and may contain inaccurate or outdated information.
{.is-warning}

# How to Install Foundry on an Android Phone
## Part 1: Installing Termux
1. Go to the Play store, search for Termux, install it.
2. Open Termux
3. Update its internal packages:
  	`pkg update`
    `pkg upgrade`

## Part 2: Remote Access
(Technically, you can skip this, and tun all the commands in part 3 directly on the Termux terminal.  But you're going to want a keyboard, and the ability to interact with the filesystem over FTP)

1.	Install openSSH in Termux
		`pkg install openssh`
2.	Start openSSH
		`openssh`
3.	Set up a password
		`passwd`
    You will be prompted for a password.  Nothing will show up as you type, this is normal.  You will need to enter it twice.
4.	Find out your username, and write it down
		`whoami`
5.	On your phone settings, go to "about phone", then "network", and note down the IP address (the one that looks like 192.128.123.45, not the long alphanumeric one)
6.	On your PC, using your SSH program of choice, set up an SSHv2 connection on port 8022, using the details you obtained above.

## Part 3: Installing Foundry

1.	If you set up SSH previously, all of the commands here can be typed in the SSH window (so long as Termux is running, and you've started openssh during that Termux session).  If not, type them in Termux on your phone.
2.	Get a "wakelock", i.e. stop your phone from closing Termux to save on battery.
		`termux-wake-lock`
3.	Install node.js
		`pkg install nodejs`
4. 	Install wget
		`pkg install wget`
5.	Make some directories for Foundry to live in
		`cd $HOME`
    `mkdir foundryvtt`
    `mkdir foundrydata`
6.	Get a temporary wget link for Foundry
	a.	Go to Foundryvtt.com, log in, and go to your purchased licenses.
  b.	Click the small "link" icon at the end of the node.js line.
  c.	A long URL will now have been copied to your clipboard.
7.	Install Foundry
		`cd foundryvtt`
    `wget -O foundryvtt.zip [the link you copied]`
    `unzip foundryvtt.zip`
8.	Run Foundry
		`node $HOME/foundryvtt/resources/app/main.js --dataPath=$HOME/foundrydata`
9.	In a browser on a PC, go to http://[Phone's IP]:30000
10.	If all has gone well up to this point, you should see a screen to enter your license key.  Do this.  Then you'll be on your standard Foundry window, but the server is running off your phone.  From here, it's the same as any other self-hosted install.

### Problems at this stage

Most of the time, UPnP will take care of the port forwarding for you, allowing you to connect from outside your local network.  Sometimes, this won't work.  There are guides elsewhere for setting this up.

Also, every time you restart your phone, Termux will lose its wakelock, the ssh server will shut down, and Foundry will close.  You can automate this (see below), or run the following commands when you start Termux:

`termux-wake-lock`
`sshd`
`node $HOME/foundryvtt/resources/app/main.js --dataPath=$HOME/foundrydata`

## Part 4: Making Foundry Start When the Phone Boots
1.	From the Play Store, install Termux Boot (this costs a small amount)
2.	Run Termux Boot once (it can then be ignored, but don't uninstall it)
3.	Make a new folder at `$HOME/.termux/boot/`.  This is easiest to do over FTP if you set that up earlier, but can be done within Termux or over SSH.
4.	Make a new file in that directory called `startup.sh`, with the following text:
	`#!/data/data/com.termux/files/usr/bin/sh`
	`termux-wake-lock`
	`node $HOME/foundryvtt/resources/app/main.js --dataPath=$HOME/foundrydata`
  `openssh` (only if you want the SSH server to start on boot)
  
- Note 1:  This is easiest to do in a text editor (e.g. notepad++) on PC, and then moving the file over SSH to the phone.
- Note 2:  You need just line feeds at the end of the line, not a “carriage return” which is what pressing the enter or return key will generally give.  To fix this, in notepad++, press ctrl+F, go to “replace”, and enter “\r” in the top line, and nothing in the bottom line.  Make sure “Extended” search mode is selected, and then hit “replace all”.  Trying to fix this in notepad just results in sadness - get notepad++.
  
5.	Restart your phone, wait a couple of minutes without doing anything on it, and then try connecting to the Foundry server.

## Troubleshooting/FAQ
Q: Do I need to root my phone?
A: No! Everything is being done by Termux, in filespace which Termux has access to, so Android is happy to do this with default permissions.

Q: My SFTP session disconnects while transferring stuff!
A: That might happen.  Just reconnect, and it will start back up again.

Q: I want to set up SSL for Foundry.
A: There are various guides out there.  You’ll need to set up SFTP access to get to move the keys over to the Config folder, but otherwise it works the same.

Q: Foundry isn’t starting with the phone!
A: Give it a couple minutes. It can take a bit of time to actually start.  
	If it’s working, you should see a notification in the taskbar that termux is running “1 task (wake lock held)”
	If you see that and Foundry is not working, you probably mistyped something.
	If it’s not working, then fire up SSH, type `cd $HOME/.termux/boot`, then `bash startup.sh`, and see if it throws any errors.  If it’s saying unknown command, and the commands end in \r, then go back and read the note on deleting carriage returns.

Q: You’ve done something in a silly/overcomplicated way!
A: Very probably, this guide was originally made when I (BadIdeasBureau) was new to linux.  It's a wiki, edit it to fix it.

Q: Why?
A: Why not?
Also, because it provides a way of turning an old phone that was sat around failing to be recycled into a thing I can stick in a cupboard and use as a dedicated server.

Q: Can I do this on an iPhone?
A: No idea.  Probably not.  Definitely not with Termux, but there might be an equivalent.

Q: Why aren’t you using the shared internal storage/external storage?
A: I tried this.  It went weird.  Files created by the terminal didn’t show up when connected via USB (even in folders which were definitely public), and Foundry could make its config file, but then threw an error when trying to access it. Fixing this would be a neat improvement to the guide, but for now you're stuck inside Termux's app space (which isn't much of a limitation, but does mean file transfer has to be via FTP or Foundry's interface, rather than just over USB).



