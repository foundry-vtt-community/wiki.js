---
title: How to set up a Package to be Release History friendly
description: Foundry's Package manager supports a history of package releases, this guide intends to lay out some ways to accommodate that.
published: false
date: 2020-10-19T19:05:35.032Z
tags: 
editor: markdown
dateCreated: 2020-10-19T15:45:56.156Z
---



Outline:
- Foundry and Packages
  - How does Foundry install a Package
  	- From UI?
    - From module.json url?
  - How does Foundry check for Updates

- Module Admin
	- How do you set up the Foundry Admin fields to allow a previous version to be downloaded

- Repository Resources
	- Github Releases
	- Gitlab Releases
	- Potential Automations

# Objectives

> Promote an understanding of how Foundry installs and updates Packages with suggestions for how to create a safe workflow whereby package authors can rest easy knowing that users are at no risk of installing non-working code.
{.is-info}

Foundry's package installation and update process is robust enough to allow automatic update detection and installation, while also allowing users who wish to to install a specific version of a package.

Setting up a package to make full use of this ability requires a little legwork, but for modules with frequent updates, the rewards are peace of mind.

# Background

Foundry prides itself on the ability to allow users the freedom to install packages from any source, not just the official list. So it's important to understand exactly how Foundry's client installs packages and checks for updates.

## How Foundry installs Packages

Installation is fairly straightforward and can be done in one of two ways.

### From the UI

0. User opens the UI, finds a package to install, clicks "Install."
	UI is populated from packages submitted and accepted to https://foundryvtt.com/packages with at least one release.
3. Foundry fetches the manifest from the latest release submitted to the module's `foundryvtt.com/admin` page and looks for a `download` url within it.
4. Foundry downloads the manifest's `download` url and checks if it is a zip file. If so, it unzips.

### From a user-input manifest url

0. User opens the UI and inputs a manifest url, clicks "Install."
2. Foundry fetches the manifest json and gets the `download` url from it.
3. Foundry downloads the manifest's `download` url and checks if it is a zip file. If so, it unzips.

## How Foundry checks for Package Updates

0. User initiates the flow by either clicking on "Check Update" an individual module or the "Update All" button.
1. Foundry fetches the manifest from the url in the currently installed package's manifest json.
2. Foundry compares the `version` strings of the installed module's manifest against the fetched manifest.
  2.a If the user is doing this on an individual package they have to then click the "Update" button.
	Foundry uses its [`isNewerVersion` helper function](https://foundryvtt.com/api/global.html#isNewerVersion) to compare manifest version strings.
3. If Foundry determines that the fetched manifest json has a newer version, it then downloads that manifest's `download` and checks if it is a zip file. If so, it unzips.

# Package Administration

After your submitted package is approved, you'll get access to a Package Management site at foundryvtt.com/admin. From here you can find a list of all of your approved packages. You can use this screen to manage other aspects of the how your Packages are displayed on the Official package list, but we're most concerned about the Versions for this guide.

## Package Versions

> If a package has no Package Versions present, it will not be available on either the official package list or in the Foundry UI.
{.is-warning}

> **This list does not participate in the process of updating a package.**
> 
> Deleting the latest version from this list will prevent new installations from the UI from installing, but will not prevent updates if the manifest url points to a json with a later version.
> 
> See the [How Foundry checks for Package Updates](#how-foundry-checks-for-package-updates) section for more information.
{.is-danger}

This section directly populates the list of released versions on the foundryvtt.com page for your module. It also informs the package installation UI within Foundry which version to download.

There is no limit to how many package versions you can have. Or if there is a limit, it's very high. We can leverage this to allow users to backtrack and install a previous version. This also allows us to "un-release" a version if it turns out there's problems with it.

![example-package-versions-display.png](/development/guides/releases-and-history/example-package-versions-display.png)
*Figure 1: Example of how these fields display at the bottom of a module's page.*

### Version Number

> If this increments but the linked manifest json's `version` field has not incremented, automatic updates will not pick up the change.
{.is-warning}

Should match the version number of the linked `module.json`.

### Package Manifest URL
Url to the `module.json` for this particular release.

This `module.json` should have a `download` field that points at a `zip` of just this release. This is important as it allows a user to go back and download/install a specific version of the module from the history.

### Release Notes URL (optional)
A nice-to-have url which points to a release note for this particular release. It could link to a specific heading of a larger release notes file, or to a release-specific page.

It's common to include a changelog in the repository's README or to use your source control host's release feature for this.

### Required Core Version

> This informs the Foundry Package Installer UI which version of the module to offer based on the Core version being used.
>
> Example: A package has two versions, one with the "Required Core Version" of `0.7.4` and one with `0.6.6`. A Foundry user looking for this package on a `0.6.6` version of foundry will be served the manifest url which satisfies this requirement.
{.is-info}

Should match the `minimumCoreVersion` field in the linked `module.json`.

The minimum required Foundry Core version that this particular release requires to work.

### Compatible Core Version

Should match the `compatibleCoreVersion` field  in the linked `module.json`.

The maximum Foundry Core version you are confident to say that this package works in. Note that nothing stops a user from installing the package on a version of Foundry Core that is higher than this.


# Version Control Host Specific Resources

## Github

### Releases

### Automation (Github Actions)

## Gitlab

### Releases

### Automation