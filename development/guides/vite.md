---
title: Using Vite to build for Foundry
description: everything you ever wanted from hot module replacement but were afraid to ask
published: true
date: 2025-07-04T15:47:13.513Z
tags: development, javascript
editor: markdown
dateCreated: 2021-08-17T05:52:46.709Z
---

For various reasons — performance on large systems, using compile-to-JS languages like Typescript, etc. — you may wish to use a bundler[^1] when writing your system or module. 

This article will walk you through an example of using Vite to bundle for Foundry, drawing on our experience using it for the Lancer system. 

> **Note**
The lancer system uses an old version of svelte which mandates the use of an old version of vite.  Some parts of this guide may not fully match your experience in a green field project.  Updates for recent versions of vite are in progress.
{.is-warning}

# Why?

Vite taps into the state-of-the-art Rollup ecosystem for production builds. In the Lancer system, we build about 215kb of typescript code + 10mb of runtime `node_modules` into a 500kb final bundle, after code splitting and tree shaking, in 15s. 

More interesting, though, is the vite dev server. It starts up instantly and sits in front of your foundry server, intercepting requests to your module or system. This allows it to not just immediately serve changed files, but in many cases, to *hot-reload* the corresponding modules without having to restart your foundry session or even closing the modal you're working on. 

Caveats up front: this hot-reloading takes a small amount of work to enable for bog-standard ES modules. However, it is automatic for CSS and for many component frameworks like Svelte.

# How?

## Production Builds

After a quick `npm install -D vite`, the core configuration file is `vite.config.js`. Vite is also fully typed in Typescript, so you can use its config type in a `vite.config.ts` if you like — that's what we'll demonstrate here.

Start with something like this.

`vite.config.ts`:
```javascript
import { defineConfig } from 'vite';

export default defineConfig({
  publicDir: "public", // This is the default
  base: "/systems/lancer/",
  server: {
    port: 30001,
    open: "/",
    proxy: {
      "^(?!/systems/lancer)": "http://localhost:30000/",
      "/socket.io": {
        target: "ws://localhost:30000",
        ws: true,
      },
    }
  },
  esbuild: { keepNames: true },
  build: {
    outDir: "dist", // This is the default
    emptyOutDir: false,
    sourcemap: true,
    lib: {
      name: "lancer",
      entry: "src/lancer.ts",
      formats: ["es"],
      fileName: "lancer"
    }
  },
}
```

The `server` section sets up the vite dev server as a proxy on top of foundry. Read `base` as: mount all source files under `/systems/lancer/`, and read the `server.proxy` section as: proxy all urls that do not begin with `/systems/lancer` to foundry, and the `/socket.io` url to foundry's websocket server. 

The `build` section sets up the build under "library mode" defaults, making sure to compile to the modern esmodule standard. `build.lib.entry` is the initial source file of your application, the one that eventually `import`s everything your application needs.

This needs to include your style files. Vite assumes your build's entrypoint will eventually depend on everything it needs to process. If you haven't been coding under this assumption, the easiest way to satisfy this is to add a line to your entrypoint like 

```typescript
import './lancer.scss';
```

You will also probably need to include certain files in the build as-is, without being processed by vite. These files can be placed in the `public` folder, to be copied without modification during production builds, and to be served untouched at the root during the dev server. `public/system.json` is also a convenient place to put `system.json`.

What should this `system.json` point to? When built, this vite build will produce the entrypoints `dist/lancer.mjs` and `dist/lancer.css`, and so your `system.json` can refer to those files.

This is sufficient to use vite as a bundler: `npx vite build` will build your module/system into `dist`. You may also wish at this point, if you haven't done so already, to link your system's folder within foundry to this `dist` folder, like `ln -s ~/path/to/foundryvtt-lancer/dist ~/.local/share/FoundryVTT/Data/systems/lancer `.

