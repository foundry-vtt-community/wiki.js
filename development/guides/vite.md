---
title: Using Vite to build for Foundry
description: a guide~
published: true
date: 2021-08-17T06:55:11.373Z
tags: development, javascript
editor: markdown
dateCreated: 2021-08-17T05:52:46.709Z
---

For various reasons — performance on large systems, using compile-to-JS languages like Typescript, etc. — you may wish to use a bundler[^1] when writing your system or module. 

This article will walk you through an example of using Vite to bundle for Foundry, drawing on our experience using it for the Lancer system. 

# Why?

Vite taps into the state-of-the-art Rollup ecosystem for production builds. In the Lancer system, we build about 215kb of typescript system code + 10mb of runtime `node_modules` into a 500kb final bundle, after code splitting and tree shaking, in 15s. 

More interesting, though, is the vite dev server. It starts up instantly and sits in front of your foundry server, intercepting requests to your module or system. This allows it to not just immediately serve changed files, but in many cases, to *hot-reload* the corresponding modules without having to restart your foundry session or even closing the modal you're working on. 

Caveats up front: this hot-reloading takes a small amount of work to enable for bog-standard ES modules. However, it is automatic for CSS and for many component frameworks (Svelte, for instance.)

# How?

### Production Builds

After a quick `npm install --save-dev vite`, the core of the configuration file will be `vite.config.js`. Vite is also fully typed in Typescript, so you can use its config type in a `vite.config.ts` if you like — that's what we'll demonstrate here.

The core of a foundry vite config is as follows:

`vite.config.ts`:
```typescript
import type { UserConfig } from 'vite';
const config: UserConfig = {
  public: 'public',
  base: '/systems/lancer/',
  server: {
    port: 30001,
    open: true,
    proxy: {
      '^(?!/systems/lancer)': 'http://localhost:30000/',
      '/socket.io': {
        target: 'ws://localhost:30000',
        ws: true,
      },
    }
  },
  build: {
    outDir: 'dist',
    emptyOutDir: true,
    sourcemap: true,
    lib: {
      name: 'lancer',
      entry: 'lancer.ts',
      formats: ['es'],
      fileName: 'lancer'
    }
  },
}

export default config;
```

This is what sets up the vite dev server as a proxy on top of foundry. Read `base` as: mount all source files under `/systems/lancer/`, and read the `server.proxy` section as: proxy all urls that do not begin with `/systems/lancer` to foundry, and the `/socket.io` url to foundry's websocket server. 

It also sets up the build under "library mode" defaults, making sure to compile to the modern esmodule standard. Vite will compile typescript/coffeescript/scss/sass/etc automatically, as long as the corresponding compiler is installed. `build.lib.entry` is the initial source file of your application, the one that eventually depends on everything your application needs.

This includes your style files. Vite assumes your build's entrypoint will eventually depend on everything it needs to process. If you haven't been coding under this assumption, the easiest way to satisfy this is to add a line to your entrypoint like 

```typescript
import './lancer.scss';
```

You will also probably need to include certain files in the build as-is, without being processed by vite. These files can be placed in the `public` folder, to be copied without modification during production builds, and to be served untouched at the root during the dev server. `public/system.json` is also a convenient place to put `system.json`.

What should this `system.json` point to? When built, this vite build will produce the entrypoints `dist/index.js` and `dist/style.css`, and so your `system.json` can refer to those files.

This is sufficient to use vite as a bundler: `npx vite build` will build your module/system into `dist`. A couple more steps are necessary to use the vite development server.

### Development Server

The dev server assumes that `index.html` is your application's entrypoint (it doesn't read `build.lib.entry`). Since we know this file will be mounted at `/systems/lancer/index.html`, we create a simple file that redirects to `/` — i.e., the url that will be proxied to `/` for foundry, the login screen.

`index.html`:
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Initial Index (for Vite)</title>
  </head>
  <body>
    <script>
      window.location = "/";
    </script>
  </body>
</html>
```

From here, we set up a dev environment within foundry as normal. 

Just a couple more steps: since we've told foundry, via `system.json`, to look for `/systems/lancer/index.js` and `/systems/lancer/style.css`, we should make those files.

`index.js`:
```javascript
// only required for dev
// in prod, foundry loads index.js, which is compiled by vite/rollup
// in dev, foundry loads index.js, this file, which loads lancer.ts

window.global = window; // some of your dependencies might need this
import * as LANCER from './lancer.ts';
```

If you've imported your \*css from your code files as suggested above, this will be sufficient, as the vite dev server will also see this import and load the corresponding css. In this case the only reason to also make a `style.css` is to silence the console error during development.

`style.css`:
```css
// only required in dev

