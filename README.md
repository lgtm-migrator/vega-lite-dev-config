# vega-lite-dev-config

[![Build Status](https://github.com/vega/vega-lite-dev-config/workflows/Test/badge.svg)](https://github.com/vega/vega-lite-dev-config/actions)
[![npm version](https://img.shields.io/npm/v/vega-lite-dev-config.svg)](https://www.npmjs.com/package/vega-lite-dev-config)

Version-controlled build config for easy re-use and sharing using Beemo.

## Install

```sh
yarn add --dev vega-lite-dev-config
```

Before using the build config provided you should remove any older dependencies or config files for the drivers you intend to use (e.g., remove `eslint` and `.eslintrc`). `vega-lite-dev-config` will handle these dependencies, and it will _auto-generate the config files for you_.

## Using drivers

This project is built with [beemo](https://github.com/milesj/beemo), and therefore requires a `".config/beemo.ts"` configuration file with a list of drivers you want to enable. You can optionally configure some drivers as shown below:

```ts
// .config/beemo.ts
import { Config } from "vega-lite-dev-config";

const config: Config = {
  module: 'vega-lite-dev-config',
  drivers: {
    babel: true,
    prettier: true,
    eslint: true,
    jest: true,
    typescript: {
      buildFolder: "build"
    }
  },
  // optional settings for the vega-lite-dev-config
  settings: {
    node: true,
    react: false
  }
};
```

If you want to customize the drivers, you need to create overrides as described in [the Beemo docs](https://milesj.gitbook.io/beemo/consumer).

## Executing drivers

Executing a driver will initialize Beemo's pipeline, generate configuration files (e.g., it will generate a `.eslintrc` or `prettier.config.js`, and execute the underlying driver binary and logging to the console.

> All arguments passed to Beemo are passed to driver.

You may define these commands as scripts in your package.json:

```js
// package.json
{
  "scripts": {
    "prepare": "beemo create-config --silent",
    "test": "beemo jest",
    "test:inspect": "node --inspect-brk ./node_modules/.bin/jest --runInBand",
    "prettierbase": "beemo prettier '*.{css,scss,html}'",
    "eslintbase": "beemo eslint .",
    "format": "yarn eslintbase --fix && yarn prettierbase --write",
    "lint": "yarn eslintbase && yarn prettierbase --check",
    "types": "beemo typescript",
    "types:watch": "beemo typescript --watch"
  }
}
```