```
$ npx vite build
vite v2.5.0 building for production...
✓ 963 modules transformed.
rendering chunks (9)...

dist/lancer.es.js     0.04 KiB / brotli: 0.05 KiB
dist/lancer.es.js.map 0.09 KiB
dist/aws-exports.js   0.92 KiB / brotli: 0.42 KiB
dist/aws-exports.js.map 2.32 KiB
dist/index.js         1.69 KiB / brotli: 0.70 KiB
dist/index.js.map     5.04 KiB
dist/marked.js        46.42 KiB / brotli: 12.97 KiB
dist/marked.js.map    137.94 KiB
dist/index4.js        52.08 KiB / brotli: 14.08 KiB
dist/index4.js.map    238.45 KiB
dist/Credentials.js   138.40 KiB / brotli: 29.90 KiB
dist/Credentials.js.map 968.75 KiB
dist/style.css        36.59 KiB / brotli: 7.38 KiB
dist/index2.js        144.89 KiB / brotli: 34.89 KiB
dist/index2.js.map    602.29 KiB
dist/index3.js        179.36 KiB / brotli: 33.85 KiB
dist/index3.js.map    1536.81 KiB
dist/lancer.js        1030.44 KiB / brotli: skipped (large chunk)
dist/lancer.js.map    1777.34 KiB
```

## Development Server

A couple more steps are necessary to use the vite development server.

From here, we set up a dev environment within foundry as normal. The underlying `systems/lancer` folder still needs to exist under the foundry data directory, as foundry directly checks the filesystem for a `system.json`, but all requests for files within that directory by the browser will be intercepted by the vite dev server.

Since we've told foundry, via `system.json`, to look for `/systems/lancer/lancer.mjs` and `/systems/lancer/lancer.css`, we should make those files.

`lancer.js`:
```javascript
// only required for dev
// in prod, foundry loads lancer.mjs, which is compiled by vite/rollup
// in dev, foundry loads lancer.mjs, this file, which loads lancer.ts

import './src/lancer.ts';
```

If you've imported your \*css from your code files as suggested above, this will be sufficient, as the vite dev server will also see this import and load the corresponding css. In this case the only reason to also make a `style.css` is to silence the console error during development.

`lancer.css`:
```css
/**
 * only required in dev
 * in prod, foundry loads style.css, as compiled by vite/rollup,
 * mostly from the `import lancer.scss` line at the top of lancer.ts
 *
 * in dev, foundry loads style.css, this file, which is a no-op
 * in dev, lancer.ts imports lancer.scss directly
 */
```

Now you can start foundry, then start `npx vite`, to launch the vite dev server.

```
$ npx vite serve

  vite v2.5.0 dev server running at:

  > Local: http://localhost:30001/systems/lancer/
  > Network: use `--host` to expose

  ready in 205ms.
```

### Hot Module Replacement

On the average project, the vite server will only hot-reload css by default. If you happen to be using a component library, like Svelte or Vue, their plugins do come with hot-reloading support; even preserving state across hot-reloads.

Certain code can be easily set up for hot reloading, particularly Hooks.  While hooks like `init`, `setup` and `ready` are only executed once during foundry startup, things like `hotBarDrop` can be easily swapped out to iterate through handling dropping to the hotbar without a full reload.  With the following in the entrypoint changes to `src/hooks/hotbar.mjs` will no longer trigger a full page reload, and instead any hotbar drops will switch to using the new hook code.

```javascript
import { onHotbarDrop } from "./hooks/hotbar.mjs";

// ...

const hot_hooks = {};
hot_hooks.hotbar = Hooks.on("hotbarDrop", onHotbarDrop);

if (import.meta.hot) {
  import.meta.hot.accept("./hooks/hotbar.mjs", module => {
    Hooks.off("hotbarDrop", hot_hooks.hotbar);
    hot_hooks.hotbar = Hooks.on("hotbarDrop", module.onHotbarDrop);
  });
}
```

When building for release, Rollup will be able to detect that the hot reload code and the hot_hoot hooks object are dead code and remove them from the compiled output.

Things like Document classes and DataModels are more tied into the running state of the webapp, and thus it makes more sense to let changes there trigger a full reload.

See Vite's [HMR api](https://vitejs.dev/guide/api-hmr) for more details.

## Plugins

