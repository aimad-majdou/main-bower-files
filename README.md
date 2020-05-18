main-yarn-files
================
![status](https://travis-ci.org/aimad-majdou/main-yarn-files.svg?branch=master)
[![npm version](https://badge.fury.io/js/main-yarn-files.svg)](https://badge.fury.io/js/main-yarn-files)

- [Installation](#installation)
- [Usage](#usage)
    - [Usage with gulp](#usage-with-gulp)
- [Options](#options)
    - [Overrides Options](#overrides-options)
        - [ignore](#ignore)
        - [dependencies](#dependencies)
    - [Common Options](#common-options)
        - [debugging](#debugging)
        - [main](#main)
        - [env](#env)
        - [paths](#paths)
        - [scope](#scope)
        - [checkExistence](#checkexistence)
        - [filter](#filter)

>This plugin only works with gulp (feel free to add grunt support).
>This plugin is a replacement for the `main-bower-files` plugin when migrating from bower to yarn, with less options, and it will only list modules declared in `dependencies` and not the ones delcared in `devDependencies`.

## Installation

__with npm:__ npm install main-yarn-files

__with yarn:__ yarn add main-yarn-files

## Usage

```javascript
var mainYarnFiles = require('main-yarn-files');
var files = mainYarnFiles([[filter, ]options][, callback]);
```

If first argument is type of `String`, `Array` or `RegExp` it will be used as a filter, otherwise it will be used as options.

This will read your `package.json`, iterate through your dependencies and returns an array of files defined in the main property of the packages `package.json` if not found it will check for `bower.json`, `.bower.json` and `component.json`.

You can override the behavior if you add an `overrides` property to your own `package.json`.

### Usage with gulp

```javascript
var gulp = require('gulp');
var mainYarnFiles = require('main-yarn-files');

gulp.task('TASKNAME', function() {
    return gulp.src(mainYarnFiles())
        .pipe(/* what you want to do with the files */)
});
```

#### You've got a flat folder/file structure after `.pipe(gulp.dest('my/dest/path'))`?

`mainYarnFiles` returns an array of files where each file is a absolute path without any globs (** or *). gulp requires globs in these paths to apply the base path. Because of this, you always have to tell gulp your dependencies base path (the path to the node_modules/@bower directory) explicitly.

Here is an example:

```javascript
var gulp = require('gulp');
var mainYarnFiles = require('main-yarn-files');

gulp.task('TASKNAME', function() {
    return gulp.src(mainYarnFiles(/* options */), { base: 'path/to/node_modules/@bower' })
        .pipe(/* what you want to do with the files */)
});

Or:

gulp.task('TASKNAME', function() {
    return gulp.src(mainYarnFiles({
			paths: {
				modulesFolder: nrhSrc + '/bower_components'
			}
		}))
        .pipe(/* what you want to do with the files */)
});
```

Now you should get something like `my/dest/path/jquery/jquery.js` if you have jquery installed.

## Options

### Overrides Options

#### ignore

Type: `Boolean` Default: `false`

Set to `true` if you want to ignore this package.

#### dependencies

Type: `Object`

You can override the dependencies of a package. Set to `null` to ignore the dependencies.

### Common Options

These options can be passed to this plugin, e.g: `mainYarnFiles(/* options*/)`

#### debugging

Type: `boolean` Default: `false`

Set to `true` to enable debugging output.

#### main

Type: `String` or `Array` or `Object` Default: `null`

You can specify for all packages a default main property which will be used if the package does not provide a main property.

#### env

Type: `String` Default: `process.env.NODE_ENV`

If `process.env.NODE_ENV` is not set you can use this option.

#### paths

Type: `Object` or `String`

You can specify the paths where the following yarn specific files are located:

`node_modules/@bower` and `package.json`

For example:

```javascript
mainYarnFiles({
    paths: {
        modulesFolder: 'path/for/node_modules/@bower',
        jsonFile: 'path/for/package.json'
    }
})
.pipe(gulp.dest('client/src/lib'));
```

If a `String` is supplied instead, it will become the basepath for default paths.

For example:

```javascript
mainYarnFiles({ paths: 'path/for/project' });
/*
    {
        modulesFolder: 'path/for/node_modules/@bower',
        jsonFile: 'path/for/package.json'
    }
*/
```
#### scope

Type: `String` Default: `@bower`

The scope used for dependencies declared in the package.json dependencies section (More infos about this : https://github.com/bower/bower/issues/2467#issue-239235304).

For example:

```
yarnFiles({
	paths: {
		modulesFolder: nrhSrcLibs
	},
	scope: nrhSrcLibsScope
})
```

#### checkExistence

Type: `boolean` Default: `false`

Set this to true if you want that the plugin checks every file for existence.

If enabled and a file does not exists, the plugin will throw an exception.

### filter

Type: `RegExp` or `function` or `glob` Default: `null`

You can filter the list of files by a regular expression, glob or callback function (the first and only argument is the file path).


## LICENSE

(MIT License)

Copyright (c) 2018 Aimad MAJDOU <aimad.majdou@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
