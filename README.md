# Build Zurb Foundation 4

A simple Rake build script to get your Sass / Compass Zurb Foundation 4 projects ready for production. Tested as of Zurb Foundation 4.3.2, included.

Created by [James Stone](http://manofstone.com)

source and repo: [github.com/manofstone/build-foundation](http://github.com/manofstone/build-foundation)

## MIT License

Go Crazy, Stay Fired Up!

## Requirements:

### UglifierJS

```
npm install uglify-js
```

### CleanCss

```
npm install clean-css
```


## Usage:

```
rake
rake build
```

## Sample Output:

```
$ rake build
creating build/ directory
mkdir build
mkdir build/javascripts
mkdir build/javascripts/vendor
mkdir build/stylesheets
mkdir build/images
minifying seperate js files
cp javascripts/vendor/custom.modernizr.js build/javascripts/vendor/custom.modernizr.js
uglifyjs --verbose build/javascripts/vendor/custom.modernizr.js -mo build/javascripts/vendor/custom.modernizr.js.min
cp javascripts/vendor/jquery.js build/javascripts/vendor/jquery.js
uglifyjs --verbose build/javascripts/vendor/jquery.js -mo build/javascripts/vendor/jquery.js.min
cp javascripts/vendor/zepto.js build/javascripts/vendor/zepto.js
uglifyjs --verbose build/javascripts/vendor/zepto.js -mo build/javascripts/vendor/zepto.js.min
minifying combined js
uglifyjs --verbose build/javascripts/app.js -mo build/javascripts/app.js.min
compressing css
compass compile -e production --force
identical stylesheets/app.css
cp stylesheets/app.css build/stylesheets/app.css
cleancss -e -o build/stylesheets/app.css.min build/stylesheets/app.css
copying media files
cp: images: No such file or directory
copying over html
cp *.html build
modify html to use minified and concated resources
index.html
  removing ref to foundation/foundation.js
  removing ref to foundation/foundation.abide.js
  removing ref to foundation/foundation.alerts.js
  removing ref to foundation/foundation.clearing.js
  removing ref to foundation/foundation.cookie.js
  removing ref to foundation/foundation.dropdown.js
  removing ref to foundation/foundation.forms.js
  removing ref to foundation/foundation.interchange.js
  removing ref to foundation/foundation.joyride.js
  removing ref to foundation/foundation.magellan.js
  removing ref to foundation/foundation.orbit.js
  removing ref to foundation/foundation.placeholder.js
  removing ref to foundation/foundation.reveal.js
  removing ref to foundation/foundation.section.js
  removing ref to foundation/foundation.tooltips.js
  removing ref to foundation/foundation.topbar.js
```