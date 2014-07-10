# gulp-manifest 
> Generate HTML5 Cache Manifest files. Submitted by [Scott Hillman](https://github.com/hillmanov/).

## Editing

Many thanks to Scott Hillman who developed [grunt-manifest](https://github.com/hillmanov/gulp-manifest) for gulpjs. I made few changes in the plugin to support the relative path of the file which are to be cahched.

Big thanks to [Gunther Brunner](https://github.com/gunta/) for writing the [grunt-manifest](https://github.com/gunta/grunt-manifest) plugin. This plugin was heavily influenced by his great work. 

Visit the [HTML 5 Guide to AppCache](http://www.html5rocks.com/en/tutorials/appcache/beginner/) for more information on Cache Manifest files.

## Usage

```shell
npm install gulp-appcache
```

## API

### Parameters

### manifest(options)

This controls how this task (and its helpers) operate and should contain key:value pairs, see options below.

#### options.relativePath
Type: `String`

Adds the relative path to the file to be caches with hash working as previous.

#### options.filename
Type: `String`  
Default: `"app.manifest"`

Set name of the Cache Manifest file.

#### options.cache

It is not supported in this plugin as the hash does not change with in manifest file.

#### options.exclude
Type: `String` `Array`  
Default: `undefined`  

Exclude specific files from the Cache Manifest file.

#### options.network
Type: `String` `Array`  
Default: `"*"` (By default, an online whitelist wildcard flag is added)   

Adds a string to the **NETWORK** section.

See [here](http://diveintohtml5.info/offline.html#network) for more information.

#### options.fallback
Type: `String` `Array`  
Default: `undefined`  

Adds a string to the **FALLBACK** section.

See [here](http://diveintohtml5.info/offline.html#fallback) for more information.

#### options.preferOnline
Type: `Boolean`   
Default: `undefined`

Adds a string to the **SETTINGS** section, specifically the cache mode flag of the ```prefer-online``` state.

See [here](http://www.whatwg.org/specs/web-apps/current-work/multipage/offline.html#concept-appcache-mode-prefer-online) for more information.

#### options.timestamp
Type: `Boolean`   
Default: `true` 

Adds a timestamp as a comment for easy versioning.

Note: timestamp will invalidate application cache whenever cache manifest is rebuilt, even if contents of files in `src` have not changed.

#### options.hash
Type: `Boolean`
Default: `false`

Adds a sha256 hash of all `src` files (actual contents) as a comment.

This will ensure that application cache invalidates whenever actual file contents change (it's recommented to set `timestamp` to `false` when `hash` is used).

### Usage Example


    gulp.task('manifest', function(){
      gulp.src(['../client/resources/build/**/*'])
        .pipe(manifest({
	  relativePath: '/resources/build'
          hash: true,
          preferOnline: true,
          network: ['http://*', 'https://*', '*'],
          filename: 'app.manifest',
          exclude: 'app.manifest'
         }))
        .pipe(gulp.dest('build'));
    });


### Output example

    CACHE MANIFEST

    CACHE:
    /resources/build/js/app.js
    /resources/build/css/style
    /resources/build/css/style.css
    /resources/build/js/zepto.min.js
    /resources/build/js/script.js
    /resources/build/some_files/index.html
    /resources/build/some_files/about.html

    NETWORK:
    http://*
    https://*
    *

    # hash: 76f0ef591f999871e1dbdf6d5064d1276d80846feeef6b556f74ad87b44ca16a


You do need to be fully aware of standard browser caching.
If the files in **CACHE** are in the network cache, they won't actually update,
since the network cache will spit back the same file to the application cache.
Therefore, it's recommended to add a hash to the filenames's, akin to rails or yeoman. See [here](http://www.stevesouders.com/blog/2008/08/23/revving-filenames-dont-use-querystring/) why query strings are not recommended.
