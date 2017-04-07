# api documentation for  [babelify (v7.3.0)](https://github.com/babel/babelify)  [![npm package](https://img.shields.io/npm/v/npmdoc-babelify.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-babelify) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-babelify.svg)](https://travis-ci.org/npmdoc/node-npmdoc-babelify)
#### Babel browserify transform

[![NPM](https://nodei.co/npm/babelify.png?downloads=true)](https://www.npmjs.com/package/babelify)

[![apidoc](https://npmdoc.github.io/node-npmdoc-babelify/build/screenCapture.buildNpmdoc.browser.%2Fhome%2Ftravis%2Fbuild%2Fnpmdoc%2Fnode-npmdoc-babelify%2Ftmp%2Fbuild%2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-babelify/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-babelify/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-babelify/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Sebastian McKenzie",
        "email": "sebmck@gmail.com"
    },
    "bugs": {
        "url": "https://github.com/babel/babelify/issues"
    },
    "dependencies": {
        "babel-core": "^6.0.14",
        "object-assign": "^4.0.0"
    },
    "description": "Babel browserify transform",
    "devDependencies": {
        "babel-plugin-transform-es3-property-literals": "^6.0.14",
        "babel-plugin-transform-node-env-inline": "^6.0.14",
        "babel-plugin-transform-react-display-name": "^6.0.14",
        "babel-plugin-undeclared-variables-check": "^6.0.14",
        "babel-preset-es2015": "^6.0.14",
        "babel-preset-react": "^6.0.14",
        "browserify": "^13.0.0",
        "convert-source-map": "^1.1.0",
        "lodash.zipobject": "^4.1.3",
        "path-is-absolute": "^1.0.0",
        "tap": "^5.7.1"
    },
    "directories": {},
    "dist": {
        "shasum": "aa56aede7067fd7bd549666ee16dc285087e88e5",
        "tarball": "https://registry.npmjs.org/babelify/-/babelify-7.3.0.tgz"
    },
    "gitHead": "27282793d1a4819e7e44a33540a84792f1f79dd9",
    "homepage": "https://github.com/babel/babelify",
    "license": "MIT",
    "maintainers": [
        {
            "name": "sebmck",
            "email": "sebmck@gmail.com"
        },
        {
            "name": "zertosh",
            "email": "zertosh@gmail.com"
        }
    ],
    "name": "babelify",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/babel/babelify.git"
    },
    "scripts": {
        "test": "tap test/*.js"
    },
    "version": "7.3.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module babelify](#apidoc.module.babelify)
1.  [function <span class="apidocSignatureSpan">babelify.</span>configure (opts)](#apidoc.element.babelify.configure)
1.  [function <span class="apidocSignatureSpan">babelify.</span>super_ (options)](#apidoc.element.babelify.super_)



# <a name="apidoc.module.babelify"></a>[module babelify](#apidoc.module.babelify)

#### <a name="apidoc.element.babelify.configure"></a>[function <span class="apidocSignatureSpan">babelify.</span>configure (opts)](#apidoc.element.babelify.configure)
- description and source-code
```javascript
configure = function (opts) {
  opts = assign({}, opts);
  var extensions = opts.extensions ? babel.util.arrayify(opts.extensions) : null;
  var sourceMapsAbsolute = opts.sourceMapsAbsolute;
  if (opts.sourceMaps !== false) opts.sourceMaps = "inline";

  // babelify specific options
  delete opts.sourceMapsAbsolute;
  delete opts.extensions;
  delete opts.filename;

  // babelify backwards-compat
  delete opts.sourceMapRelative;

  // browserify specific options
  delete opts._flags;
  delete opts.basedir;
  delete opts.global;

  // browserify cli options
  delete opts._;
  // "--opt [ a b ]" and "--opt a --opt b" are allowed:
  if (opts.ignore && opts.ignore._) opts.ignore = opts.ignore._;
  if (opts.only && opts.only._) opts.only = opts.only._;
  if (opts.plugins && opts.plugins._) opts.plugins = opts.plugins._;
  if (opts.presets && opts.presets._) opts.presets = opts.presets._;

  return function (filename) {
    if (!babel.util.canCompile(filename, extensions)) {
      return stream.PassThrough();
    }

    var _opts = sourceMapsAbsolute
      ? assign({sourceFileName: filename}, opts)
      : opts;

    return new Babelify(filename, _opts);
  };
}
```
- example usage
```shell
...
var babelify = require("babelify");
browserify().transform(babelify, {presets: ["es2015", "react"]});
'''

Or, with the 'configure' method:

'''js
browserify().transform(babelify.configure({
  presets: ["es2015", "react"]
}));
'''

#### Customizing extensions

By default, all files with the extensions '.js', '.es', '.es6' and '.jsx' are compiled. You can change this by passing an array
of extensions.
...
```

#### <a name="apidoc.element.babelify.super_"></a>[function <span class="apidocSignatureSpan">babelify.</span>super_ (options)](#apidoc.element.babelify.super_)
- description and source-code
```javascript
function Transform(options) {
  if (!(this instanceof Transform))
    return new Transform(options);

  Duplex.call(this, options);

  this._transformState = new TransformState(this);

  var stream = this;

  // start out asking for a readable event once data is transformed.
  this._readableState.needReadable = true;

  // we have implemented the _read method, and done the other things
  // that Readable wants before the first _read call, so unset the
  // sync guard flag.
  this._readableState.sync = false;

  if (options) {
    if (typeof options.transform === 'function')
      this._transform = options.transform;

    if (typeof options.flush === 'function')
      this._flush = options.flush;
  }

  // When the writable side finishes, then flush out anything remaining.
  this.once('prefinish', function() {
    if (typeof this._flush === 'function')
      this._flush(function(er) {
        done(stream, er);
      });
    else
      done(stream);
  });
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
