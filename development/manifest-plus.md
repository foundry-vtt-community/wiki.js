---
title: Package Manifest+
description: An expanded manifest format.
published: true
date: 2020-12-02T05:15:21.804Z
tags: 
editor: markdown
dateCreated: 2020-12-02T04:47:58.438Z
---

The Manifest+ specification is an unofficial community developed extension of the official Foundry manifest intended to add additional data which can be used by packages within Foundry and by external application.

This specification was developed by The League of FVTT developers. While the Core Foundry software simply ignores these additions, the full data of the manifest is still available to packages running within Foundry. Additionally, a number of external applications including The Bazaar package browser on The Forge use this extended data to give users a more rich experience by adding cover images, icons, enhanced contact details, and more.

## Package Manifest Standard Fields

These are documented in the official [Introduction to Module Development](https://foundryvtt.com/article/module-development/) page.

### Required Fields

```json
	/* name: Must match the package's folder name */
	"name": "package_name",
	
	"title": "Package Title",
	
	"description": "Description of the package",

	/* version: Using a x.y format or a non string field can lead to confusion 
		and problems due to 0.10 being inferior to 0.9 for example when taken as a number */
	"version": "version number in x.y.z format",
```

### Optional but Highly Recommended
```json
{
	/* scripts: should not be used if esmodules is used */
	"scripts": ["relative/path/to/script.js"],

	"esmodules": ["relative/path/to/esmodule.js"],

	"styles": ["relative/path/to/style.css"],

	/* url: Can be set to the project page or its README in the git repository for example */
	"url": "https://homepage.com",

	/* manifest: Be careful not to point to the html version on github for example, always point 
		to the raw file and always to the latest manifest file otherwise the package wouldn't be updateable */
	"manifest": "https://link.com/to/raw.module.json",

	/* download: Point this to the zip file archive of the latest version. Do not point this to your master repository branch
		but instead to a version tag, otherwise you risk having users downloading the wrong version */
	"download": "https://github.com/kakaroto/fvtt-module-lmrtfy/archive/v1.3.zip",

	/* minimumCoreVersion: If set, the package cannot be installed on Foundry versions inferior to this */
	"minimumCoreVersion": "0.7.0",

	/* compatibleCoreVersion: if not set, or if set to a value inferior to the current Foundry version, 
		a warning will appear to the user about compatibility risk*/
	"compatibleCoreVersion": "0.7.7"
}
```

### Optional Fields
```json
{
	/* type: Specify the package type, can be 'module', 'system' or 'world'.
			This is useful if the manifest file isn't 'module.json', 'system.json' or 'world.json' */
	"type": "module",

	/* systems: Array of systems that the package requires, set to null for system agnostic.
			An empty array means it does not support any system at all */
	"systems": ["system_name"], 

	"packs": [{
		"name": "name_of_the_compendium",

		"label": "The label/title of the pack",

		"path": "relative/path/to/pack.db",

		/* entity: The entity name can be: Actor, JournalEntry, Scene, Item, Macro, Playlist */
		"entity": "EntityName"
	}],

	/* author: For multiple authors, or providing extra information, you can use the authors field, 
		but this field must be filled as it is what Foundry will display */
	"author": "The League",

	/* authors: You can specify a list of authors with more information about each individual author,
		Some of these fields are additions from the manifest+ specification */
	"authors": [{
		"name": "Name of the author",

		"url": "https://website.com/of/the/author",

		"email": "email@example.com",

		/* discord: Manifest+ addition */
		"discord": "discordID#0001",

		/* twitter: Manifest+ addition */
		"twitter": "@TwitterHandle",

		/* reddit: Manifest+ addition */
		"reddit": "u/RedditUsername"
	}],

	"languages": [
		{
			/* lang: 2 letter language code, or 2 language code with 2 letter country code such as pt-BZ */
			"lang": "en",

			/* name: The language name, use the translated name, for example 'Español' instead of 'Spanish' */
			"name": "English",

			/* path: Relative path to the translation json file */
			"path": "lang/en.json"
		}
	],

	/* dependencies: If your module depends on functionality from another module, add it here so
		it can be installed alongside it, and enabled automatically by Foundry. 
		Do not assume it will always be enabled. */
	"dependencies": [
		{
			/* name: This needs to be the dependency name (not title) but it is what will be displayed in Foundry */
			"name": "module_name",

			/* type: The type can be 'module' or 'system', defaulting to 'module'. */
			"type": "module",
			
			/* manifest: Specifying the manifest ensures it can be installed at the same time if it's not already */
			"manifest": "https://link.com/to/manifest.json"
		}
	],

	/* socket: Boolean value representing whether this module uses game.socket.emit with 'module.<name>' message names */
	"socket": true,

	/* minimumSystemVersion: If set, the module can not be activated in a world 
		with a system whose version does not exceed the set value */
	"minimumSystemVersion": "version number in x.y.z format",
	
	"readme": "https://link.com/to/readme.html",
	
	"bugs": "https://link.com/to/issue/tracker",
	
	/* license: Specify the license this package is released under. */
	"license": "relative/path/to/license/file",
	
	/* changelog: Specify a link to the latest changelog*/
	"changelog": "https://link.com/to/this/versions/changelog",
}
```

## Manifest+ Specific Additions

In addition to the extra fields in the `authors` array, the following are added to manifests with this standard. By definition, these are optional but useful.

```json
{
	/* media: Use the media field to specify any sort of media files relating to this package's listing.
		this can be used to specify screenshots or video tutorials and the like */
	"media": [
		{
			/* type: The type of media, can be: icon, cover, screenshot, video */
			"type": "screenshot",

			/* url: This can be a relative path to the media file, or a URL to it (youtube link for example)
				it is recommend to use web accessible URLs rather than relative paths, otherwise a screenshot 
				or cover image would not be accessible unless the zip download was first downloaded and extracted
				*/
			"url": "link/to/media/file"
		}
	],

	/* library: Set to true if this module is a library module and should not be shown in a UI, or shouldn't be user browsable/installable */
	"library": true,

	/* includes: List the files to be included in this package. Can be useful for automatic CI/CD pipelines 
		generating a zip archive or to make sure that no file is missing from a package */
	"includes": ["relative/path/to/files"],

	/* deprecated: details added by a module author to explain that it should not be used any more */
	"deprecated": {
		/* coreVersion: If set, this module is assumed to have been deprecated by a core update */
		"coreVersion": "0.7.0",

		/* reason: Human readable string explaining why this is deprecated */
		"reason": "This was added to foundry core.",

		/* alternatives: Array of alternative modules with the same fiels as dependencies */
		"alternatives": [
			{
				"name": "module_name",
				"manifest": "https://link.com/to/manifest.json"
			}
		]
	},

  /* conflicts: Same as "dependencies" but the opposite, any known conflicts */
	"conflicts": [
    {
      "name": "module_name",
      "type": "module",
      
      /* versionMin/Max: If defined, this is the range of the other package's versions in which the conflict manifests */
      "versionMin": "version number in x.y.z format",
      "versionMax": "version number in x.y.z format"
    }
  ]
}
```

### Media Recommendations

There is no guarantee how the media files will be used, but these are the recommended dimensions and known existing usages.

#### Cover

Avoid putting large text on the cover image, it should showcase the package rather than the name of the package.

- Width: 1200px
- Aspect Ratio: 2:1

Currently used on the [Forge's Bazaar](https://forge-vtt.com/bazaar).

#### Icon
- Width: 512px
- Aspect Ratio: 1:1

Fallback on the Bazaar if Cover is not defined.

#### Screenshot
Anything that should go into an `<img>` HTML element: `.png`, `.gif`, `.webp`. Try to keep the file size under 1MB, definitely no more than 10MB. Gifs in particular will probably need to be larger, but know that the larger the image the longer it will take to load.

#### Video
Anything that should go into an `<video>` HTML element: `.mp4`, `.webm`.

