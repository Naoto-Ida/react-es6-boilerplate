
# React ES6 Boilerplate

This boilerplate avoids making too many architectural decisions and instead focuses on providing tooling and build automation for development and production.

- An ES6 version of the classic redux todo example is provided together with a few snapshot tests. All of this can be completely removed if desired.

## Versions

This has been tested and works with Node 4.3.0 and NPM 2.10.1 however, you will see a huge (buildtime) performance improvement using Node 6+ and NPM 3+ due to Babel module discovery improvements.


## Features

 - Babel v6 (support for all ES6 syntax client and server)
 - React v15
 - Redux v3
 - Jest/Jasmine v14 (inc. snapshot support)
 - Express v4 (inc. hbs)
 - Webpack
 - PostCSS (with SASS support)


## Get started

Install required dependencies via npm 
```
npm install
```
Build application and start watching for changes
```
npm run build
```
Run tests (optional) - outputs reports to terminal and ./coverage 
```
npm test
```
Run application (new terminal window)
```
npm start 
```

Webpack will watch for changes and automatically recompile your javascript and css on the fly to your ./build folder


## Build and runtime pattern

- Babel CLI takes care of transpilation and webpack ensures fast recompile 
- Avoided using hot reloader modules
- Avoided using babel-polyfill - this attaches to global
- Avoided babel-node - this is slow 
- PostCSS in favour of SASS - I have implemented precss and cssnext, enabling sass syntax as well as future selectors
- Webpack dev/prod configs take care of babel and css transpilation 
- Processed files are moved to a <root>/build folder and the application runs from there
- When a standard build is run, webpack watch is used to track only files with changes and re-process them to the build folder
- Refreshing the browser will show latest React or CSS changes. This results in a very fast dev experience.


Dev time build - will build server and assets uncompressed to ./build and watch css/js for changes 
```
npm run build
```
Production build - will build server and assets compressed to ./build
```
npm run build:prod
```
Distribution/deployment - will run build:prod and also package the server and assets to ./dist as separate zip files 
```
npm run package
```


## Express

- A simple Express server is supplied with handlebars view engine 
- A virtual path is configured for assets in the ./build folder

The only 'custom' setting is the attachment of script and style tags to app.locals

```
app.locals.scripts = tags.scripts;
app.locals.styles = tags.styles;
```
This is done to allow handlebars views to seamlessly inject script or style tags. The 'viewtags' module is a simple function used by webpack to determine the filenames of js/css for the latest build.
In production build (build:prod) this also enables easy injection of hashed filenames.

The viewtags module currently creates tags for the {{{scripts}}} and {{{styles}}} hbs placeholders, more can be easily supported by editing the code in config/webpack/webpack.viewtags.js:

```
  var assetTagConfig = {
    scripts: scripts,  
    styles: styles,
    inlineStyles: '',
    deferredScripts: '',
    deferredStyles: ''
  }
```


## Testing

Jest was chosen as the test suite as it supports Jasmine syntax, full coverage reports, and is being actively developed by Facebook.

All Jest configuration settings can be found in package.json, it has been configured to:

- Find any file with a name containing 'test.js'
- Ignore config, node_module, and other commonly unwanted areas
- Mock css and images as objects to avoid require issues
- Automocking is disabled by default - https://facebook.github.io/jest/docs/automatic-mocking.html#content 
- By using the --coverage flag, a coverage report is shown in terminal and written to <root>./coverage


## Load Testing

3.1 GHz Intel Core i7
16 GB 1867 MHz DDR3

10 Test iterations

Average:

Concurrency Level:      20
Time taken for tests:   0.059 seconds
Requests per second:    4212.87 [#/sec] (mean)
Time per request:       4.747 [ms] (mean)
Time per request:       0.237 [ms] (mean, across all concurrent requests)


## TODO

- Add disabled/commented server-side render example
- Add performance profile tooling
- Cross-platform shell commands for npm scripts