Vite comes with support for Typescript, JSX, PostCSS, CSS Modules, and WASM out of the box. It also supports other css preprocessors without requiring a specific plugin, but you have to `npm install --save-dev sass` or `less` or `stylus` as you require. By default, all files in `publicDir` (`public/` by default) are copied to the output, removing the need for a copy plugin such as `rollup-plugin-copy`.

For more complicated needs, there are many high quality plugins available, like `@vitejs/plugin-vue` and `@sveltejs/vite-plugin-svelte`, and for bespoke needs, it's possible to write a new plugin directly in the vite config or included as a module in your repository.

# Appendices

Here are some gotchas we encountered, and how to fix them.

## Changing the root directory

Our source files actually live in `src/`, not `./`. You may assume this is a simple matter of setting `root: "src"`. However, some other paths in `vite.config.ts` are relative to this, and so we found the best solution was just to use absolute paths via `require('path').resolve(__dirname, "...")`.

## `@aws-sdk`

If you happen to be using `@aws-sdk`, there is a [bug](https://github.com/aws-amplify/amplify-js/issues/7499) in the current version of the library that can be worked around with:

```typescript
  resolve: {
    alias: [{
      find: "./runtimeConfig",
      replacement: "./runtimeConfig.browser",
    }]
  },
```

`resolve` is a highly powerful hammer to solve general bundling problems in `vite`. It allows you to completely customise what any import in your whole system, including your dependencies, resolves to.

## Cyclic dependency issues

A rare kind of cyclic dependency in ES modules triggers a [bug](https://github.com/evanw/esbuild/issues/1433) in the current version of `esbuild`, as used in the vite dev server. If this occurs to you, you will see your dev server crash during loading, with errors in your console about some dependency being undefined.

To fix this, add the module to `optimizeDeps.exclude`. However, if that module has any commonJS dependencies, you will then need to add them to `optimizeDeps.include`. 

## `vite-plugin-svelte`

If you're using `vite-plugin-svelte` from within an ES module, you should know that it expects to import a commonjs file. To handle that, you will need to rename that file to `.cjs` and explicitly name it as the config file for the plugin.

## Full `vite.config.ts`

With all of the caveats above accounted for, the Lancer system's config file looks like this:

`vite.config.ts`:
```typescript
import type { UserConfig } from "vite";
import { svelte } from "@sveltejs/vite-plugin-svelte";
import { visualizer } from "rollup-plugin-visualizer";
import preprocess from "svelte-preprocess";
import checker from "vite-plugin-checker";
import path from "node:path";

const config: UserConfig = {
  root: "src/",
  base: "/systems/lancer/",
  publicDir: path.resolve(__dirname, "public"),
  server: {
    port: 30001,
    open: true,
    proxy: {
      "^(?!/systems/lancer)": "http://localhost:30000/",
      "/socket.io": {
        target: "ws://localhost:30000",
        ws: true,
      },
    },
  },
  resolve: {
    alias: [
      {
        find: "./runtimeConfig",
        replacement: "./runtimeConfig.browser",
      },
    ],
  },
  optimizeDeps: {
    include: ["@massif/lancer-data", "jszip" /* "axios" */], // machine-mind's cjs dependencies
  },
  build: {
    outDir: path.resolve(__dirname, "dist"),
    emptyOutDir: false,
    sourcemap: true,
    lib: {
      name: "lancer",
      entry: path.resolve(__dirname, "src/lancer.ts"),
      formats: ["es"],
      fileName: "lancer",
    },
  },
  esbuild: {
    minifyIdentifiers: false,
    keepNames: true,
  },
  plugins: [
    svelte({
      preprocess: preprocess(),
    }),
    checker({
      typescript: true,
      // svelte: { root: __dirname },
    }),
    visualizer({
      gzipSize: true,
      template: "treemap",
    }),
  ],
  define: {
    "process.env": process.env,
  },
};

export default config;

```

[^1]: A bundler generally adds a compile step to your javascript package, producing output javascript from your input code, after having gone through many transformations (including the eponymous bundling of multiple files together to improve download times). 