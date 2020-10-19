---
title: How to set up a Package to be Release History friendly
description: Foundry's Package manager supports a history of package releases, this guide intends to lay out some ways to accommodate that.
published: false
date: 2020-10-19T21:28:54.858Z
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

# General Guidelines for History-friendly Releases

## Package Files

The files in the package zip that is downloaded and installed on the user's Foundry instance.

### Manifest JSON

#### `download`
This field should be a url to only this package version's zip download. It should not point to a place where a user can find the latest version of the package.

#### `manifest`
This field should be a stable url that will always point to the latest manifest JSON without needing to change.


## Foundry Package Admin Version Fields

Fields for a given row on the Package Admin "Package Version" list.

#### Package Manifest URL

This should point to this specific version's manifest JSON. The reason for this is because only that specific manifest JSON has the `download` field pointing to that particular version's zip files.

#### Required Core Version

Keep this up to date at all times, especially when breaking changes happen.


# Version Control Host Specific Resources

The odds are good that whatever service you use to host your project repository has some tools that can help keep distinct versions of your package.

## Github

### Releases
Releases are Tags on Github that have extra metadata you can edit from the UI or from an API. Github provides a stable URL that will always point to the latest release: `user/repo/releases/latest`.

When a package author is ready to create a release version of their package, they can create a release to tag a particular commit in the history as that version's source. Then they can attach two artifacts to the release:

- Manifest: `module.json` -> `download` should point at the `package.zip` uploaded to this release
- Package: `package.zip` -> this should include the same `module.json` as is attached

From there, in the Foundry Admin Package Version list, provide the url for the release's manifest as the Manifest URL.

See the [full documentation](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/managing-releases-in-a-repository) for more information.


### Automation (Github Actions)

There is a lot of cool stuff you can leverage [Github Actions](https://docs.github.com/en/free-pro-team@latest/actions) to automate with this process. Below are a few repositories to give you some ideas.

- [League-of-Foundry-Developers/FoundryVTT-Module-Template](https://github.com/League-of-Foundry-Developers/FoundryVTT-Module-Template) - Action triggers when a release is created which updates the `module.json` version/download fields, zips up the files, and attaches them to the release.
- [League-of-Foundry-Developers/foundry-typescript-template](https://github.com/League-of-Foundry-Developers/foundry-typescript-template) - Action triggers on any push to master which creates a release, zips up the `/dist` directory and attaches both it and the `module.json` to the created release.
- [Spice-King/foundry-swnr](https://github.com/Spice-King/foundry-swnr) - Action triggers when a tag with `v*` in the name is pushed which builds and zips the system files, creates a release with that tag's name, and attaches the built files to the release.


### Example Process

ElfFriend-DnD is a package author who is ready to release verson 2.23.1 of their module: "AmazingModule".

Their `module.json` already leverages the `releases/latest` Github Repo url to ensure Foundry clients always look for the most recently released `module.json`:

```json
  "manifest": "https://github.com/ElfFriend-DnD/AmazingModule/releases/latest/download/module.json"
```

1. Update the following in `module.json`:
  - `version` increment to 2.23.1
  - `download` -> `https://github.com/ElfFriend-DnD/AmazingModule/releases/download/2.23.1/module.zip`
2. Zip up all relevant module files into a file called `module.zip`.
3. (Optional) Commit and push these changes.
  *It is recommended to at least commit and push the `module.json` changes to keep your repo up to date with the latest version numbers.*
4. [Create a Release](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/managing-releases-in-a-repository#creating-a-release) on the correct branch of the repository. 
  4.a. Set the tag name to "2.23.1"
  4.b. Add some change notes to the body of the release
  4.c. Attach the `module.json` and `module.zip` files to the release
5. Open AmazingModule's Foundry Package Admin page.
6. At the very bottom of the Package Version list, fill in the fields for this new release.
  - Version: `2.23.1`
  - Manifest: `https://github.com/ElfFriend-DnD/AmazingModule/releases/download/2.23.1/module.json`
  - Changelog:  `https://github.com/ElfFriend-DnD/AmazingModule/releases/tag/2.23.1`
  - Required Core Version and Compatible Core Version as required.
7. Hit Save.


## Gitlab

This is a Stub, I'm not familiar with how Gitlab does things, but I expect it's a similar process to the above. [This](https://gitlab.com/fvtt-modules-lab/quick-insert/-/tree/master) is an example of a project that leverages Gitlab releases.

### Releases

[Gitlab Releases Docs](https://docs.gitlab.com/ee/user/project/releases/)
