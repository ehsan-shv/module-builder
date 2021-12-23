# 📦 Nuxt Module Builder

> Complete solution to build Nuxt Modules.

**🧪 This tool is still under heavy development for Nuxt3. Feedback is more than welcome!**

## Features

- Unified build with [unjs/unbuild](https://github.com/unjs/unbuild)
- Compatible with Nuxt 3 and Nuxt Kit
- Automated build config using last module spec
- Typescript and ESM support
- Auto generated CommonJS stubs
- Auto generated types and shims for `@nuxt/schema`


## Project structure

Using module builder, requires a special project setup. You can check a full example [here](./example).

### `src/module.ts`

This is the entrypoint for module definition.

A default export using `defineNuxtModule` and `ModuleOptions` type export is expected.

```js [src/module.ts]
import { defineNuxtModule } from '@nuxt/kit'

export interface ModuleOptions {
  apiKey: string
}

export default defineNuxtModule<ModuleOptions>({
  meta: {
    name: 'my-module',
    configKey: 'myModule'
  },
  defaults: {
    apiKey: 'test'
  },
  async setup (moduleOptions, nuxt) {
    // Write module logic in setup function
  }
})
```

### `src/runtime/`

Any runtime file and code that we need to provide by module including plugins, composables and server api, should be in this directory.

Each file will be transformed individually using [unjs/mkdist](https://github.com/unjs/mkdist) to `dist/runtime/`.

<!-- TODO: Docs about how to address runtime from within setup -->

### `package.json`:

A minimum `package.json` should look like this:

```json [package.json]
{
  "name": "my-module",
  "license": "MIT",
  "version": "1.0.0",
  "exports": {
    ".": {
      "import": "./dist/module.mjs",
      "require": "./dist/module.cjs"
    }
  },
  "main": "./dist/module.cjs",
  "types": "./dist/types.d.ts",
  "files": [
    "dist"
  ],
  "scripts": {
    "prepack": "nuxt-module-build"
  },
  "dependencies": {
    "@nuxt/kit": "npm:@nuxt/kit-edge@latest"
  },
  "devDependencies": {
    "@nuxt/module-builder": "latest"
  }
}
```


## 💻 Development

- Clone repository
- Enable [Corepack](https://github.com/nodejs/corepack) using `corepack enable` (use `npm i -g corepack` for Node.js < 16.10)
- Install dependencies using `yarn install`
- Try building [example module](./example) using `yarn example:build`

## License

[MIT](./LICENSE) - Made with 💚
