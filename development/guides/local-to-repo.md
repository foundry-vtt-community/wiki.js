---
title: Publishing a Module
description: 
published: false
date: 2025-02-08T22:18:03.061Z
tags: 
editor: markdown
dateCreated: 2025-02-07T22:15:43.217Z
---

# A. Overview
Your content here

# B. GitHub Account
In order for Foundry clients to be able to install your module, its files need to be hosted somewhere on the public internet (premium modules have extra layers on top of what we'll cover here).

Since most module developers need this hosting *and also* benefit from version control management, a free Git repository fills both of these needs nicely. Even if you don't ever use the Git parts, it's still a great way to host your module.

>There are two major services for this kind of thing, [GitHub](https://github.com/) and [GitLab](https://about.gitlab.com/). For reasons of familiarity, this guide will use GitHub. {.is-info}

<a id="B1" href="#B1">B1:</a> If you don't have one already, sign up for a new GitHub account [here](https://github.com/signup).

# C. New Repository
<a id="C1" href="#C1">C1:</a> From your GitHub dashboard after logging in, hit the green `Create repository` button.

<a id="C2" href="#C2">C2:</a> The `name` of this repository should typically be the same as the `id` of your module, for consistency, but it doesn't *really* matter what you choose here.

<a id="C3" href="#C3">C3:</a> Make sure you choose `Public` for the repo's visibility. Foundry can't download files from private repositories.

<a id="C4" href="#C4">C4:</a> Ignore the rest of the settings, unless you know what you're doing with them, then hit `Create repository`

<a id="C5" href="#C5">C5:</a> You now have a GitHub repo for your module. Copy the base URL for this repo now, as we'll need it in the next section. This will be something like `https://github.com/my-user/my-module-name`

# D. Local Module Setup
For various reasons, it will be better to do the following changes locally, rather than uploading them to the repo and making changes over there.

>This guide will assume that you're hosting on a desktop PC. If your host is on a server somewhere else, you'll do the same steps (starting with <a id="D2" href="#D2">D2</a>), but after first downloading your module's files to your local machine, using FTP or whatever else you use to manage your remote files.

<a id="D1" href="#D1">D1:</a> Find your module's folder in your userdata: While Foundry is running, right-click it in the Windows taskbar or macOS dock, and choose `Browse User Data`. Open the `Data` folder in there, then the `modules` folder. You will see your module's folder named with its `id`.

<a id="D2" href="#D2">D2:</a> Inside your module's folder, find and open the `module.json` file in any text editor. It will look something like this, probably with some additional lines defining compendium packs:
```json
{
  "id": "my-module",
  "title": "My Module",
  "version": "1.0.0",
  "compatibility": {
    "minimum": "12",
    "verified": "12"
  }
}
```


# E. Upload

# F. Create a Release

# G. Test

