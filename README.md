gulp-ng-constant
================

## Information

<table>
<tr> 
<td>Package</td><td>gulp-ng-constant</td>
</tr>
<tr>
<td>Description</td>
<td>Plugin for dynamic generation of angular constant modules.<br>
Based of <a href="https://github.com/werk85/grunt-ng-constant">grunt-ng-constant</a></td>
</tr>
<tr>
<td>Node Version</td>
<td>>= 0.10</td>
</tr>
</table>

## Usage
  
_**gulpfile.js**_

    var ngConstant = require('gulp-ng-constant');

    gulp.task('config', function () {
      gulp.src('app/config.json')
        .pipe(ngConstant({
          space: '\t',
          deps: [],
          wrap: 'amd',
          templatePath: TEMPLATE_PATH
        }))
        // Writes config.js to dist/ folder
        .pipe(gulp.dest('dist'));
    });

_**app/config.json**_

    {
      "name": "my.module.config",
      "deps": ["ngAnimate"],
      "constants": {
        "myFirstCnt": true,
        "mySecondCnt": { "hello": "world" }
      }
    }

_**dist/config.js**_

    define(["require", "exports"], function(require, exports) {
      return angular.module("my.module.config", ["ngAnimate"])
        .constant("myFirstCnt", true)
        .constant("mySecondCnt", { "hello": "world" });
    });


#### options.space

Type: `string`  
Default: `'\t'`  
_optional_

A string that defines how the JSON.stringify method will prettify your code.

#### options.deps

Type: `array<string>`  
Default: `[]`  
_optional_

An array that specifies the default dependencies a module should have.
Overridable from configuration file.

#### options.wrap

Type: `boolean|string`  
Default: `false`  
Available: `['amd', 'commonjs']`
_optional_

A boolean to active or deactive the automatic wrapping.
A string who will wrap the result of file, use the
`<%= __ngModule %>` variable to indicate where to put the generated
module content.
A string with 'amd' that wraps the module as an AMD module, 
compatible with RequireJS

#### options.template

Type: `string`  
Default: _content of [tpls/constant.tpl.ejs](https://github.com/guzart/gulp-ng-constant/blob/master/tpls/constant.tpl.ejs)_  
_optional_

EJS template to apply when creating the output configuration file. The following variables
are passed to the template during render:

  * `moduleName`: the module name (`string`)
  * `deps`: the module dependencies (`array<string>`)
  * `constants`: the module constants (`array<contantObj>`)
    * where a `constantObj` is an object with a `name` and a `value`, both `strings`.

#### options.templatePath

Type: `string`  
Default: `'tpls/constant.tpl.ejs'`  
_optional_

Location of a custom template file for creating the output configuration file. Defaults to the provided constants template file if none provided.
