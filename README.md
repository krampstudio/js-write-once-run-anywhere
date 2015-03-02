# Java[Script] : Write once run anywhere

- The Java lang arguments

## The promise of node.js

 - But some early attempts like Jaxer, Mozilla Rhino, etc.
 - The same code on browsers, servers, microcontrollers, etc.

## How to write a library that can be integrated in any environments ?

### Which version of the languages

 - ES3, ES5 or ES6/7 ?
 - ES5 because of https://github.com/es-shims/es5-shim
 - Missing block : module system

### Old school

_NO MODULES_

 - export to global context

Declaration :
```js
var aja = {
 awesome : function(){
    console.log('awesome');
 }
};
window.aja = window.aja || aja;
```

Usage :
```js
window.aja.awesome();
```

### Common JS

 - runs on node.js, io.js, browserify

Declaration :
```js
var aja = {
 awesome : function(){
    console.log('awesome');
 }
};
module.exports = aja;
```

Usage :
```js
var aja = require('aja');
aja.awesome();
```

### AMD

  - require.js, almond, etc.

Declaration :
```js
define([], function(){
  var aja = {
   awesome : function(){
      console.log('awesome');
   }
  };
  return aja;
});
```

Usage :
```js
require(['aja'], function(aja){
  aja.awesome();
});
```

### ES6 imports

  - standard

Declaration :
```js
var aja = {
 awesome : function(){
    console.log('awesome');
 }
};
export default aja;
```

Usage :
```js
import aja from 'aja';
aja.awesome();
```

## All together

```js
(function(){
'use strict';
  var aja = {
   awesome : function(){
      console.log('awesome');
   }
  };

  //AMD
  if (typeof define === 'function' && define.amd) {
    define([], function(){  //don't name it
      return aja;
    });

  //CommonJs
  } else if (typeof exports === 'object') {
    module.exports = aja;

  //Old school
  } else {
  window.aja = window.aja || aja;
  }

 });
```

  Or even better at https://github.com/umdjs/umd

> But where are ES6 modules?

Imposible to feature detect (at least for now), see http://stackoverflow.com/questions/27922232/how-to-feature-detect-es6-modules

## Going further

Building a generator that bundle the code for each system:
 - a generic one (using UMD)
 - AMD
 - CJS
 - Old School
 - ES6

> I am not going to maintain 5 versions of my source code
So, automate it dude

Creation of a Grunt task : [grunt-exportify](https://github.com/krampstudio/grunt-exportify)
