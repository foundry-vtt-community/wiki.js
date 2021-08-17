---
title: Using Vite to build for Foundry
description: a guide~
published: true
date: 2021-08-17T05:56:54.428Z
tags: development, javascript
editor: markdown
dateCreated: 2021-08-17T05:52:46.709Z
---

For various reasons — performance on large systems, using compile-to-JS languages like Typescript, etc. — you may wish to use a "bundler"[^1] when writing your system/module. 

This article will walk through an example of using Vite to bundle for Foundry, drawing on our experience using it for the Lancer system. 

# Why?

Vite taps into the state-of-the-art Rollup ecosystem for production builds. In the Lancer system, we build about 215k of typescript system code + 10mb of runtime `node_modules` into a 500k final bundle, after code splitting and tree shaking, in 15s. 

More interesting, though, is the vite dev server. It starts up immediately and sits in front of your foundry server, intercepting requests to your module or system. This allows it to not just immediately serve changed files, but in many cases, to *hot-reload* the corresponding modules without having to restart your foundry session or even closing the modal you're working on. 

Caveats up front: this hot-reloading takes a small amount of work to enable for bog-standard modules. However, it is automatic for CSS and for many component frameworks (we're experimenting with svelte, for instance.)

# How?

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

It also sets up the build under "library mode" defaults, making sure to compile to the modern esmodule standard. Vite will compile typescript/coffeescript/etc automatically, as long as the corresponding compiler is installed. `build.lib.entry` is the initial source file of your application, the one that eventually depends on everything your application needs.

This includes your css/scss/sass/less/etc. Vite assumes your build's entrypoint will also eventually depend on all your css. If you haven't been coding under this assumption, the easiest way to satisfy this is to add a line to your entrypoint like 

```typescript
import './lancer.scss';
```

There probably will be files you wish to include in the build that should be directly included, rather than being compiled or minified by vite. In this configuration, we place them in the `public` directory; vite copies them into `dist` during production builds and serves everything in them untouched at the root during the dev server.

When built, this will produce the entrypoints `dist/index.js` and `dist/style.css`, and so your `system.json` (now living in `public/system.json`) can refer to those files.

This is sufficient to use vite as a bundler. A couple more steps are necessary to use it as a development server.

---

The dev server assumes that `index.html` is your application's entrypoint; it doesn't read `build.lib.entry` above. Since we know this file will be mounted at `/systems/lancer/index.html`, we create a simple file that redirects to `/` — i.e., the url that will be proxied to `/` for foundry, the login screen.

`index.html`:
```


This is, then, the full `vite.config.ts` for our Lancer system.

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
      replacement: ("./runtimeConfig.browser"),
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