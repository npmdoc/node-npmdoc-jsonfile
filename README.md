# api documentation for  [jsonfile (v2.4.0)](https://github.com/jprichardson/node-jsonfile#readme)  [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-jsonfile.svg)](https://travis-ci.org/npmdoc/node-npmdoc-jsonfile)
#### Easily read/write JSON files.

[![NPM](https://nodei.co/npm/jsonfile.png?downloads=true)](https://www.npmjs.com/package/jsonfile)

[![apidoc](https://npmdoc.github.io/node-npmdoc-jsonfile/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-jsonfile_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-jsonfile/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-jsonfile/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "author": {
        "name": "JP Richardson",
        "email": "jprichardson@gmail.com"
    },
    "bugs": {
        "url": "https://github.com/jprichardson/node-jsonfile/issues"
    },
    "dependencies": {
        "graceful-fs": "^4.1.6"
    },
    "description": "Easily read/write JSON files.",
    "devDependencies": {
        "mocha": "2.x",
        "mock-fs": "^3.8.0",
        "rimraf": "^2.4.0",
        "standard": "^6.0.8"
    },
    "directories": {},
    "dist": {
        "shasum": "3736a2b428b87bbda0cc83b53fa3d633a35c2ae8",
        "tarball": "https://registry.npmjs.org/jsonfile/-/jsonfile-2.4.0.tgz"
    },
    "gitHead": "00b3983ac4aade79c64c7a8c2ced257078625c6d",
    "homepage": "https://github.com/jprichardson/node-jsonfile#readme",
    "keywords": [
        "read",
        "write",
        "file",
        "json",
        "fs",
        "fs-extra"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "jprichardson",
            "email": "jprichardson@gmail.com"
        }
    ],
    "name": "jsonfile",
    "optionalDependencies": {
        "graceful-fs": "^4.1.6"
    },
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+ssh://git@github.com/jprichardson/node-jsonfile.git"
    },
    "scripts": {
        "lint": "standard",
        "test": "npm run lint && npm run unit",
        "unit": "mocha"
    },
    "version": "2.4.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module jsonfile](#apidoc.module.jsonfile)
1.  [function <span class="apidocSignatureSpan">jsonfile.</span>readFile (file, options, callback)](#apidoc.element.jsonfile.readFile)
1.  [function <span class="apidocSignatureSpan">jsonfile.</span>readFileSync (file, options)](#apidoc.element.jsonfile.readFileSync)
1.  [function <span class="apidocSignatureSpan">jsonfile.</span>writeFile (file, obj, options, callback)](#apidoc.element.jsonfile.writeFile)
1.  [function <span class="apidocSignatureSpan">jsonfile.</span>writeFileSync (file, obj, options)](#apidoc.element.jsonfile.writeFileSync)
1.  object <span class="apidocSignatureSpan">jsonfile.</span>spaces



# <a name="apidoc.module.jsonfile"></a>[module jsonfile](#apidoc.module.jsonfile)

#### <a name="apidoc.element.jsonfile.readFile"></a>[function <span class="apidocSignatureSpan">jsonfile.</span>readFile (file, options, callback)](#apidoc.element.jsonfile.readFile)
- description and source-code
```javascript
function readFile(file, options, callback) {
  if (callback == null) {
    callback = options
    options = {}
  }

  if (typeof options === 'string') {
    options = {encoding: options}
  }

  options = options || {}
  var fs = options.fs || _fs

  var shouldThrow = true
  // DO NOT USE 'passParsingErrors' THE NAME WILL CHANGE!!!, use 'throws' instead
  if ('passParsingErrors' in options) {
    shouldThrow = options.passParsingErrors
  } else if ('throws' in options) {
    shouldThrow = options.throws
  }

  fs.readFile(file, options, function (err, data) {
    if (err) return callback(err)

    data = stripBom(data)

    var obj
    try {
      obj = JSON.parse(data, options ? options.reviver : null)
    } catch (err2) {
      if (shouldThrow) {
        err2.message = file + ': ' + err2.message
        return callback(err2)
      } else {
        return callback(null, null)
      }
    }

    callback(null, obj)
  })
}
```
- example usage
```shell
...
[![windows Build status](https://img.shields.io/appveyor/ci/jprichardson/node-jsonfile/master.svg?label=windows%20build)](https://
ci.appveyor.com/project/jprichardson/node-jsonfile/branch/master)

<a href="https://github.com/feross/standard"><img src="https://cdn.rawgit.com/feross/standard/master/sticker.svg" alt="Standard
JavaScript" width="100"></a>

Why?
----

Writing 'JSON.stringify()' and then 'fs.writeFile()' and 'JSON.parse()' with 'fs.readFile()' enclosed in 'try/catch' blocks became
 annoying.



Installation
------------

npm install --save jsonfile
...
```

#### <a name="apidoc.element.jsonfile.readFileSync"></a>[function <span class="apidocSignatureSpan">jsonfile.</span>readFileSync (file, options)](#apidoc.element.jsonfile.readFileSync)
- description and source-code
```javascript
function readFileSync(file, options) {
  options = options || {}
  if (typeof options === 'string') {
    options = {encoding: options}
  }

  var fs = options.fs || _fs

  var shouldThrow = true
  // DO NOT USE 'passParsingErrors' THE NAME WILL CHANGE!!!, use 'throws' instead
  if ('passParsingErrors' in options) {
    shouldThrow = options.passParsingErrors
  } else if ('throws' in options) {
    shouldThrow = options.throws
  }

  var content = fs.readFileSync(file, options)
  content = stripBom(content)

  try {
    return JSON.parse(content, options.reviver)
  } catch (err) {
    if (shouldThrow) {
      err.message = file + ': ' + err.message
      throw err
    } else {
      return null
    }
  }
}
```
- example usage
```shell
...
- 'throws' ('boolean', default: 'true'). If 'JSON.parse' throws an error, throw the error.
If 'false', returns 'null' for the object.

'''js
var jsonfile = require('jsonfile')
var file = '/tmp/data.json'

console.dir(jsonfile.readFileSync(file))
'''


### writeFile(filename, obj, [options], callback)

'options': Pass in any 'fs.writeFile' options or set 'replacer' for a [JSON replacer](https://developer.mozilla.org/en-US/docs/Web
/JavaScript/Reference/Global_Objects/JSON/stringify). Can also pass in 'spaces'.
...
```

#### <a name="apidoc.element.jsonfile.writeFile"></a>[function <span class="apidocSignatureSpan">jsonfile.</span>writeFile (file, obj, options, callback)](#apidoc.element.jsonfile.writeFile)
- description and source-code
```javascript
function writeFile(file, obj, options, callback) {
  if (callback == null) {
    callback = options
    options = {}
  }
  options = options || {}
  var fs = options.fs || _fs

  var spaces = typeof options === 'object' && options !== null
    ? 'spaces' in options
    ? options.spaces : this.spaces
    : this.spaces

  var str = ''
  try {
    str = JSON.stringify(obj, options ? options.replacer : null, spaces) + '\n'
  } catch (err) {
    if (callback) return callback(err, null)
  }

  fs.writeFile(file, str, options, callback)
}
```
- example usage
```shell
...
[![windows Build status](https://img.shields.io/appveyor/ci/jprichardson/node-jsonfile/master.svg?label=windows%20build)](https://
ci.appveyor.com/project/jprichardson/node-jsonfile/branch/master)

<a href="https://github.com/feross/standard"><img src="https://cdn.rawgit.com/feross/standard/master/sticker.svg" alt="Standard
JavaScript" width="100"></a>

Why?
----

Writing 'JSON.stringify()' and then 'fs.writeFile()' and 'JSON.parse()' with 'fs.readFile()' enclosed in 'try/catch' blocks became
 annoying.



Installation
------------

npm install --save jsonfile
...
```

#### <a name="apidoc.element.jsonfile.writeFileSync"></a>[function <span class="apidocSignatureSpan">jsonfile.</span>writeFileSync (file, obj, options)](#apidoc.element.jsonfile.writeFileSync)
- description and source-code
```javascript
function writeFileSync(file, obj, options) {
  options = options || {}
  var fs = options.fs || _fs

  var spaces = typeof options === 'object' && options !== null
    ? 'spaces' in options
    ? options.spaces : this.spaces
    : this.spaces

  var str = JSON.stringify(obj, options.replacer, spaces) + '\n'
  // not sure if fs.writeFileSync returns anything, but just in case
  return fs.writeFileSync(file, str, options)
}
```
- example usage
```shell
...

'''js
var jsonfile = require('jsonfile')

var file = '/tmp/data.json'
var obj = {name: 'JP'}

jsonfile.writeFileSync(file, obj)
'''

**formatting with spaces:**

'''js
var jsonfile = require('jsonfile')
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
