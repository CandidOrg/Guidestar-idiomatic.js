##Naming

A. You are not a human code compiler/compressor, so don't try to be one.

The following code is an example of egregious naming:

```javascript

// 6.A.1.1
// Example of code with poor names

function q(s) {
  return document.querySelectorAll(s);
}
var i,a=[],els=q("#foo");
for(i=0;i<els.length;i++){a.push(els[i]);}
```

Without a doubt, you've written code like this - hopefully that ends today.

Here's the same piece of logic, but with kinder, more thoughtful naming (and a readable structure):

```javascript

// 6.A.2.1
// Example of code with improved names

function query( selector ) {
  return document.querySelectorAll( selector );
}

var idx,
  elements = [],
  matches = query("#foo"),
  length = matches.length;

for ( idx = 0; idx < length; idx++ ) {
  elements.push( matches[ idx ] );
}

```

A few additional naming pointers:

```javascript

// 6.A.3.1
// Naming strings

`dog` is a string

// 6.A.3.2
// Naming arrays

`dogs` is an array of `dog` strings

// 6.A.3.3
// Naming functions, objects, instances, etc

camelCase; function and var declarations


// 6.A.3.4
// Naming constructors, prototypes, etc.

PascalCase; constructor function


// 6.A.3.5
// Naming regular expressions

rDesc = //;


// 6.A.3.6
// From the Google Closure Library Style Guide

functionNamesLikeThis;
variableNamesLikeThis;
ConstructorNamesLikeThis;
EnumNamesLikeThis;
methodNamesLikeThis;
SYMBOLIC_CONSTANTS_LIKE_THIS;

```

B. Faces of `this`

Beyond the generally well known use cases of `call` and `apply`, always prefer `.bind( this )` or a functional equivalent, for creating `BoundFunction` definitions for later invocation. Only resort to aliasing when no preferable option is available.

```javascript

// 6.B.1
function Device( opts ) {

  this.value = null;

  // open an async stream,
  // this will be called continuously
  stream.read( opts.path, function( data ) {

    // Update this instance's current value
    // with the most recent value from the
    // data stream
    this.value = data;

  }.bind(this) );

  // Throttle the frequency of events emitted from
  // this Device instance
  setInterval(function() {

    // Emit a throttled event
    this.emit("event");

  }.bind(this), opts.freq || 100 );
}

// Just pretend we've inherited EventEmitter ;)

```

When unavailable, functional equivalents to `.bind` exist in many modern JavaScript libraries.


```javascript
// 6.B.2

// eg. lodash/underscore, _.bind()
function Device( opts ) {

  this.value = null;

  stream.read( opts.path, _.bind(function( data ) {

    this.value = data;

  }, this) );

  setInterval(_.bind(function() {

    this.emit("event");

  }, this), opts.freq || 100 );
}

// eg. jQuery.proxy
function Device( opts ) {

  this.value = null;

  stream.read( opts.path, jQuery.proxy(function( data ) {

    this.value = data;

  }, this) );

  setInterval( jQuery.proxy(function() {

    this.emit("event");

  }, this), opts.freq || 100 );
}

// eg. dojo.hitch
function Device( opts ) {

  this.value = null;

  stream.read( opts.path, dojo.hitch( this, function( data ) {

    this.value = data;

  }) );

  setInterval( dojo.hitch( this, function() {

    this.emit("event");

  }), opts.freq || 100 );
}

```

As a last resort, create an alias to `this` using `self` as an Identifier. This is extremely bug prone and should be avoided whenever possible.

```javascript

// 6.B.3

function Device( opts ) {
  var self = this;

  this.value = null;

  stream.read( opts.path, function( data ) {

    self.value = data;

  });

  setInterval(function() {

    self.emit("event");

  }, opts.freq || 100 );
}

```

C. Use `thisArg`

Several prototype methods of ES 5.1 built-ins come with a special `thisArg` signature, which should be used whenever possible

```javascript

// 6.C.1

var obj;

obj = { f: "foo", b: "bar", q: "qux" };

Object.keys( obj ).forEach(function( key ) {

  // |this| now refers to `obj`

  console.log( this[ key ] );

}, obj ); // <-- the last arg is `thisArg`

// Prints...

// "foo"
// "bar"
// "qux"

```

`thisArg` can be used with `Array.prototype.every`, `Array.prototype.forEach`, `Array.prototype.some`, `Array.prototype.map`, `Array.prototype.filter`

