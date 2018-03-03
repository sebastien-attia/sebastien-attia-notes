---
layout: post
cover: 'assets/images/cover1.jpg'
navigation: True
title: How to integrate JSF 2.3 with bootstrap-sass, FontAwesome, browser-sync and gulp ?
date: 2017-06-25 23:14:00
tags: programming JSF web
subclass: 'post tag-programming tag-JSF tag-web'
logo: 'assets/images/innovea-tech-white.svg'
author: sebastien
categories: sebastien
---

This post is based on the [excellent article](http://www.codevoila.com/post/32/customize-bootstrap-using-bootstrap-sass-and-gulp) from http://www.codevoila.com and the [following answer](https://stackoverflow.com/questions/31434885/how-to-use-font-awesome-4-x-css-file-with-jsf-browser-cant-find-font-files) in stackOverflow.

## The problem

The relative paths used in the URLs of the bootstrap's glyphicons are broken when bootstrap is used with JSF.
In standard, the format of the path is as following: 
`'../fonts/fontawesome-webfont.eot?v=4.3.0'`,  
but the format required by JSF is as following: 
`#{resource['font-awesome:fonts/fontawesome-webfont.eot']}&v=4.3.0`

The idea is to build bootstrap with sass and modify consequently the path.

## The solution

#### 1. Install npm, bower and gulp

Run:
- `npm init`  
- `npm install bower --saved-dev`  
- `npm install gulp --saved-dev`    
- `npm install gulp-sass --saved-dev`  
- `npm install gulp-cssnano --saved-dev`  


#### 2. Install bootstrap-sass, FontAwesome and browser-sync

- `npm install browser-sync --saved-dev`  
- `bower install bootstrap-sass --saved-dev`  
- `bower install components-font-awesome --saved-dev`  


#### 3. The gulp script

To have the full explanation of this part, report to the [following article](http://www.codevoila.com/post/32/customize-bootstrap-using-bootstrap-sass-and-gulp) in http://www.codevoila.com.

We suppose here that all the scss files for customization are in `src/main/view/`.

``` javascript
var gulp        = require('gulp');
var browserSync = require('browser-sync').create();
var sass        = require('gulp-sass');
var cssnano     = require('gulp-cssnano');


//source and distribution folder
var
    source = 'src/main/view/',
    dest = 'src/main/webapp/resources/dist/';
    
// Bootstrap scss source
var bootstrapSass = {
        in: './bower_components/bootstrap-sass/'
};

// Font-Awesome scss sources
var fontAwesomeSass = {
        in: './bower_components/components-font-awesome/'
};

var jqueryScripts = {
		in: './bower_components/jquery/'
};

// Bootstrap fonts source
var fonts = {
        in: [source + 'fonts/*.*', bootstrapSass.in + 'assets/fonts/**/*', fontAwesomeSass.in + 'fonts/**/*'],
        out: dest + 'fonts/'
};

var bootstrapJS = {
        in: [jqueryScripts.in+'dist/**/*', bootstrapSass.in + 'assets/javascripts/**/*'],
        out: dest + 'js/'
};

// Our scss source folder: .scss files
var scss = {
    in: source + 'scss/main.scss',
    out: dest + 'css/',
    watch: source + 'scss/**/*',
    sassOpts: {
        outputStyle: 'nested',
        precison: 3,
        errLogToConsole: true,
        includePaths: [fontAwesomeSass.in+'scss', bootstrapSass.in + 'assets/stylesheets']
    }
};

// Bootstrap javascripts
gulp.task('bootstrap_js', function () {
    return gulp
        .src(bootstrapJS.in)
        .pipe(gulp.dest(bootstrapJS.out));
});

//copy bootstrap required fonts to dest
gulp.task('fonts', function () {
    return gulp
        .src(fonts.in)
        .pipe(gulp.dest(fonts.out));
});


//compile scss
gulp.task('sass', ['fonts', 'bootstrap_js'], function () {
    return gulp.src(scss.in)
        .pipe(sass(scss.sassOpts))
        .pipe(gulp.dest(scss.out));
});

gulp.task('dist', ['fonts', 'bootstrap_js'], function () {
    return gulp.src(scss.in)
        .pipe(sass(scss.sassOpts))
        .pipe(cssnano())
        .pipe(gulp.dest(scss.out));
});

//default task
gulp.task('default', ['sass'], function () {
	browserSync.init({
		proxy: "localhost:8080"
	});
	
	gulp.watch(scss.watch, ['sass']);
	gulp.watch("src/main/webapp/**/*.xhtml").on('change', browserSync.reload);
});

```

#### 4. The sass' files

In the directory `src/main/view/` (source directory in the gulp file), the following custom sass' files:

- The file `_bootstrap-custom.scss`:  

``` scss
/*!
 * Bootstrap v3.3.7 (http://getbootstrap.com)
 * Copyright 2011-2016 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */

@function twbs-font-path($path) {
 	@return "\#{resource['dist:#{$path}']}";
}

@function twbs-image-path($path) {
  	@return $path;
}

$bootstrap-sass-asset-helper: true;


// Core variables and mixins
@import "bootstrap/variables";
@import "bootstrap/mixins";

// Reset and dependencies
@import "bootstrap/normalize";
@import "bootstrap/print";
@import "bootstrap/glyphicons";

// Core CSS
@import "bootstrap/scaffolding";
@import "bootstrap/type";
@import "bootstrap/code";
@import "bootstrap/grid";
@import "bootstrap/tables";
@import "bootstrap/forms";
@import "bootstrap/buttons";

// Components
@import "bootstrap/component-animations";
@import "bootstrap/dropdowns";
@import "bootstrap/button-groups";
@import "bootstrap/input-groups";
@import "bootstrap/navs";
@import "bootstrap/navbar";
@import "bootstrap/breadcrumbs";
@import "bootstrap/pagination";
@import "bootstrap/pager";
@import "bootstrap/labels";
@import "bootstrap/badges";
@import "bootstrap/jumbotron";
@import "bootstrap/thumbnails";
@import "bootstrap/alerts";
@import "bootstrap/progress-bars";
@import "bootstrap/media";
@import "bootstrap/list-group";
@import "bootstrap/panels";
@import "bootstrap/responsive-embed";
@import "bootstrap/wells";
@import "bootstrap/close";

// Components w/ JavaScript
@import "bootstrap/modals";
@import "bootstrap/tooltip";
@import "bootstrap/popovers";
@import "bootstrap/carousel";

// Utility classes
@import "bootstrap/utilities";
@import "bootstrap/responsive-utilities";
```

- The file `_bootstrap-override-variables.scss`:  

``` scss
<copy of bower_components/bootstrap-sass/assets/stylesheets/bootstrap/_variables.scss file>
```
bootstrap can be customized in this file.


- The file `_font-awesome-custom.scss`:  

``` scss
/*!
 *  Font Awesome 4.7.0 by @davegandy - http://fontawesome.io - @fontawesome
 *  License - http://fontawesome.io/license (Font: SIL OFL 1.1, CSS: MIT License)
 */

@function resource_url($url,$params) { 
	@return url + "(\"\#{resource['dist:#{$url}']}&#{$params}\")";
}


@import "variables";
@import "mixins";
@import "font-awesome-path-custom";
@import "core";
@import "larger";
@import "fixed-width";
@import "list";
@import "bordered-pulled";
@import "animated";
@import "rotated-flipped";
@import "stacked";
@import "icons";
@import "screen-reader";
```


- The file `_font-awesome-override-variables.scss`:  

``` scss
// Variables
// --------------------------

$fa-font-path:        "/fonts" !default;
<+ rest of file bower_components/components-font-awesome/scss/_variables.scss >
```

- and the file `main.scss`:

```
/* main.scss */
@import "bootstrap-override-variables";

@import "bootstrap-custom";
@import "bootstrap/theme";

/* import font-awesome */
@import "font-awesome-override-variables";

@import "font-awesome-custom";
```

