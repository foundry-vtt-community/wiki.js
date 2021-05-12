---
title: Package Manifest+
description: An expanded manifest format.
published: true
date: 2021-05-12T04:19:31.539Z
tags: manifest, manifest+
editor: markdown
dateCreated: 2020-12-02T04:47:58.438Z
---

The Manifest+ specification is an unofficial community developed extension to the official Foundry VTT manifest 
specification for module and system development intended to add additional data which can be used by packages within 
Foundry VTT and by external applications.

This specification is developed by The League of Extraordinary Foundry Developers. The core Foundry VTT software 
simply ignores these additional manifest fields. A number of external applications including [The Bazaar](https://forge-vtt.com/bazaar) package browser on 
The Forge and [Foundry Hub](https://www.foundryvtt-hub.com/packages/) use this extended data to give the general public a rich experience by adding cover images, icons, 
enhanced contact details, and more.

<a name="package-manifest-standard-fields"></a>
## Package Manifest Standard Fields
The Foundry VTT manifest contains a number of required and optional fields which act as metadata for your package. The 
official fields are documented in the core documentation for modules and systems respectively:

- [Introduction to Module Development](https://foundryvtt.com/article/module-development/)
- [Introduction to System Development](https://foundryvtt.com/article/system-development/)

Manifest+ does not replace the standard manifest, but enhances it. Most of the fields added do not touch any of 
the core attributes at all except the `authors` field.

<a name="package-manifest-standard-fields-authors"></a>
### Authors

The core Foundry VTT manifest specification has two ways of defining the package author; either through the `author` 
field which expects a simple string or the more complex `authors` field. We *highly* recommend including the `authors` 
field in your package as it provides much more flexibility.

> Currently, while `authors` is a part of the standard manifest Foundry VTT does not actually utilize this field, so you 
> should also include the `author` field until such a time as that changes. {.is-info}

The `authors` field is an array of objects with each object providing information about one of the authors of the 
package. This could be one author or many. The standard version includes `name`, `url`, and `email` fields. Only `name` 
is required.

For Manifest+ we want to recognize that a personal website and email address are not necessarily the best or only 
ways to contact the author. To that end, we introduce `discord`, `ko-fi`, `patreon`, `reddit`, and `twitter` fields as 
well.

```json
"author": "Name of the author",
"authors": [
  {
    "name": "Name of the author",
    "url": "https://website.com/of/the/author",
    "email": "email@example.com",
    "discord": "discordID#0001",
    "ko-fi": "kofiName",    
    "patreon": "patreonName",
    "reddit": "u/RedditUsername",
    "twitter": "@TwitterHandle",
  }
]
```
Each of these additional fields follow the naming convention of the platform. For example with Twitter handles the 
`@TwitterHandle` format is used.


<a name="manifest"></a>
## Manifest+

All Manifest+ fields are *optional*, but they are all useful. We recommend including as many of these fields as 
reasonable in order to provide enriched metadata for your package.

<a name="manifest-version"></a>
### Version
Document Version: 1.2.0

It is recommended to include a `manifestPlusVersion` field in your manifest to denote which version of Manifest+ you 
have implemented. Breaking changes are not intended for this specification, but if they do occur the major version 
will be incremented in accordance with [semantic versioning](https://semver.org/).

```json
"manifestPlusVersion": "1.2.0"
```


<a name="manifest-media"></a>
### Media
One of the largest additions, the `media` field, is an array of objects that each provide data for a single 
multimedia item. This data includes a `type` field which indicates what kind of media is being provided as 
well as an `url` field which provides the address of the media resource. An optional `caption` field is available
to describe your multimedia item. 

```json
"media": [
  {
    "type": "screenshot",
    "url": "link/to/media/file",
    "caption": "<Insert description of screenshot>"
  },
  {
    "type": "cover",
    "url": "https://somereposite.com/author/repo/raw/images/cover.png",
    "caption": "<Insert module / system name>"
  },
  {
    "type": "video",
    "url": "https://somereposite.com/author/repo/raw/videos/demo.webm",
    "loop": true,
    "thumbnail": "https://somereposite.com/author/repo/raw/images/thumb.png",
    "caption": "<Insert description of a cool feature from the video>"
  }
]
```

<a name="manifest-media-media-types"></a>
#### Media Types
The following type of media are defined by the Manifest+ specification:
- `cover` - A cover image which is intended to be displayed along with the package description.
- `icon` - A small image icon such as a logo or author avatar.
- `screenshot` - An image of the package in action.
- `video` - A video file which can be played, or the URL of an embeddable video (such as Youtube or Vimeo)
    - `loop` - Optional field specific to video media. If loop is set to true, the video is expected to be treated as 
      an animated image, like a GIF (i.e. muted and looped).
    - `thumbnail` - Optional URL to provide a video thumbnail.

<a name="manifest-media-media-recommendations"></a>
#### Media Recommendations
There is no guarantee how the media files will be used, but these are the recommended dimensions and known existing 
usages.

<a name="manifest-media-caption"></a>
#### Caption
Every media type accepts an optional caption field that can be used to describe what the media shows. It is up to the 
consumer of the Manifest+ specification to determine how it is used. The caption could be used as an alt image tag or as
an actual caption below the media type itself.

<a name="manifest-media-caption-cover"></a>
##### Cover
Avoid putting large text on the cover image as it should showcase the package rather than the name of the package.

- Width: 1280px
- Aspect Ratio: 2:1

This is currently used on the [Forge's Bazaar](https://forge-vtt.com/bazaar).

<a name="manifest-media-caption-icon"></a>
##### Icon
- Width: 512px
- Aspect Ratio: 1:1

Fallback on the Bazaar if `cover` is not defined.

<a name="manifest-media-caption-screenshot"></a>
##### Screenshot
Anything that should go into an `<img>` HTML element: `.png`, `.gif`, `.webp`. Try to keep the file size under 1MB, 
definitely no more than 10MB. Gifs in particular will probably need to be larger, but know that the larger the image 
the longer it will take to load.

<a name="manifest-media-caption-video"></a>
##### Video
Anything that should go into an `<video>` HTML element: `.mp4`, `.webm`.

Additionally, some Manifest+ consumers may supported embedding video from common providers like YouTube and Vimeo. 
The address of the video can be provided in the `url` field. Consumers should take care to parse this field 
appropriately to avoid loading a YouTube video in a `<video>` element or an `.mp4` in a YouTube embed.

<a name="manifest-media-caption-video-thumbnail"></a>
##### Video Thumbnail
Should be a static image.

- Dimensions: 1280px by 720px


<a name="manifest-library"></a>
### Library
The `library` field is a boolean that indicates whether the package is a library intended for other packages to 
depend on and consume. This field should be `true` if your package does not present any user-facing features, but 
rather provides functionality for other packages to utilize and rely upon. Packages with this field set to `true` 
may be hidden from third party package lists to avoid confusing users.

```json
	"library": true,
```

When omitted the default value of this field is `false`. It is not necessary to explicitly set this value unless 
it needs to be `true`.

> The `library` key might be coming to Foundry Core in 0.8.x. 
> [Issue on Gitlab.](https://gitlab.com/foundrynet/foundryvtt/-/issues/4129) {.is-info}


<a name="manifest-includes"></a>
### Includes
This field is intended to allow developers to create improved deployment tools. The `includes` field is an array 
of strings where each string is a relative file path that should be included in the package zip archive. Special CI / 
CD tools can use this field along with the official fields for scripts, languages, and styles to generate the archive 
with only the desired files without needing to create an additional configuration file.

```json
"includes": [
   "relative/path/to/files/script.js", 
   "relative/path/to/templates/template.html", 
   "path/to/image/assets/folder"
]
```


<a name="manifest-deprecated"></a>
### Deprecated
This field is intended to be added to the manifest of a package that is no longer being maintained and / or is no 
longer functional / useful. This may be because the author created a new better alternative, or the features are 
integrated into the Foundry VTT core software, or an associated system assumes the same functionality. Perhaps you as 
the author have abandoned the package and no longer intend to keep it up to date.

The `deprecated` field is an object which contains a number of fields explaining the nature of the deprecation status.

<a name="manifest-deprecated-fields"></a>
#### Fields
- `coreVersion` - If set the package is assumed to be a module that has been deprecated by a Foundry VTT core update. 
  This field is the core version number as a string.
- `reason` - A human-readable string explaining why the package was deprecated.
- `alternatives` - An array of objects each providing data about another package which could act as a replacement for 
  deprecated package.
    - `alternatives.name` - The `name` of the alternative package.
    - `alternatives.manifest` - The URL of the manifest file for the alternative package from which it can be 
      downloaded.

```json
"deprecated": {
  "coreVersion": "0.7.0",
  "reason": "This was added to foundry core.",
  "alternatives": [
    {
      "name": "module-name",
      "manifest": "https://link.com/to/module.json"
    }
  ]
}
```


<a name="manifest-conflicts"></a>
### Conflicts
The `conflicts` field is similar to the `dependencies` field in the core Foundry VTT manifest specification, but 
provides a mapping of packages which can not interoperate with the given package. It is an array of objects which define 
one or more packages that may pose conflicts. Each conflict object has a `name` field which matches the name of the 
other package, and a `type` which is the type of the other package. Additionally, the optional `versionMin` and 
`versionMax` fields can be used to define a range of version numbers for the other package within which the 
conflict occurs.

```json
"conflicts": [
  {
    "name": "module-name",
    "type": "module",
    "versionMin": "a.b.c",
    "versionMax": "a.c.d"
  }
]
```



## Changelog
#### version: 1.1.0
- Added `ko-fi` to `authors`, expected to be only the ko-fi username.

#### version: 1.2.0
- Added `caption` field to the `media` types.
