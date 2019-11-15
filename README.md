# gulp-sass-inheritance-independer

> Rebuild a sass/scss file with other files that have imported this file

* Based on [gulp-jade-inheritance](https://github.com/juanfran/gulp-jade-inheritance)
* Uses [sass-graph](https://github.com/xzyfer/sass-graph) for the heavy lifting
* Updated dependencies to work with Node 12 and Gulp 4

Useful when working on a larger project: Styles can be (re-)built incrementally on a per-need basis.


## Changelog

#### v1.0.0 (2019-15-11)
Forked and updated dependencies to work with Node 12 and Gulp 4

#### v1.1.0 (2017-05-13)

* **The file initially emitted is now being passed through as well**
* **Support for nested imports** - Thanks [@safareli](https://github.com/safareli)! Fixes [#3](https://github.com/berstend/gulp-sass-inheritance/issues/3) [#5](https://github.com/berstend/gulp-sass-inheritance/issues/5)
* Added [tests](./test) & solid coverage
* Updated dependencies


###### Previous: v0.5.1 (2015-04-09)



## Install

```bash
# Using npm
npm install gulp-sass-inheritance-independer --save

# Using yarn
yarn add gulp-sass-inheritance-independer
```




## Usage

You can use `gulp-sass-inheritance` with `gulp-changed` to only process the files that have changed but also recompile files that import the one that changed.

```js
'use strict';
var gulp = require('gulp');
var sassInheritance = require('gulp-sass-inheritance');
var sass = require('gulp-sass');
var cached = require('gulp-cached');
var gulpif = require('gulp-if');
var filter = require('gulp-filter');

gulp.task('sass', function() {
  return gulp.src('src/styles/**/*.scss')

    //filter out unchanged scss files, only works when watching
    .pipe(gulpif(global.isWatching, cached('sass')))

    //find files that depend on the files that have changed
    .pipe(sassInheritance({dir: 'src/styles/'}))

    //filter out internal imports (folders and files starting with "_" )
    .pipe(filter(function (file) {
      return !/\/_/.test(file.path) || !/^_/.test(file.relative);
    }))

    //process scss files
    .pipe(sass())

    //save all the files
    .pipe(gulp.dest('dist'));
});
gulp.task('setWatch', function() {
    global.isWatching = true;
});
gulp.task('watch', ['setWatch', 'sass'], function() {
    //your watch functions...
});
```


## Contributing :tada:
```bash
# Install dependencies
yarn

# Run tests
yarn test
```



## License

MIT
