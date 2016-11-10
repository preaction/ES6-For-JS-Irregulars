
# ES6 For JavaScript Irregulars

## Sources

* https://github.com/metagrover/ES6-for-humans
* https://github.com/lukehoban/es6features
* http://babeljs.io
    * https://babeljs.io/docs/learn-es2015/
* http://es6-features.org/
* https://webapplog.com/es6/
* http://youmightnotneedjquery.com/

## Terms for Novice JS Programmers

* Object - Perl hash, Python dictionary
    * But also an Object

## Variables and Data Structures

### New variable declaration

#### let

`let` is scoped to the block, unlike `var` which is scoped to the
function. Block scoping narrows the availability of variables and makes
them easier to manage.

`let` solves a common problem involving `var` and closures that often
causes undesired behavior:

    var ary = [ 0, 1, 2, 3 ];
    for ( var i = 0; i < ary.length; i++ ) {

            setTimeout(
                function () {
                    console.log( i );
                },
                1000 * i
            );

    }

If I run this, every alert will be 3, because by the time the alerts are
run, that is the value of `i`.

The previous way to fix this was to wrap the inner part of the loop in
another function (often called an "Immediately-Invoked Function
Expression" or IIFE):

    var ary = [ 0, 1, 2, 3 ];
    for ( var i = 0; i < ary.length; i++ ) {
        (function ( i ) {
            setTimeout(
                function () {
                    console.log( i );
                },
                1000 * i
            );
        })( i ); // pass in the loop var to avoid closure problem
    }

If instead I use `let`, I will get the behavior I expect without jumping
through syntactic hoops.

    var ary = [ 0, 1, 2, 3 ];
    for ( let i = 0; i < ary.length; i++ ) {

            setTimeout(
                function () {
                    console.log( i );
                },
                1000 * i
            );

    }

Now each alert is the current value of the loop iterator.

#### const

True constants are something JavaScript has never had, but now it does.
`const` follows the same block scoping rules as `let`.

Like read-only variables in Perl, this doesn't prevent you from changing
things inside a constant array or constant object (for that, we can use
`Object.freeze()`).

#### Template literals

JavaScript now has string interpolation. So, instead of having to
concatenate variables with strings using `+`:

    var name = "Doug";
    console.log( "Hello, " + name );

We can now use `\`\`` and `${ ... }` to interpolate the variable into
the string:

    var name = "Doug";
    var hello = `Hello, ${name}`;

The thing inside the `${...}` can be any expression, like a math
expression:

    console.log( `2π = ${ Math.PI * 2 }` );

(Also, JavaScript now supports Unicode better)

#### Object shorthand

If we're building an object from scratch using `{ ... }`, now we have
a few shortcuts that make it less typing.

##### Named functions

If we're adding a function to an object, we don't need to say its name
twice, and we don't even need to explicitly use the `function` keyword.

Where previously we had to write this:

    var calc = {
        add: function add( x, y ) { return x+y }
    };

Now we can write this:

    var calc = {
                      add( x, y ) { return x+y }
    };

##### Member variables

If we're adding some local variables as new members to our object, we
also don't need to say their names twice.

    var   PI = 3.1415925;
    var   E = 2.71623;
    var numbers = {
        PI: PI,
        E:  E
    };

Now becomes:

    const PI = 3.1415925;
    const E = 2.71623;
    let numbers = {
            PI,
            E
    };

These two features combine to make objects less typing:

    var Math = {
        add: function add( x, y ) { return x + y },
        PI: PI,
        E:  E
    };

    let Math = {
                      add( x, y ) { return x+y }
            PI,
            E
    };

#### Destructuring

Destructuring allows you to reduce the number of steps to get at the
important things inside arrays and objects.

So, if you have a function that returns an array, and you only want the
first two things:

    let ary = [ 0, 1, 2, 3, 4 ];
    let [ first, second ] = ary;

    console.log( first );
    console.log( second );

But even more fun is destructuring objects, which allows us to quickly
grab all the interesting properties out of an object.

    const FOO = Math.PI, BAR = Math.E;
    const { "PI": FOO, "E": BAR } = Math;

This saves the constant `Math.PI` to the local `const FOO`, and `Math.E`
to the local `const BAR`.

#### Iterables

JavaScript now has an Iterable interface, and a new loop construct:
`for..of`. This, unlike `for..in`, works like most other languages'
loops when using an array.

`for..in` loops over property names. This mostly works fine for arrays:
the only "properties" one sets on an array are numeric. But if someone
adds a named property to the array, it will show up in every single
`for..in` loop.

    let ary = [ 1, 2, 3, 4 ];
    ary.foo = "bar"; // Arrays are objects and can have properties
    for ( let i in ary ) {
        console.log( i );
    }

    1
    2
    3
    4
    foo // ???

The `for..of` construct is explicitly, for arrays, only looping over the
indexes:

    let ary = [ 1, 2, 3, 4 ];
    ary.foo = "bar"; // Arrays are objects and can have properties
    for ( let i of ary ) {
        console.log( i );
    }

    1
    2
    3
    4

## Functions and Arguments

### Generators

Along with the new `for..of` iterator loop, we have generator functions.
These functions produce a new item for an iterator every time they are
called.

XXX

### Default Param Values

Now we can more easily have default parameters in our functions:

XXX Old way?

    function pow( x, y=1 ) {
        return Math.pow( x, y );
    }

### Spread Operator (...)

When you want to apply an array as the arguments to a function, you no
longer have to call `Function.apply`. Just use the `...` spread
operator:

    function add( x, y ) {
        return x + y;
    }
    let nums = [ 1, 2 ];

    console.log( add.apply(    nums ) );


    function add( x, y ) {
        return x + y;
    }
    let nums = [ 1, 2 ];

    console.log( add      ( ...nums ) );

### Rest Operator

If we want our function to have a variable number of arguments, we can
add the `...` to its parameter list, which will put the remaining
arguments into an array we name.

Previously, we had to use the `arguments` pseudo-array (and I won't get
in to the full implications of that "pseudo" there):

    function sum( ) {
        var sum = 0;
        for ( var i = 0; i < arguments.length; i++ ) {
            sum += arguments[i];
        }
        return sum;
    }

This makes it

### Arrow Functions

#### Arrow Function Binding

promises

## Objects

super
proxy objects
Subclass built-ins like Array, Date, and Element
classes
modules

## DOM

querySelector / querySelectorAll


## Not covered

typed arrays (ArrayBuffer)


new core objects
* Map/WeakMap
* Set/WeakSet

extended core objects
* new Array methods
* new Math methods
* new String methods


How to use ES6 now
* BabelJS
* NPM
* es2015
* babel-polyfill


---

What the fuck is Symbol?

