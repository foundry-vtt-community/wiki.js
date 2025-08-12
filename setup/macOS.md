---
title: macOS
description: 
published: true
date: 2025-08-12T01:04:39.030Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:33:56.065Z
---

## Installation

See the [official knowledge base](https://foundryvtt.com/article/installation/) for installation on OSX.

## Troubleshooting

### How to allow Foundry VTT to fully accept incoming connections to macOS 15+ without turning off any firewalls

***Issue/Symptoms:***
You have port forwarding setup, your world link has a green checkmark, yet no one outside your network can connect. This is because macOS blocks *all* unsigned apps. 

***Notes:***
* This "How To" assumes that you've correctly configured your router to allow FVTT traffic via Port Forwarding. (Within your world in FVTT the **Game Settings > Invitation** Links window will display a circled green checkmark if this is working.) 
* In Step 9 below you'll need to know your macOS computer's login password. 

***Step-by-step:***
1. Open the "Keychain Access" app. 
2. From the **Keychain Access** (process) menu select **Certificate Assistant > Create a Certificate…**. 
3. Name: You choose (I used "FVTT"); Identity Type: Self Signed Root; Certificate Type: Code Signing. Leave "Let me override defaults" unchecked. 
4. Select "Create". (Your cert. is good for 1 year.) 
5. After OK'ing the creation process you should be returned to the Keychain Access app. 
6. Open the "Terminal" app. 
7. Paste in: `codesign -fs FVTT --deep /Applications/Foundry\ Virtual\ Tabletop.app` 
8. **WARNING! Make sure to replace "*FVTT*" with the name you choose for your cert., if different.** 
9. Press return on the keyboard and you'll be prompted to enter your Mac's "login" keychain password. (This is the password you use to login into your macOS computer.) 
10. *CAUTION: You should select "Allow All" or you'll be asked for your login password for each and every sub-app within the Foundry Virtual Tabletop.app package, and there's a lot of them.* 
11. After a bit of time the Terminal Window should simply return to the prompt. 
12. If so: You're all set! You should now be able to access your FVTT world using the invitation link. 
13. If not: You received an error message, so you'll need to figure out what's causing it. 

***Updating Foundry***
1. After configuring your macOS 15+ installation the permissions set in Step 7 above will break after each subsequent update of the app.
2. Just rerun Step 7 again to reapply the correct permissions.

Hopefully the above will help someone else to quickly get FVTT running on macOS.