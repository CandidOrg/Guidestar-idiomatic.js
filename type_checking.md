##Type Checking (Courtesy jQuery Core Style Guidelines)

  A. Actual Types

```javascript
  
  //String:
      typeof variable === "string"

  //Number:
      typeof variable === "number"

  //Boolean:
      typeof variable === "boolean"

  //Object:
      typeof variable === "object"

  //Array:
      Array.isArray( arrayLikeObject )
      (wherever possible)

  //Node:
      elem.nodeType === 1

  //null:
      variable === null

  //null or undefined:
      variable == null

  //undefined:
  
    //Global Variables:
      typeof variable === "undefined"

    //Local Variables:
      variable === undefined

    //Properties:
      object.prop === undefined
      object.hasOwnProperty( prop )
      "prop" in object
```

  B. Coerced Types

  Consider the implications of the following...

  Given this HTML:

  ```html

  <input type="text" id="foo-input" value="1">

  ```

  ```javascript

  // 3.B.1.1

  // `foo` has been declared with the value `0` and its type is `number`
  var foo = 0;

  // typeof foo;
  // "number"
  ...

  // Somewhere later in your code, you need to update `foo`
  // with a new value derived from an input element

  foo = document.getElementById("foo-input").value;

  // If you were to test `typeof foo` now, the result would be `string`
  // This means that if you had logic that tested `foo` like:

  if ( foo === 1 ) {

    importantTask();

  }
  // `importantTask()` would never be evaluated, even though `foo` has a value of "1"

  // 3.B.1.2

  // You can preempt issues by using smart coercion with unary + or - operators:

  foo = +document.getElementById("foo-input").value;
  //    ^ unary + operator will convert its right side operand to a number

  // typeof foo;
  // "number"

  if ( foo === 1 ) {

    importantTask();

  }

  // `importantTask()` will be called
  ```

  Here are some common cases along with coercions:

  ```javascript

  // 3.B.2.1

  var number = 1,
    string = "1",
    bool = false;

  number;
  // 1

  number + "";
  // "1"

  string;
  // "1"

  +string;
  // 1

  +string++;
  // 1

  string;
  // 2

  bool;
  // false

  +bool;
  // 0

  bool + "";
  // "false"
  ```

  ```javascript
  // 3.B.2.2

  var number = 1,
    string = "1",
    bool = true;

  string === number;
  // false

  string === number + "";
  // true

  +string === number;
  // true

  bool === number;
  // false

  +bool === number;
  // true

  bool === string;
  // false

  bool === !!string;
  // true
  ```

  ```javascript
  // 3.B.2.3

  var array = [ "a", "b", "c" ];

  !!~array.indexOf("a");
  // true

  !!~array.indexOf("b");
  // true

  !!~array.indexOf("c");
  // true

  !!~array.indexOf("d");
  // false

  // Note that the above should be considered "unnecessarily clever"
  // Prefer the obvious approach of comparing the returned value of
  // indexOf, like:

  if ( array.indexOf( "a" ) >= 0 ) {
    // ...
  }
  ```

  ```javascript
  // 3.B.2.4

  var num = 2.5;

  parseInt( num, 10 );

  // is the same as...

  ~~num;

  num >> 0;

  num >>> 0;

  // All result in 2

  // Keep in mind however, that negative numbers will be treated differently...

  var neg = -2.5;

  parseInt( neg, 10 );

  // is the same as...

  ~~neg;

  neg >> 0;

  // All result in -2
  // However...

  neg >>> 0;

  // Will result in 4294967294
  ```
