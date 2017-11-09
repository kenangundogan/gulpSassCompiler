# Gulp Sass/Scss Compiler
Gulp Sass/Scss Compiler (autoprefix, sourcemaps, minify, file-rename, browser-sync)


## Install Nodejs
[Node.js Home Page](https://nodejs.org/)



## Install Gulp (Terminal)
```
$npm install -g gulp
```

### Gulp Version Check (Terminal)
```
$npm gulp -v
```


#### Install Gulp Plugin (Terminal)
```
1. $npm install gulp-sass
2. $npm install gulp-autoprefixer
3. $npm install gulp-sourcemaps
4. $npm install gulp-clean-css
5. $npm install gulp-rename
6. $npm install browser-sync
```


## Basic Usage
<p> <b>Source File</b> </p>
<pre>
<code>
├── scss
    ├── style.scss
       ├── _reset.scss
       └── _defination.scss
</code>
</pre>

<br>

<p> <b>Outuput File</b> </p>
<pre>
<code>
├── css
    ├── style.css
    ├── style.min.css
</code>
</pre>

## gulpfile.js
<b>Variable</b>
```javascript
  var gulp             = require('gulp'),
      sass             = require('gulp-sass'),
      autoprefixer     = require('gulp-autoprefixer'),
      sourcemaps       = require('gulp-sourcemaps'),
      minifyCss        = require('gulp-clean-css'),
      rename           = require('gulp-rename'),
      browserSync      = require('browser-sync').create();
```

<b>Sass (Function)</b>
```javascript
gulp.task('sass', function(){
    // sass directory
    return gulp.src('./sass/*scss')
            .pipe(sass())
            //outputstyle (nested, compact, expanded, compressed)
            .pipe(sass({outputStyle:'compact'}).on('error', sass.logError))
            // sourcemaps
            .pipe(sourcemaps.init())
            // sourcemaps output directory
            .pipe(sourcemaps.write(('./maps')))
            // css output directory
            .pipe(gulp.dest('./css')),
            // watch file
            gulp.watch('./sass/*.scss', ['sass']);
});
```

<b>Minify (Function)</b>
```javascript
// minify css (merge + autoprefix + rename)
gulp.task('minify-css', function(){
   return gulp.src('./css/style.css')
            .pipe(minifyCss())
             // autoprefixer
            .pipe(autoprefixer({
                browsers: ['last 15 versions'],
                cascade: false
            }))
            // minify css rename
            .pipe(rename('style.min.css'))
            // minify css output directory
            .pipe(gulp.dest('./css'))
            // browser sync
            .pipe(browserSync.reload({stream: true})),
            // watch file
            gulp.watch('./css/style.css', ['minify-css']);
});
```

<b>Browser Tracking (Function)</b>
```javascript
// sass/css browser tracking
gulp.task('browser-sync', function(){
    browserSync.init({
        server:{
            baseDir: './'
        }
    });
    // watch html
    gulp.watch('./*.html').on('change', browserSync.reload);
});
```

<b>Default Compile</b>
```javascript
// gulp default (sass, minify-css, browser-sync) method
gulp.task('default', ['sass', 'minify-css', 'browser-sync']);

```
