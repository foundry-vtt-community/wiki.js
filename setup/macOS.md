---
title: macOS
description: 
published: true
date: 2025-08-31T15:51:59.090Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:33:56.065Z
---

## Installation

See the [official knowledge base](https://foundryvtt.com/article/installation/) for installation on OSX.

## Troubleshooting

### How to allow Foundry VTT to fully accept incoming connections to macOS 15+ without turning off any firewalls

***The Setup***
* You have port forwarding setup. 
* You’ve gone into **System Settings… > Network > Firewall > Options…** and have configured “Foundry Virtual Tabletop.app” to "Allow incoming connections.”
* In game your world link (**Game Settings > Invitation Links**) has a green checkmark.

That’s exactly how it should be setup, yet no one outside your network can connect! 

***The Issue***
* This is because macOS *silently* blocks ***all*** unsigned apps from accepting incoming connections.
* There are no user warnings, or error messages. macOS simply leaves the user to believe the Firewall is open when it’s not.

***The Fix***
* You have to manually create a self-signed certificate and then apply it to the Foundry VTT app.

***Notes***
* This “How To” assumes that you’ve correctly configured your router to allow FVTT traffic via Port Forwarding. (Within your world in FVTT the **Game Settings > Invitation** Links window will display a circled green checkmark if this is working.) 
* **IMPORTANT!** In Step 9 below you’ll need to know your macOS computer’s login password. This is the password you use to login into your macOS computer, not your Apple ID password.  
**DO NOT** start this process without it!

***Step-by-step***
1. Open the “Keychain Access” app. 
1. From the **Keychain Access** (process) menu select **Certificate Assistant > Create a Certificate…**: 
- **Name:** You choose (I used “FVTT”);  
**Identity Type:** Self Signed Root;  
**Certificate Type:** Code Signing.  
(Leave “Let me override defaults” unchecked.) 
3. Select “Create”. (Your certificate is good for 1 year.) 
1. After OK’ing the creation process you should be returned to the main Keychain Access app window. 
1. Quit the Keychain Access app.
1. Open the “Terminal” app. 
1. Paste in: `codesign -fs FVTT --deep /Applications/Foundry\ Virtual\ Tabletop.app`  
**WARNING! Make sure to replace “*FVTT*” with the name you choose for your certificate, if different.** 
1. Press return on the keyboard and you’ll be prompted to enter your Mac’s “login” password. 
- *Caution: You should select “Allow All” or you’ll be asked for your login password for each and every sub-app within the Foundry Virtual Tabletop.app package, and there’s a lot of them.* 
9. After a bit of time the Terminal Window should simply return to the prompt. 
- **If so:** You’re all set! You should now be able to access your FVTT world using the invitation link. 
- **If not:** You received an error message, so you’ll need to figure out what’s causing it. 

***Updating Foundry***
1. After configuring your macOS 15+ installation the permissions set in Step 7 above will break after each subsequent update of the app.
2. Just rerun Step 7 again to reapply the correct permissions.

Hopefully the above will help someone else to quickly get FVTT running on macOS.