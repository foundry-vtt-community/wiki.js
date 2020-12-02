---
title: Package Manifest+
description: An expanded manifest format.
published: true
date: 2020-12-02T17:44:38.276Z
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

> Currently while `authors` is a part of the standard manifest, Foundry does not actually utilize this field, and you should also include the `author` field until such a time as that changes.
{.is-info}

The `authors` field is an array of objects, each object provides information about one of the authors of the package. This could be one author, or many. The standard version includes `name`, `url`, and `email` fields. Only `name` is required.

For Manifest+, we wanted to recognize that a personal website and Email address are not nessesarily the best/only ways to contact the author. To that end, we introduce `discord`, `twitter`, `patreon`, and `reddit` fields as well.

```json
"author": "Name of the author",
"authors": [
  {
    "name": "Name of the author",
    "url": "https://website.com/of/the/author",
    "email": "email@example.com",
    "discord": "discordID#0001",
    "twitter": "@TwitterHandle",
    "patreon": "patreonName",
    "reddit": "u/RedditUsername"
  }
]
```
Each of these additional fields follow the naming convention of the platform, for example with Twitter handles the `@TwitterHandle` format is used.

## Manifest+

All Manifest+ properties are *optional* but they are all usful. We recommend including as many of these properties as reasonable in order to provide enriched metadata.

### Version
Document Version: 1.0.0

It is recommended to include a `manifestPlusVersion` property in your manifest which denotes which version of Manifest+ you have implemented. We do not intend for breaking changes to this spec, but if they do happen we will increment the major version in accordance with [Semver](https://semver.org/).

```json
"manifestPlusVersion": "1.0.0"
```

### Media
One of the largest additions, the `media` preperty is an array of objects which each provide data for a single multimedia item. This data includes a special `type` field which indicates what kind of media is being provided, as well as a `url` property which provides the address of the media resource.

```json
"media": [
  {
    "type": "screenshot",
    "url": "link/to/media/file"
  },
  {
    "type": "cover",
    "url": "https://somereposite.com/author/repo/raw/images/cover.png"
  },
  {
    "type": "video",
    "url": "https://somereposite.com/author/repo/raw/videos/demo.webm",
    "loop": true,
    "thumbnail": "https://somereposite.com/author/repo/raw/images/thumb.png"
  }
]
```

#### Media Types
The following type of media are defined by the Manifest+ specification:
- `"cover"` - A "cover image" which is intended to be displayed along with the package description.
- `"icon"` - A small image icon such as a logo or author avatar.
- `"screenshot"` - An image of the package in action.
- `"video"` - A video file which can be played, or the address of an embeddable video (such as Youtube or Vimeo)
	- `"loop"` - Optional Field specific to Video type media. If loop is true, the video is expected to be treated as an animated image, like a GIF (i.e. muted and looped).
  - `"thumbnail"` - Optional url to provide a video thumbnail.

#### Media Recommendations
There is no guarantee how the media files will be used, but these are the recommended dimensions and known existing usages.

Media `url`s can be relative to the package location, however we recommend absolute URLs to resources available online for media that may be displayed somewhere that the entire package contents may not be available.

##### Cover
Avoid putting large text on the cover image, it should showcase the package rather than the name of the package.

- Width: 1280px
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

Additionally, some Manifest+ consumers may supported embedding video from common providers like YouTube and Vimeo, the address of the video can be provided in the `url` field. Consumers should take care to parse this field appropriately to avoid loading a YouTube video in a `<video>` element, or an `.mp4` in a YouTube embed.

##### Video Thumbnail
Should be a static image.

- Dimensions: 1280px by 720px

### Library
The `library` property is a boolean field that indicates whether or not the package is "library" intended for other packages to depend on. This property should be `true` if your package doesn't present any user-facing features, but rather provides functionality for other packages to utilize and rely upon. Packages with this property set to `true` may be hidden from third party package lists to avoid confusing users.
```json
	"library": true,
```
When omitted, the default value of this property is `false` so it is not nessesary to explcitly set this value unless it needs to be `true`.

> The `library` key might be coming to Foundry Core in 0.8.x. [Issue on Gitlab.](https://gitlab.com/foundrynet/foundryvtt/-/issues/4129)
{.is-info}

### Includes
This special property is intended to allow developers to create improved deployment tools. The `includes` property is an array of strings, each string should contain the relative path of a file which should be included in the package .zip archive. Special CI/CD tools can use this field along with the official fields for scripts, laguages, and styles to generate the archive with only the desired files, without needing to create an additional configuration file.
```json
"includes": [
	"relative/path/to/files/script.js", 
	"relative/path/to/templates/template.html", 
	"path/to/image/assets/folder"
]
```
### Deprecated
This property is intended to be added to the manifest of a package that is no longer being maintained, and is no longer functional/useful. This may be because the author created a new better alternative, because the features were integrated into Core or a System, or because the author has abandoned the package and no longer intends to keep it up to date. 

The `deprecated` property is an object which contains a number of fields explaining the nature of the deprecation status.

#### Fields
- `"coreVersion"` - If set, this module is assumed to have been deprecated by a core update, this field is the core version number as a string.
- `"reason"` - A human readable string explaining why the package was deprecated.
- `"alternatives"` - An array of objects each providing data about another package which could act as a replacement for this one.
 	- `alternatives.name` - The `name` property of the alternative package.
 	- `alternatives.manifest` - The URL of the `manifest.json` file for the alternative package, from which it can be downloaded.

```json
"deprecated": {
  "coreVersion": "0.7.0",
  "reason": "This was added to foundry core.",
  "alternatives": [
    {
      "name": "module_name",
      "manifest": "https://link.com/to/manifest.json"
    }
  ]
}
```
### Conflicts
Similar to the `dependencies` in the standard manifest, but the opposite. The `conflicts` property is an array of objects which define another package with which this package can not inter-opperate. 

Each conflic object has a `name` property which matches the name of the other package, and a `type` which is the type of the other package.

Additionally, the optional `versionMin` and `versionMax` properties can be used to define a range of version numbers for the other package withing which the conflic occurs.

```json
"conflicts": [
  {
    "name": "module_name",
    "type": "module",
    "versionMin": "a.b.c",
    "versionMax": "a.c.d"
  }
]
```



