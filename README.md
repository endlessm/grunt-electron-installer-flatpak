![Electron Installer for Flatpak](resources/logo.png)

# grunt-electron-installer-flatpak [![Version](https://img.shields.io/npm/v/grunt-electron-installer-flatpak.svg)](https://www.npmjs.com/package/grunt-electron-installer-flatpak) [![Build Status](https://img.shields.io/travis/mattdangerw/grunt-electron-installer-flatpak.svg)](http://travis-ci.org/mattdangerw/grunt-electron-installer-flatpak) [![Dependency Status](https://img.shields.io/gemnasium/mattdangerw/grunt-electron-installer-flatpak.svg)](https://gemnasium.com/mattdangerw/grunt-electron-installer-flatpak)

> Create a Flatpak package for your Electron app.

Not a fan of [Grunt](http://gruntjs.com/)? Use the vanilla module [`electron-installer-flatpak`](https://github.com/mattdangerw/electron-installer-flatpak)!


## Requirements

This tool requires `flatpak` >= 0.6.13 to be installed on your system.


## Getting Started

This plugin requires Grunt `~0.4.0`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-electron-installer-flatpak --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-electron-installer-flatpak')
```

*This plugin was designed to work with Grunt 0.4.x. If you're still using grunt v0.3.x it's strongly recommended that [you upgrade](http://gruntjs.com/upgrading-from-0.3-to-0.4), but in case you can't please use [v0.3.2](https://github.com/gruntjs/grunt-contrib-copy/tree/grunt-0.3-stable).*


## Installer task

_Run this task with the `grunt electron-installer-flatpak` command._

Task targets, files and options may be specified according to the grunt [Configuring tasks](http://gruntjs.com/configuring-tasks) guide.

### Usage

Say your Electron app lives in `path/to/app`, and has a structure like this:

```
.
├── LICENSE
├── README.md
├── node_modules
│   ├── electron-packager
│   └── electron-prebuilt
├── package.json
├── resources
│   ├── Icon.png
│   ├── IconTemplate.png
│   └── IconTemplate@2x.png
└── src
    ├── index.js
    ├── main
    │   └── index.js
    └── renderer
        ├── index.html
        └── index.js
```

You now run `electron-packager` to build the app for Flatpak:

```
$ electron-packager . app --platform linux --arch x64 --out dist/
```

And you end up with something like this in your `dist` folder:

```
.
└── dist
    └── app-linux-x64
        ├── LICENSE
        ├── LICENSES.chromium.html
        ├── content_shell.pak
        ├── app
        ├── icudtl.dat
        ├── libgcrypt.so.11
        ├── libnode.so
        ├── locales
        ├── natives_blob.bin
        ├── resources
        ├── snapshot_blob.bin
        └── version
```

In order to create a flatpak for your app, the configuration for your Grunt task would look like this:

```js
'electron-installer-flatpak': {
  app: {
    options: {
      arch: 'amd64'
    },
    src: 'path/to/app/dist/app-linux-x64',
    dest: 'path/to/app/dist/installers/'
  }
}
```

The task will try to extract all necessary information from your `package.json`, and then generate your package at `path/to/app/dist/installers/`.

You can also create different packages for different architectures, while manually overriding certain options:

```js
'electron-installer-flatpak': {
  options: {
    productName: 'Foo',
    productDescription: 'Bar baz qux.',
    section: 'devel',
    priority: 'optional',
    categories: [
      'Utility'
    ],
    lintianOverrides: [
      'changelog-file-missing-in-native-package',
      'executable-not-elf-or-script',
      'extra-license-file'
    ]
  },

  linux32: {
    options: {
      arch: 'i386'
    },
    src: 'path/to/app/dist/app-linux-ia32',
    dest: 'path/to/app/dist/installers/'
  },

  linux64: {
    options: {
      arch: 'amd64'
    },
    src: 'path/to/app/dist/app-linux-x64',
    dest: 'path/to/app/dist/installers/'
  }
}
```

### Options

See the options supported by [`electron-installer-flatpak`](https://github.com/mattdangerw/electron-installer-flatpak#options).


## Meta

* Code: `git clone git://github.com/mattdangerw/grunt-electron-installer-flatpak.git`
* Home: <https://github.com/mattdangerw/grunt-electron-installer-flatpak/>


## Contributors

* Daniel Perez Alvarez ([unindented@gmail.com](mailto:unindented@gmail.com))
* Matt Watson ([mattdangerw@gmail.com](mailto:mattdangerw@gmail.com))


## License

Copyright (c) 2016 Daniel Perez Alvarez ([unindented.org](https://unindented.org/)). This is free software, and may be redistributed under the terms specified in the LICENSE file.
