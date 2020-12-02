---
title: Package Manifest+
description: An expanded manifest format.
published: true
date: 2020-12-02T06:06:25.245Z
tags: 
editor: markdown
dateCreated: 2020-12-02T04:47:58.438Z
---

The Manifest+ specification is an unofficial community developed extension of the official Foundry manifest intended to add additional data which can be used by packages within Foundry and by external application.

This specification was developed by The League of FVTT developers. While the Core Foundry software simply ignores these additions, the full data of the manifest is still available to packages running within Foundry. Additionally, a number of external applications including The Bazaar package browser on The Forge use this extended data to give users a more rich experience by adding cover images, icons, enhanced contact details, and more.

## Package Manifest Standard Fields
The Foundry manifest contains a number of required and optional fields which act as metadata for your package. The official properties are documented in the official Core docs at the [Introduction to Module Development](https://foundryvtt.com/article/module-development/) page.

Manifest+ does not replace the standard manifest, but enhances it. Most of the properties added here don't touch any of the Core files at all. The exception is the `authors` field.

### Authors

The Core manifest has two ways of defining the package Author. Either through the `author` field which expects a simple string, or through the more complex `authors` filed. We *highly* recommend including the `authors` field in your package as it provides much more flexibility.

*Note: Currently while `authors` is a part of the standard manifest, Foundry does not actually utilize this field, and you should also include the `author` field until such a time as that changes.*

The `authors` field is an array of objects, each object provides information about one of the authors of the package. This could be one author, or many. The standard version includes `name`, `website`, and `email` fields. Only `name` is required.

For Manifest+, we wanted to recognize that a personal website and Email address are not nessesarily the best/only ways to contact the author. To that end, we introduce `discord`, `twitter`, and `reddit` fields as well.

```json
"author": "Name of the author",
"authors": [
	{
		"name": "Name of the author",
		"url": "https://website.com/of/the/author",
		"email": "email@example.com",
		"discord": "discordID#0001",
		"twitter": "@TwitterHandle",
		"reddit": "u/RedditUsername"
	}
]
```
Each of these additional fields follow the naming convention of the platform, for example with Twitter handles the `@TwitterHandle` format is used.

## Manifest+

All Manifest+ properties are *optional* but they are all usful. We recommend including as many of these properties as reasonable in order to provide enriched metadata.

### Media
One of the largest additions, the `media` preperty is an array of objects which each provide data for a single multimedia item. This data includes a special `type` field which indicates what kind of media is being provided, as well as a `url` property which provieds the address of the media resource.

```json

"media": [
	{
		"type": "screenshot",
		"url": "link/to/media/file"
	},
  {
		"type": "cover",
		"url": "https://somereposite.com/author/repo/raw/images/cover.png"
	}
]
```

#### Media Types
The following type of media are defined by the Manifest+ specification:
- `"cover"` - A "cover image" which is intended to be displayed along with the package description.
- `"icon"` - A small image icon such as a logo or author avatar.
- `"screenshot"` - An image of the package in action.
- `"video"` - A video file which can be played.

#### Media Recommendations
There is no guarantee how the media files will be used, but these are the recommended dimensions and known existing usages.

Media `url`s can be relative to the package location, however we recommend absolute URLs to resources available online for media that may be displayed somewhere that the entire package contents may not be available.

##### Cover
Avoid putting large text on the cover image, it should showcase the package rather than the name of the package.

- Width: 1200px
- Aspect Ratio: 2:1

Currently used on the [Forge's Bazaar](https://forge-vtt.com/bazaar).

##### Icon
- Width: 512px
- Aspect Ratio: 1:1

Fallback on the Bazaar if Cover is not defined.

##### Screenshot
Anything that should go into an `<img>` HTML element: `.png`, `.gif`, `.webp`. Try to keep the file size under 1MB, definitely no more than 10MB. Gifs in particular will probably need to be larger, but know that the larger the image the longer it will take to load.

##### Video
Anything that should go into an `<video>` HTML element: `.mp4`, `.webm`.
```
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



