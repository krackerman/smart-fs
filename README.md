# smart-fs

[![Build Status](https://circleci.com/gh/blackflux/smart-fs.png?style=shield)](https://circleci.com/gh/blackflux/smart-fs)
[![Test Coverage](https://img.shields.io/coveralls/blackflux/smart-fs/master.svg)](https://coveralls.io/github/blackflux/smart-fs?branch=master)
[![Dependabot Status](https://api.dependabot.com/badges/status?host=github&repo=blackflux/smart-fs)](https://dependabot.com)
[![Dependencies](https://david-dm.org/blackflux/smart-fs/status.svg)](https://david-dm.org/blackflux/smart-fs)
[![NPM](https://img.shields.io/npm/v/smart-fs.svg)](https://www.npmjs.com/package/smart-fs)
[![Downloads](https://img.shields.io/npm/dt/smart-fs.svg)](https://www.npmjs.com/package/smart-fs)
[![Semantic-Release](https://github.com/blackflux/js-gardener/blob/master/assets/icons/semver.svg)](https://github.com/semantic-release/semantic-release)
[![Gardener](https://github.com/blackflux/js-gardener/blob/master/assets/badge.svg)](https://github.com/blackflux/js-gardener)

Abstraction Layer for File Management.

## Getting Started

    $ npm install --save smart-fs

## Functions

To ignore file extension and force treat the file as a certain type, you can pass the option `treatAs` as e.g. `json`.

### smartRead(filepath, options = { treatAs = null })

Read and parse file based on file extension.

The following extensions are handled in order:

- `.json`: Loads file using [JSON.parse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse).
- `.yml` and `.yaml`: Loads file using [yaml-boost](https://github.com/blackflux/yaml-boost).
- `.js`: Loads file using [require](https://nodejs.org/api/modules.html#modules_require_id).
- `.*`: Treats file as text file and loads as array of lines.

Note that the [required cache](https://nodejs.org/api/modules.html#modules_require_cache) is not automatically invalidated when loading cached `.js` files.

### smartWrite(filepath. content, options = { treatAs = null, mergeStrategy = (existing, changeset) => changeset })

Serialize and write content to file based on file extension.

The file and all necessary folders are created if not present.

The file is only actually written if the content has changed. 
Returns true if the file was written, false otherwise.

The following extension are handled in order:

- `.json`: Serialize uses [JSON.stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).
- `.yml` and `.yaml`: Serialize uses [yaml-boost](https://github.com/blackflux/yaml-boost).
- `.*`: Expects content as array and serializes by joining array using new line character.

The `mergeStrategy` option can be used to customize how the new content is merged if the target file already exists.
By default the file is simply overwritten.

## Important

Do not use this library for loading if you don't trust the source of the files you are loading!
