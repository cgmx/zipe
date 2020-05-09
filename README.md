# ⚡ ZIPE ⚡

> Vite but prerender

⚠️ _EXPERIMENTAL_ - Currently only supporting render entry file, no special SSR handling, that will be in the works for the next versions

[vite](https://github.com/vuejs/vite) is "fast"⚡ but how fast can it render on the server?

> **Spoiler alert**: really fast!

`zipe` will compile the components on the fly and cache them, if the component change it will refresh the cache, making it super quick to render.

The HMR is the same as `vite` so no more page reloads, only the changed component updates.

## Working

- Proof of concept
- SSR with HMR

## NOT working

- No `build` support for now
- No `router`
- No source maps
- css modules
- css scope seems not to be working when doing SSR
- importing js/ts files in SFC, only SFC files are supported

## TODO

- `typescript` support
- `vue-router` support (some limitations, for example adding new routes on the fly and such)
- `source-maps` currently source maps don't exist, may need help here.
- `lifecycle hooks` with composition-api
- some composables to get request info, etc.
- Building `SPA` and `static`
- SSR serve
- Add option to disable `HMR` and some dev cache features to be able to run this in a server
- Tests

## Installing

```bash
# install with yarn
yarn add zipe

# install with npm
npm install zipe
```

## Usage

Create a plugin and provide the entry file for the SSR

```js
const { ssrBuild } = require("../dist");
const { createServer } = require("vite");

const myPlugin = ({
  root, // project root directory, absolute path
  app, // Koa app instance
  resolver, // resolve file
  server, // raw http server instance
  watcher, // chokidar file watcher instance
}) => {
  app.use(async (ctx, next) => {
    if (ctx.path === "/app") {
      const filePath = resolver.requestToFile("/App.vue"); // get the full path
      const html = ssrBuild(filePath, resolver, root, watcher); // build HTML
      ctx.body = html; //assign the html output
      return;
    }
  });
};

createServer({
  plugins: [myPlugin],
}).listen(3200);
```

## Development

```bash

git clone https://github.com/pikax/zipe

cd zip

yarn

yarn dev & yarn playground

# edit /App.vue
# or go to src/_playground.ts and change

```
