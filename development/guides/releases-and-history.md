---
title: How to set up a Package to be Release History friendly
description: Foundry's Package manager supports a history of package releases, this guide intends to lay out some ways to accommodate that.
published: false
date: 2020-10-19T16:17:21.149Z
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

# Foundry and Packages

Foundry prides itself on the ability to allow users the freedom to install packages from any source, not just the official list. So it's important to understand exactly how Foundry's client installs packages and checks for updates.

## How Foundry installs Packages

### From the UI

0. User opens the UI, finds a package to install, clicks "Install."
	UI is populated from packages submitted and accepted to https://foundryvtt.com/packages with at least one release.
3. Foundry fetches the `module.json` from the latest release submitted to the module's `foundryvtt.com/admin` page to get the `download` url.
4. Foundry downloads the `module.json` `download` url and checks if it is a zip file. If so, it unzips.

### From a user-input module.json url

0. User opens the UI and inputs a `module.json` url, clicks "Install."
2. Foundry fetches the json from the input url and gets the `download` url.
3. Foundry downloads the `module.json` `download` url and checks if it is a zip file. If so, it unzips.

## How Foundry checks for Package Updates

0. User initiates the flow by either clicking on "Check Update" an individual module or the "Update All" button.
1. Foundry fetches the `module.json` from the url in the installed package's `module.json`.
2. Foundry compares the `version` strings of the installed module against the fetched `module.json`.
  2.a If the user is doing this on an individual module they have to then click the "Update" button.
	Foundry uses its [`isNewerVersion` helper function](https://foundryvtt.com/api/global.html#isNewerVersion) to compare version strings.
3. If Foundry determines that the fetched json has a newer version, it then downloads that json's `download` and checks if it is a zip file. If so, it unzips.

# Package Administration

After your submitted package is approved, you'll get access to a Package Management site at foundryvtt.com/admin. From here you can find a list of all of your approved packages. You can use this screen to manage other aspects of the how your Packages are displayed on the Official package list, but we're most concerned about the Versions for this guide.

## Package Versions
This section directly populates the list of released versions on the foundryvtt.com page for your module. It also informs the package installation UI within Foundry which version to download.

The following fields are present for each release entry at the time of this writing:

### Version Number
Should match the version number of the linked `module.json`.

### Package Manifest URL
Url to the `module.json` for this particular release.

This `module.json` should have a `download` field that points at a `zip` of just this release. This is important as it allows a user to go back and download/install a specific version of the module from the history.

### Release Notes URL (optional)
A nice-to-have url which points to a release note for this particular release. It could link to a specific heading of a larger release notes file, or to a release-specific page.

It's common to include a changelog in the repository's README or to use your source control host's release feature for this.

### Required Core Version

Should match the `minimumCoreVersion` field in the linked `module.json`.

The minimum required Foundry Core version that this particular release requires to work.

### Compatible Core Version

Should match the `compatibleCoreVersion` field  in the linked `module.json`.

The maximum Foundry Core version you are confident to say that this package works in. Note that nothing stops a user from installing the package on a version of Foundry Core that is higher than this.

![example-package-versions-display.png](/development/guides/releases-and-history/example-package-versions-display.png)
*Figure 1: Example of how these fields display at the bottom of a module's page.*

# Version Control Host Specific Resources

## Github

### Releases

### Automation (Github Actions)

## Gitlab

### Releases

### Automation