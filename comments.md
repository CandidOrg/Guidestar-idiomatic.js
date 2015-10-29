Comments

* Comments SHOULD answer WHAT the line/s in question are doing
* Comments should be placed before the first statment in a block of code that explain that block of code and are allowed sparingly inside of the block of code.
* Comments can only be placed on their own line, or at the end of a line of code.
* Comments are NOT a replacement for self-documenting code.
* Even self-documenting code should have comments.
* Comments should be as short and informative as possible.
* If a block of code isn't self-documented or is "hacky" then it should have a comment
* JSDoc style is good, but requires a significant time investment  

In the case where the variable/function/object name makes the use obvious, no comment is neccesary.
```javascript

// Best Comment 

var maxThreads = 10;
```

A rule of thumb is to add comments if you can't immiediately determine what a section of code does just by looking at it. 
Another is to add comments if you ask yourself if this needs a comment. This may also be a [code smell](https://en.wikipedia.org/wiki/Code_smell)
If in doubt, leave a comment, it can always be removed later.

```javascript

// Good Comment //

// maximum number of concurrent threads allowed
var maxThreads = 10;

// Bad Comment //

// The maximum number of threads that are allowed to run concurrently in this program.  
// There can be no more than this number of threads running at once.
var maxThreads = 10;
```

