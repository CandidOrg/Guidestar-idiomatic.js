##Misc

This section will serve to illustrate ideas and concepts that should not be considered dogma, but instead exists to encourage questioning practices in an attempt to find better ways to do common JavaScript programming tasks.

A. Using `switch` should be avoided, modern method tracing will blacklist functions with switch statements

There seems to be drastic improvements to the execution of `switch` statements in latest releases of Firefox and Chrome.
http://jsperf.com/switch-vs-object-literal-vs-module

Notable improvements can be witnessed here as well:
https://github.com/rwldrn/idiomatic.js/issues/13

```javascript

// 7.A.1.1
// An example switch statement

switch( foo ) {
  case "alpha":
    alpha();
    break;
  case "beta":
    beta();
    break;
  default:
    // something to default to
    break;
}

// 7.A.1.2
// A alternate approach that supports composability and reusability is to
// use an object to store "cases" and a function to delegate:

var cases, delegator;

// Example returns for illustration only.
cases = {
  alpha: function() {
    // statements
    // a return
    return [ "Alpha", arguments.length ];
  },
  beta: function() {
    // statements
    // a return
    return [ "Beta", arguments.length ];
  },
  _default: function() {
    // statements
    // a return
    return [ "Default", arguments.length ];
  }
};

delegator = function() {
  var args, key, delegate;

  // Transform arguments list into an array
  args = [].slice.call( arguments );

  // shift the case key from the arguments
  key = args.shift();

  // Assign the default case handler
  delegate = cases._default;

  // Derive the method to delegate operation to
  if ( cases.hasOwnProperty( key ) ) {
    delegate = cases[ key ];
  }

  // The scope arg could be set to something specific,
  // in this case, |null| will suffice
  return delegate.apply( null, args );
};

// 7.A.1.3
// Put the API in 7.A.1.2 to work:

delegator( "alpha", 1, 2, 3, 4, 5 );
// [ "Alpha", 5 ]

// Of course, the `case` key argument could easily be based
// on some other arbitrary condition.

var caseKey, someUserInput;

// Possibly some kind of form input?
someUserInput = 9;

if ( someUserInput > 10 ) {
  caseKey = "alpha";
} else {
  caseKey = "beta";
}

// or...

caseKey = someUserInput > 10 ? "alpha" : "beta";

// And then...

delegator( caseKey, someUserInput );
// [ "Beta", 1 ]

// And of course...

delegator();
// [ "Default", 0 ]

```


B. Early returns promote code readability with negligible performance difference

```javascript

// 7.B.1.1
// Bad:
function returnLate( foo ) {
  var ret;

  if ( foo ) {
    ret = "foo";
  } else {
    ret = "quux";
  }
  return ret;
}

// Good:

function returnEarly( foo ) {

  if ( foo ) {
    return "foo";
  }
  return "quux";
}
```

