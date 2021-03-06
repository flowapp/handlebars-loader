# handlebars-loader

A [`handlebars`](http://handlebarsjs.com) template loader for [`webpack`](https://github.com/webpack/webpack).

## General Usage

### webpack configuration

```javascript
{
  ...
  module: {
    loaders: [
      ...
      { test: /\.handlebars$/, loader: "handlebars-loader" }
    ]
  }
}
```

### Your JS making use of the templates

```javascript
var template = require("./file.handlebars");
// => returns file.handlebars content as a template function
```

## Details

The loader resolves partials and helpers automatically. They are looked up relative to the current directory (this can be modified with the `rootRelative` option) or as a module if you prefix with `$`.

```handlebars
A file "/folder/file.handlebars".
{{> partial}} will reference "/folder/partial.handlebars".
{{> ../partial}} will reference "/partial.handlebars".
{{> $module/partial}} will reference "/folder/node_modules/module/partial.handlebars".
{{helper}} will reference the helper "/folder/helper.js" if this file exists.
{{[nested/helper] 'helper parameter'}} will reference the helper "/folder/nested/helper.js" if this file exists, passes 'helper parameter' as first parameter to helper.
{{../helper}} {{$module/helper}} are resolved similarly to partials.
```

The following query options are supported:
 - *helperDirs*: Defines additional directories to be searched for helpers. Allows helpers to be defined in a directory and used globally without relative paths. You must surround helpers in subdirectories with brackets (Handlerbar helper identifiers can't have forward slashes without this). See [example](https://github.com/altano/handlebars-loader/tree/master/examples/helperDirs)
 - *runtime*: Specify the path to the handlebars runtime library. Defaults to look under the local handlebars npm module, i.e. `handlebars/runtime`.
 - *extensions*: Searches for templates with alternate extensions. Defaults are .handlebars, .hbs, and '' (no extension).
 - *inlineRequires*: Defines a regex that identifies strings within helper/partial parameters that should be replaced by inline require statements.
 - *rootRelative*: When automatically resolving partials and helpers, use an implied root path if none is present. Default = `./`. Setting this to be empty effectively turns off automatically resolving relative handlebars resources for items like `{{helper}}`. `{{./helper}}` will still resolve as expected.
 - *debug*: Shows trace information to help debug issues (e.g. resolution of helpers).

See [`webpack`](https://github.com/webpack/webpack) documentation for more information regarding loaders.

## Full examples

See `examples` folder in this repo. The examples are fully runnable and demonstrate a number of concepts (using partials and helpers) -- just run `webpack` in that directory to produce dist/bundle.js in the same folder, open index.html.

## License

MIT (http://www.opensource.org/licenses/mit-license)
