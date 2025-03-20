---
title: Improving Intellisense
description: Leveraging Foundry's type hints within VSCode
published: true
date: 2025-03-20T05:50:17.722Z
tags: 
editor: markdown
dateCreated: 2025-03-20T05:50:17.722Z
---

![Up to date as of v13](https://img.shields.io/static/v1?label=FoundryVTT&message=v13&color=informational)

One common issue when writing code for Foundry is that you're entirely dependent on an external library, which is not naturally included in VSCode's intellisense support. This guide covers how to configure your repository to dramatically improve the type hints.

**Note:** This guide is written for Foundry v13.338 and above, when Foundry fully migrated to ESM. Several key details differ for lower versions.

## 0. Prerequisites

This guide requires a local install of foundry that will be read and accessed. It also uses symlinks, which may require enabling in developer settings on your machine. It also assumes you've set your package folder up as a node package.

## 1. Symlinks

Before we begin, the files we want to symlink shouldn't be committed, so start by adding the following to your `.gitignore` file.

```.gitignore
foundry-config.yaml
foundry
```

Our next step is to create a `foundry-config.yaml` file. It doesn't need much - just the full path to our foundry install. The following is an example taken from a windows device. We've added this file to `.gitignore` because the path will differ from device-to-device. 
```yaml
installPath: "C:\\Program Files\\Foundry Virtual Tabletop\\Version 13"
```
You can commit an `example-foundry-config.yaml` file to your repository that will not be used directly but can be used as a reference. YAML has been chosen over JSON for this because you can add comments to this `example-foundry-config.yaml` file for instructional purposes.

This file is then used in the following script which will actually *create* the symlinks. For convention, you can place this in a `tools/create-symlink.mjs` file and add `"createSymlinks": "node ./tools/create-symlinks.mjs"` to the `scripts` section of your `package.json`, allowing you to just run `npm run createSymlinks`. You can even add `"postinstall": "npm run createSymlinks"` to automatically run this script after the package dependencies are installed on a new device.
```js
import * as fs from "fs";
import yaml from "js-yaml";
import path from "path";

console.log("Reforging Symlinks");

if (fs.existsSync("foundry-config.yaml")) {
  let fileRoot = "";
  try {
    const fc = await fs.promises.readFile("foundry-config.yaml", "utf-8");

    const foundryConfig = yaml.load(fc);

    // As of 13.338, the Node install is *not* nested but electron installs *are*
    const nested = fs.existsSync(path.join(foundryConfig.installPath, "resources", "app"));

    if (nested) fileRoot = path.join(foundryConfig.installPath, "resources", "app");
    else fileRoot = foundryConfig.installPath;
  } catch (err) {
    console.error(`Error reading foundry-config.yaml: ${err}`);
  }

  try {
    await fs.promises.mkdir("foundry");
  } catch (e) {
    if (e.code !== "EEXIST") throw e;
  }

  // Javascript files
  for (const p of ["client", "common"]) {
    try {
      await fs.promises.symlink(path.join(fileRoot, p), path.join("foundry", p));
    } catch (e) {
      if (e.code !== "EEXIST") throw e;
    }
  }

  // Language files
  try {
    await fs.promises.symlink(path.join(fileRoot, "public", "lang"), path.join("foundry", "lang"));
  } catch (e) {
    if (e.code !== "EEXIST") throw e;
  }
} else {
  console.log("Foundry config file did not exist.");
}
```

This script will also symlink in the `en.json` lang files from foundry; using them with the i18n-ally extension will be the subject of another guide.

Running this script *should* produce a `foundry` folder in the root of your package with three subfolders - `client`, `common`, and `lang`. 

## 2. jsconfig.json

The next part is setting up the actual type hints by creating a file named `jsconfig.json` in the root of your repository. The only line that needs adjustment is the `include` line; you should include the root of your esm files that you reference in your `module.json` or `system.json`.
```json
{
  "compilerOptions": {
    "module": "ES6",
    "target": "ES6",
    "paths": {
      "@client/*": ["./foundry/client/*"],
      "@common/*": ["./foundry/common/*"]
    },
  },
  "exclude": ["node_modules", "**/node_modules/*"],
  "include": ["your-package-root.mjs", "foundry/client/client.mjs"],
  "typeAcquisition": {
    "include": ["jquery"]
  }
}
```

Once this is done, you may need to restart vscode to see the changes. You should see that the `foundry` namespace now provides full intellisense support, e.g. `foundry.applications.api.DialogV2.prompt`. As of 13.338, most other global objects are constructed & assigned in such a way that they are not available.

## 3. Importing types

If you do need to import types, you can do that with `@` references, such as the following:
```js
/** @import {FormSelectOption} from "@client/applications/forms/fields.mjs" */
```

This can allow you to easily have full intellisense support for typing parameters and any other variable.