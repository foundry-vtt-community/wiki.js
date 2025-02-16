---
title: Publishing a Module
description: 
published: true
date: 2025-02-16T23:11:47.551Z
tags: 
editor: markdown
dateCreated: 2025-02-07T22:15:43.217Z
---

# A. Overview
This guide will help you take any locally-created module and make it available for others to install, by publishing releases on a public repository.

If your goal is to publish a *premium* package that isn't freely available, you should follow Foundry's [Marketplace FAQ](https://www.foundryvtt.store/faq), reaching out to them for support if needed.

>All of the same steps here apply to publishing a Game System as well; you'll just use `system` instead of `module` in various places.

>Note that because the steps in this guide involve making your module's content downloadable by anyone, you should not include anything for which you don't have full distribution rights — including art assets, published intellectual property, etc. {.is-warning}

# B. GitHub Account
In order for Foundry clients to be able to install your module, its files need to be hosted somewhere on the public internet (premium modules have extra layers on top of what we'll cover here).

Since most module developers need this hosting *and also* benefit from version control management, a free Git repository fills both of these needs nicely. Even if you don't ever use the Git parts, it's still a great way to host your module.

>There are two major services for this kind of thing, [GitHub](https://github.com/) and [GitLab](https://about.gitlab.com/). For reasons of familiarity, this guide will use GitHub. {.is-info}

<a id="B1" href="#B1">B1:</a> If you don't have one already, sign up for a new GitHub account [here](https://github.com/signup).

# C. New Repository
<a id="C1" href="#C1">C1:</a> From your GitHub dashboard after logging in, click the green `Create repository` button.

<a id="C2" href="#C2">C2:</a> The `name` of this repository should typically be the same as the `id` of your module, for consistency, but it doesn't *really* matter what you choose here.

<a id="C3" href="#C3">C3:</a> Make sure you choose `Public` for the repo's visibility. Foundry can't download files from private repositories.

<a id="C4" href="#C4">C4:</a> Ignore the rest of the settings, unless you know what you're doing with them, then hit `Create repository`

<a id="C5" href="#C5">C5:</a> You now have a GitHub repo for your module. Note the base URL for this repo now, as we'll need it in the next section. This will be something like `https://github.com/my-user/my-module`

# D. Local Module Setup
For various reasons, it will be better to do the following changes locally, rather than uploading them to the repo and making changes over there.

>This guide will assume that you're hosting on a desktop PC. If your host is on a server somewhere else, you'll do the same steps (starting with [D2](#D2)), but after first downloading your module's files to your local machine, using FTP or whatever else you use to manage your remote files. {.is-info}

<a id="D1" href="#D1">D1:</a> Find your module's folder in your userdata: While Foundry is running, right-click it in the Windows taskbar or macOS dock, and choose `Browse User Data`. Open the `Data` folder in there, then the `modules` folder. You will see your module's folder named with its `id`.

>After using this shortcut to open your userdata location, it's best to make sure Foundry is closed from now on; we don't want your module's files to be potentially in use while we're making changes. {.is-warning}

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
<a id="D3" href="#D3">D3:</a> We're going to add two new lines to this manifest. Hit Enter/Return at the end of the `"version"` line to create a new blank line. On that line, paste this:
```json
"manifest": "https://github.com/my-user/my-module/releases/latest/download/module.json",
```
After pasting that in, replace the `my-user` and `my-module` parts with your own GitHub username and repo name, which we noted above in [C5](#C5)</a>.

>**What this line does:** After your module has been installed on someone's host, this is the URL that Foundry checks to see if a newer version of your module is available; it's pointing at whatever the latest release is on your repository. Foundry will download *that* manifest, check the `version` listed inside, and compare that against the currently installed version.

<a id="D4" href="#D4">D4:</a> Make another blank line after the `manifest` line we just added, then paste this:
```json
"download": "https://github.com/my-user/my-module/releases/download/1.0.0/module.zip",
```
Again, edit the `my-user` and `my-module` parts to be your own.

>**What this line does:** This `download` URL points at a zip file that contains this specific version of your module (which doesn't exist yet, but we'll make it soon). Note that the version numbers in the URL match the `version` of this manifest. We don't ever want to have a `download` URL that points at your repo's `releases/latest`, because that would prevent users from being able to download *older* versions of your module when needed.

<a id="D5" href="#D5">D5:</a> Check your manifest for typos or anything that may have been missed. The section we changed should look something like this now (but with your own GitHub paths):
```json
{
  "id": "my-module",
  "title": "My Module",
  "version": "1.0.0",
  "manifest": "https://github.com/my-user/my-module/releases/latest/download/module.json",
  "download": "https://github.com/my-user/my-module/releases/download/1.0.0/module.zip",
  "compatibility": {
    "minimum": "12",
    "verified": "12"
  }
}
```
>Note that each line we added has a comma at the end. JSON is an extremely syntax-sensitive format, and your manifest will cease to work entirely if there are any errors. If you're ever in doubt, or want to double-check anyway, copy the entire text of the manifest, paste it into [jsonlint.com](https://jsonlint.com/), and hit the `Validate JSON` button. {.is-info}

<a id="D6" href="#D6">D6:</a> Now we'll create the zip file. Close the `module.json` file if it's still open, then navigate up a folder into `modules`, so that you can see the folder for your own module.

Right-click your module's folder, and compress it into a zip file:
- Windows 10: `Send to > Compressed (zipped) folder`
- Windows 11: `Compress to > zip`
- macOS: `Compress [folder name]`

<a id="D7" href="#D7">D7:</a> For ease of referring to this file in other places, rename it to `module.zip` now.

>If your OS hides file extensions, make sure you're not accidentally naming it `module.zip.zip`.

# E. Upload
<a id="E1" href="#E1">E1:</a> Head back to your GitHub repository, and click the `uploading an existing file` link in the blue "Quick Setup" section.

>Make sure that the Foundry app isn't running for the next step. Any time you're copying user data, you want to be sure none of your databases are still open. {.is-warning}

<a id="E2" href="#E2">E2:</a> In your userdata on your computer, open up your module's folder (ignore the zip file for now), then drag all of its *contents* into the GitHub page, where it indicates you can drop files.

<a id="E3" href="#E3">E3:</a> Edit the descriptive fields if you like, but you can also leave them as-is. Click the green `Commit changes` button.


# F. Create a Release
<a id="F1" href="#F1">F1:</a> On the right side of your GitHub page, you should see a Releases section. Click the `Create a new release` link there.

<a id="F2" href="#F2">F2:</a> Click the `Choose a tag` dropdown, enter `1.0.0` as the tag, and either hit Enter/Return or click `Create new tag`.

>You'll see a suggestion in the sidebar here about prefixing your versions with a `v`. For Foundry projects, do **not** do this. Foundry compares version numbers in a way that breaks down if you have a `v` in some cases but not in others, so it's best to avoid the practice entirely. {.is-info}

<a id="F3" href="#F3">F3:</a> Enter `1.0.0` as the `Release title` as well. This doesn't strictly need to match the tag, but it's a decent way to name your releases.

<a id="F4" href="#F4">F4:</a> Write a `Description` if you like, but you can also leave this field blank. Ignore the "Set as a pre-release" checkbox; leave it as-is.

<a id="F5" href="#F5">F5:</a> Now we're going to drop two things into the zone where it says `Attach binaries by dropping them here`, from your computer:

- The `module.json` file, from inside your module's folder.
- The `module.zip` file, from outside your module's folder, that we created earlier.

The page should now look like this:
![local-to-repo-new-release-dark.webp](/development/guides/local-to-repo-new-release-dark.webp)

<a id="F6" href="#F6">F6:</a> Click the green `Publish release` button. You will be taken to a new page for this release, showing its name, tag, and attached assets.

<a id="F7" href="#F7">F7:</a> Right-click on the attached `module.json` file, and choose `Copy Link Address` (or whatever your browser's command is for copying this URL). This is your **Manifest URL**.

>We have just created a place on the web where the files for this specific version of your module now live. The `module.json` asset is your module's Manifest, and the `module.zip` is the file that the manifest tells Foundry to download. {.is-info}

>You may have noticed that the contents of your module that we uploaded to the repository earlier were not actually involved at all in the end result. The publishing workflow automatically attached a `Source Code` file to the release, but we created our own zip manually, and uploaded *that*. For various reasons, it's better to be intentional about the file we provide here, and not rely on the automatically-generated zip. If you get into using automated release workflows, this practice will change slightly.

# G. Test
<a id="G1" href="#G1">G1:</a> Your module should now be manually installable in any Foundry host. Your *own* local host already has this module installed though, so to test this out, start by removing its folder from the `modules` folder (Foundry should still be closed at this point). You can drag it to your desktop, or anywhere else temporarily while we make sure the installation works. Can also remove the zip file we made.

<a id="G2" href="#G2">G2:</a> Launch Foundry, go to the Add-on Modules tab in Setup, click the `Install Module` button.

<a id="G3" href="#G3">G3:</a> In the `Manifest URL` field at the bottom of the Install Module window, paste in the URL that we copied in step [F7](#F7), then click the `Install` button.

You should see a blue banner indicating that your module was installed successfully. If it worked, you can now delete the module folder that you moved in step [G1](#G1); your module is now inside `modules` again.

>Once your module is installable, it's now also **volatile**: Any time you update it in Setup, your local copy will be wiped out completely and replaced with a freshly downloaded copy, just like all other package updates. If you're going to continue working on your module locally — making changes, adding more content — you should **right-click it in Setup > Lock Module**, to keep it safe from this. {.is-warning}

# H. (Optional) Official Listing
Once your module is hosted and installable, you may choose to submit it to Foundry for review and potential listing in their index, which would give it a package page on [foundryvtt.com](https://foundryvtt.com) and make it available to be found via search in Foundry Setup.

<a id="H1" href="#H1">H1:</a> To submit your module for review, go to [foundryvtt.com/creators/submit/](https://foundryvtt.com/creators/submit/), and fill out the form. Your `Package URL` is your GitHub repository's base URL, from step [C5](#C5).

<a id="H2" href="#H2">H2:</a> Once your module has been approved, and you've got a package page for your module on the Foundry site, you can create a release for it there. Publishing a Foundry release puts your module into the searchable Foundry ecosystem, letting it be found and installed by users in Setup.

While logged in as yourself on the Foundry site, go to your [Authored Packages](https://foundryvtt.com/me/packages) page, and hit the `Edit` link for your module.

<a id="H3" href="#H3">H3:</a> Fill out the `Description` field if you haven't already, to give users context for what your module does.

<a id="H4" href="#H4">H4:</a> At the bottom of the page, on the right side, hit the `+ Add` button to add a new release.

<a id="H5" href="#H5">H5:</a> Fill in the fields as follows:
- **Version Number:** Enter exactly the same number as the `tag` you used for this release on GitHub (so, `1.0.0` if following this guide). Do not use a `v` here (or anywhere).
- **Package Manifest URL:** The same link we copied in [F7](#F7) — right-click the `module.json` asset attached to the release > Copy Link Address.
- **Release Notes URL:** Typically this will just be the GitHub page for this specific release — the page you're taken to immediately after publishing the release. In the case of our example here, `https://github.com/my-user/my-module/releases/tag/1.0.0`
- **Minimum Core Version:** The absolute minimum core Foundry version that is required to install your module. If you're unsure about this, use the whole number of the version you built the module with. So, if you're currently running `12.331`, for example, enter `12` here.
- **Verified Core Version:** The specific version of core Foundry that you have verified your module works with, for sure. If it runs on `12.331`, enter `12.331` here. If your module is fairly straightfoward, or is just compendium packs, you can be more broad here and just enter `12`.
- **Maximum Core Version:** Leave this field blank, *unless* you have tested your module in newer versions of core Foundry and you know that it doesn't function there.

>You can edit any of these fields after the fact, including making changes to the version requirements, if for example you later discover that your module is also verified to work under Foundry v13. You don't need to publish a new release to have these changes go into effect.

<a id="H6" href="#H6">H6:</a> Click the `Save Package` button when you're done.

# I. Updating Your Module
Publishing a new version of your module is *mostly* the same as publishing the first release, as we did in [Section F](#F) (and then also in [Section H](#H) for the official Foundry release, if you're published there). We'll go through the process in detail here.

After making all of your content/functionality changes to your module locally, we need to update its local manifest too:

<a id="I1" href="#I1">I1:</a> With Foundry closed, go into your module's local folder, and open `module.json` in a text editor.

<a id="I2" href="#I2">I2:</a> Find the `"version":` line, and enter a *new* version number, higher than the last, by whatever amount you feel is appropriate. If the last version was `1.0.0`, and this is update just has some small fixes, `1.0.1` would be appropriate. If it's an entirely new set of content or features, perhaps `2.0.0` would be better. As always, **do not prefix with a `v`**.

<a id="I3" href="#I3">I3:</a> Find the `"download":` line, and edit the version number inside that path to match the new number you chose above. The `"manifest":` line doesn't need to change.

<a id="I4" href="#I4">I4:</a> Save and close the `module.json` file.

Now we'll upload the bare files of your module to your repository, so that the repo is up-to-date:

<a id="I5" href="#I5">I5:</a> On the main page of your repo (the `<> Code` tab), click the `Add file` dropdown, and choose `Upload files`.

<a id="I6" href="#I6">I6:</a> From inside your module's local folder, drag all of its *contents* into the `Drag files here...` zone in the GitHub page that we just opened — don't drag the entire module folder itself, as that will add another layer of folder depth that we don't want.

<a id="I7" href="#I7">I7:</a> Change the `Commit changes` text that says "Add files via upload" to something more descriptive, like "Upload version 2 content" or "Fixed broken compendium links" or whatever else briefly describes the changes you've made in this update. You can elaborate in the `Description` field if you like.

<a id="I8" href="#I8">I8:</a> Leave the rest as-is, and hit the green `Commit changes` button.

>Most of the process described in this section can be automated with a few clicks if you choose to set up something like [GitHub Desktop](https://desktop.github.com/download/) on your local machine, but that is beyond the scope of this guide.

Now we publish a new Release:

<a id="I9" href="#I9">I9:</a> Click the `Releases` link over on the right side of your GitHub page, then click the `Draft a new release` button, in the upper right.

<a id="I10" href="#I10">I10:</a> Hit the `Choose a tag` dropdown, and enter exactly the same `version` number you entered into the `module.json` file in step [I2](#I2). Again, no `v`.

<a id="I11" href="#I11">I11:</a> Enter a title and a description. The title doesn't need to match the version number, but it's a reasonable practice.

Now we're ready to to attach the release assets:

<a id="I12" href="#I12">I12:</a> From your module's local folder, drag the `module.json` file into the `Attach binaries...` zone.

<a id="I13" href="#I13">I13:</a> Go up one folder so that you're in `/modules`, right-click your module's folder, and compress it to a zip file. Rename the file to `module.zip` when it's done.

<a id="I14" href="#I14">I14:</a> Drag the `module.zip` file into the `Attach binaries...` zone on the GitHub release page.

<a id="I15" href="#I15">I15:</a> Click the green `Publish release` button.

>At this point, anyone with your module installed should receive this new version when checking for updates. Remember that if you check for updates in this way in your own local host, your module's folder will be wiped out and replaced with the new download — you may not want to do this. {.is-info}

>If you have an official Foundry listing for your module, you should now also post a new release there. This is done the same way as before, starting from [H4](#H4).
