# @videojs/babel-config

[![Build Status](https://travis-ci.org/videojs/babel-config.svg?branch=master)](https://travis-ci.org/videojs/babel-config)
[![Greenkeeper badge](https://badges.greenkeeper.io/videojs/babel-config.svg)](https://greenkeeper.io/)
[![Slack Status](http://slack.videojs.com/badge.svg)](http://slack.videojs.com)

Currently babel configs are the same for most plugins, and most of them are built using this babel config via babel. We have some libraries though that are should not be built using rollup for nodejs consumption, that are currently doing so. So this config while still consumed by [videojs-generate-rollup-config](https://github.com/videojs/videojs-generate-rollup-config) can also be used standalone with babel-cli.

Lead Maintainer: Brandon Casey [@brandonocasey](https://github.com/brandonocasey)

Maintenance Status: Stable


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Installation](#installation)
- [Usage](#usage)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Installation

```
$ npm install --save-dev @babel/cli @videojs/babel-config
```


## Usage

1. In your rollup config add a line to delete module builds:
```js
if (config.builds.module) {
  delete config.builds.module;
}
```
2. Add a npm scripts for cjs/es dists to your package.json:

```json
{
  "build:cjs": "babel-config-cjs -d ./cjs ./src",
  "build:es": "babel-config-es -d ./es ./src",
  "watch:cjs": "npm run build:cjs -- -w",
  "watch:es": "npm run build:es -- -w"
}
```

3. verify that `main` in `package.json` is set to the cjs dist. Something like `cjs/index.js`
4. verify that `module` in `package.json` is set to the es dist. Something like `es/index.js`
5. verify that `browser` in `package.json` is set to the browser dist. Something like `dist/project-name.js`
6. Add `es` and `cjs` to `vjsstandard.ignore` in `package.json`
7. Make sure that `es/` and `cjs/` are in `files` in `package.json`
8. Add `/es` and `/cjs` to `.gitignore`.
9. Add `./es` and `./cjs` to the npm script for `clean` after `mkdir -p` and `rm -rf`.

```sh
shx rm -rf ./dist ./test/dist ./cjs ./es && shx mkdir -p ./dist ./test/dist ./cjs ./es
```


## Important things
* When running through `babel-config-cjs`, `babel-config-es`, or `babel-config-run` if the `TEST_BUNDLE_ONLY` environment variable is set **nothing** will run! This is to maintain parity with [`videojs-generate-rollup-config`](https://github.com/videojs/videojs-generate-rollup-config).
* The `babel-config-cjs` binary runs babel cli with `--verbose` and `--config-file` set to the cjs config exported here.
* The `babel-config-es` binary runs babel cli with `--verbose` and `--config-file` set to the es config exported here.
* The `babel-config-run` binary runs babel cli with `--verbose` but no config file.

## Changing configuration
1. require the configuration you want to use `const config = require('@videojs/babel-config/cjs.js');`
2. Make changes to it and export: `module.export = config`.
3. Change the build scripts to  `babel-config-run --config-file <new-config> ...`