// in prod, foundry loads style.css, as compiled by vite/rollup,
// mostly from the `import lancer.scss` line at the top of lancer.ts

// in dev, foundry loads style.css, this file, which is a no-op
// in dev, lancer.ts imports lancer.scss directly
```

Now you can start foundry, then start `npx vite serve`, to launch the vite dev server.

### Hot Module Replacement

On the average project, the vite server will only hot-reload css by default. If you happen to be using a component library, like Svelte or Vue, their plugins do come with hot-reloading support; even preserving state across hot-reloads.

However, mostly we use plain javascript modules, in foundry development. To hot-reload an ES module, you need to transform it into code that looks like this:

```javascript
let _foo = ...;
export { _foo as foo };

// this exact if statement guarantees this will be tree-shaken out in prod
if (import.meta.hot) {
   import.meta.hot.accept(newModule => {
     _foo = newModule.foo;
   });
}
```

This requires that all your exports are `export let`s, rather than `export const`s. If this is acceptable, the hot-reloading code can be committed to your repository, as it will be tree-shaken out and not appear in production builds. If you want your modules to `export const`, you can still do this transformation just while you're working on the code, of course.

See Vite's [HMR api](https://vitejs.dev/guide/api-hmr) for more details.

# Appendices

Here are some gotchas we encountered, and how to fix them.

### Changing the root directory

Our source files actually live in `src/`, not `./`. You may assume this is a simple matter of setting `root: "src"`. However, some other paths in `vite.config.ts` are relative to this, and so we found the best solution was just to use absolute paths via `require('path').resolve(__dirname, "...")`.

### `@aws-sdk`

If you happen to be using `@aws-sdk`, there is a [bug](https://github.com/aws-amplify/amplify-js/issues/7499) in the current version of the library that can be worked around with:

```typescript
  resolve: {
    alias: [{
      find: "./runtimeConfig",
      replacement: ("./runtimeConfig.browser"),
    }]
  },
```

`resolve` is a highly powerful hammer to solve general bundling problems in `vite`. It allows you to completely customise what any import in your whole system, including your dependencies, resolves to.

### Cyclic dependency issues

A rare kind of cyclic dependency in ES modules triggers a [bug](https://github.com/evanw/esbuild/issues/1433) in the current version of `esbuild`, as used in the vite dev server. If this occurs to you, you will see your dev server crash during loading, with errors in your console about some dependency being undefined.

To fix this, add the module to `optimizeDeps.exclude`. However, if that module has any commonJS dependencies, you will then need to add them to `optimizeDeps.include`. 

### `vite-plugin-svelte`

If you're using `vite-plugin-svelte` from within an ES module, you should know that it expects to import a commonjs file. To handle that, you will need to rename that file to `.cjs` and explicitly name it as the config file for the plugin.

### Full `vite.config.ts`

`vite.config.ts`:
```typescript
import type { UserConfig } from 'vite';
import { svelte } from '@sveltejs/vite-plugin-svelte';
import { visualizer } from 'rollup-plugin-visualizer';
const path = require('path');

const config: UserConfig = {
  root: 'src/',
  base: '/systems/lancer/',
  publicDir: path.resolve(__dirname, 'public'),
  server: {
    port: 30001,
    open: true,
    proxy: {
      '^(?!/systems/lancer)': 'http://localhost:30000/',
      '/socket.io': {
        target: 'ws://localhost:30000',
        ws: true,
      },
    }
  },
  resolve: {
    alias: [{
      find: "./runtimeConfig",
      replacement: ("./runtimeConfig.browser"), // thanks @aws-sdk
    }]
  },
  optimizeDeps: {
    exclude: ['machine-mind'], // machine-mind triggers https://github.com/evanw/esbuild/issues/1433
    include: ['lancer-data', 'jszip', 'axios'], // machine-mind's cjs dependencies
  },
  build: {
    outDir: path.resolve(__dirname, 'dist'),
    emptyOutDir: true,
    sourcemap: true,
    brotliSize: true,
    lib: {
      name: 'lancer',
      entry: path.resolve(__dirname, 'src/lancer.ts'),
      formats: ['es'],
      fileName: 'lancer'
    }
  },
  plugins: [
    svelte({
      configFile: '../svelte.config.cjs', // relative to src/
    }),
    visualizer({
      gzipSize: true,
      template: "treemap",
    })
  ]
};

export default config;

```

[^1]: A bundler generally adds a compile step to your javascript package, producing output javascript from your input code, after having gone through many transformations (including the eponymous bundling of multiple files together to improve download times). 