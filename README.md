# memfs

In-memory file-system with Node's `fs` API.

[See example in browser.](https://jsfiddle.net/6a96vLoj/2/)

A [`fs`](https://nodejs.org/api/fs.html) API to work with *virtual in-memory* files.

```javascript
var memfs = require('memfs');

var mem = new memfs.Volume;
mem.mountSync('./', {
    "test.js": "console.log(123);",
    "dir/hello.js": "console.log('hello world');"
});

console.log(mem.readFileSync('./dir/hello.js').toString());
```

Use it together with [`unionfs`](http://www.npmjs.com/package/unionfs):

```javascript
var unionfs = require('unionfs');
var fs = require('fs');

// Create a union of two file systems:
unionfs
    .use(fs)
    .use(mem);
    
// Now `unionfs` has the `fs` API but on both file systems.
console.log(unionfs.readFileSync('./test.js').toString()); // console.log(123);
    
// Replace `fs` with the union of those file systems.
unionfs.replace(fs);

// Now you can do this.
console.log(fs.readFileSync('./test.js').toString()); // console.log(123);

// ... and this:
require('./test.js'); // 123

```

This package assumes you are running on Node or have a
[`path`](https://www.npmjs.com/package/path) and `buffer` modules available.

It also uses `process` and `setImmediate` globals, but mocks them, if not
available.

## Contributing

TODOs:


Testing:

    npm test
    npm run test-watch

Building:

    npm run build
