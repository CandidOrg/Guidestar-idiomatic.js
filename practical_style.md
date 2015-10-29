##Practical Style

```javascript

// 5.1.1
// A Practical Module

(function( global ) {
  var Module = (function() {

    var data = "secret";

    return {
      // This is some boolean property
      bool: true,
      // Some string value
      string: "a string",
      // An array property
      array: [ 1, 2, 3, 4 ],
      // An object property
      object: {
        lang: "en-Us"
      },
      getData: function() {
        // get the current value of `data`
        return data;
      },
      setData: function( value ) {
        // set the value of `data` and return it
        return ( data = value );
      }
    };
  })();

  // Other things might happen here

  // expose our module to the global object
  global.Module = Module;

})( this );

// 5.2.1
// A Practical Constructor

(function( global ) {

  function Ctor( foo ) {

    this.foo = foo;

    return this;
  }

  Ctor.prototype.getFoo = function() {
    return this.foo;
  };

  Ctor.prototype.setFoo = function( val ) {
    return ( this.foo = val );
  };

  // To call constructor's without `new`, you might do this:
  var ctor = function( foo ) {
    return new Ctor( foo );
  };

  // expose our constructor to the global object
  global.ctor = ctor;

})( this );

```

