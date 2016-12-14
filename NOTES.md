
# ES6 For JavaScript Irregulars

## Sources

* <https://github.com/metagrover/ES6-for-humans>
* <https://github.com/lukehoban/es6features>
* <http://es6-features.org/>
* <https://webapplog.com/es6/>
* <http://exploringjs.com/es6/ch_modules.html>
* <http://youmightnotneedjquery.com/>
* <http://babeljs.io>
    * <https://babeljs.io/docs/learn-es2015/>

## Terms for Novice JS Programmers

* Object - Perl hash, Python dictionary
    * But also an Object

## A Short History of JavaScript

JavaScript was pitched as a way to interact with Java applets in the
Netscape browser. In those halcyon days, it was thought that Java
applets were going to be the next step in the evolution of the web.
Unfortunately, the limitations of technology at the time ensured Java
applets were a passing fad, but JavaScript stayed around.

Shortly after its release, IE also implemented JavaScript (called
JScript). This was at the height of the browser wars, and everything
sucked. Rather than continue this forever, JavaScript was given to
a standards body called Ecma.

And like all other languages, JavaScript is evolving. In 1999, ECMA
released the ECMAScript 3 standard, which shored up some of the
ambiguities in the language and gave the browser vendors a baseline to
work from. At this point, JavaScript got `try/catch` and regular
expressions, both common standard features in modern programming
languages.

After a disastrous attempt at creating ECMAScript 4, which was
completely incompatible, ECMAScript 5 was released in 2009. This version
gave us `'use strict'`, which allowed us to prevent some unexpected and
undesired behaviors, and JSON was added to the core language.

But the version we're going to cover here is ECMAScript 6, released in
2015. This was a massive upgrade to the language, with a lot of new
features that everyone should be using on a daily basis. And, with the
help of new JavaScript tools, you can use ES6 today in browsers that do
not yet support it natively!

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
These functions produce an iterator object. Every time the `next()`
method is called on this object, the function runs until another value
is generated by a `yield` statement.

    function* fib( limit ) {
        let x = 1, y = 1;
        for ( let count = 0; count < limit; count++ ) {
            let temp = x;
            x = y;
            y = temp + x;
            yield temp;
        }
    }

    let iter = fib(3);
    iter.next();
    console.log( iter.value );
    // { value: 1, done: false }
    iter.next();
    console.log( iter.value );
    // { value: 1, done: false }
    iter.next();
    console.log( iter.value );
    // { value: 2, done: true }

We could also make our generator infinite: Since the function is halted
every time `yield` is called, we won't be in an infinite loop.

    function* fib(       ) {
        let x = 1, y = 1;
        while ( 1 ) {
            let temp = x;
            x = y;
            y = temp + x;
            yield temp;
        }
    }

Since our loop is forever, we can keep pulling values off until we find
the one we want:

    > iter.next();
    { value: 1, done: false }
    > iter.next();
    { value: 1, done: false }
    > iter.next();
    { value: 2, done: false }
    > iter.next();
    { value: 3, done: false }
    > iter.next();
    { value: 5, done: false }
    > iter.next();
    { value: 8, done: false }
    > iter.next();
    { value: 13, done: false }
    > iter.next();
    { value: 21, done: false }

### Default Param Values

Now we can more easily have default parameters in our functions:

Previously we could achieve this by using `||` in the body of the
function:

    function pow( x, y ) {
        y = y || 1;
        return Math.pow( x, y );
    }

    pow( 2, 2 );
    // 4

    pow( 2 );
    // 2

Except this causes a problem when `y` is `0`. Anything raised to the 0th
power is 1, but...

    pow( 2, 0 );
    // 2 <- should be 1

So often we need to use the `?:` ternary instead:

    function pow( x, y ) {
        y = typeof y == 'undefined' ? 1 : y;
        return Math.pow( x, y );
    }

    pow( 2, 0 );
    // 1 <- correct

In ES6, we can simply define the default right in the arguments, so when
the argument is missing, it is assigned the correct value:

    function pow( x, y=1 ) {
        return Math.pow( x, y );
    }

    pow( 2, 2 );
    // 4

    pow( 2, 0 );
    // 1

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

This makes it more difficult when you've got some required arguments and
other optional arguments.

### Arrow Functions

This is my favorite thing in new JavaScript, and not just because
there's less typing. JavaScript is massively callback-oriented:
Everything async requires at least one callback. Often, in a series of
async operations, you have callbacks calling callbacks calling
callbacks. It gets to be a lot of small, anonymous functions.

Arrow functions make callbacks a lot easier:

    var add = function ( x, y ) { return x + y };

    let add =          ( x, y ) =>       x + y;

We don't need the word `function`, we don't need the curly braces,
because it's a single expression, and we don't need `return`!

#### Arrow Function Binding

But the best part is that arrow functions do not affect the context
of the code. `this` is still the same as it was when the callback was
created. This prevents us from having to save our scope variable to some
other variable like `self` or `that`:

    var self = this;
    this.addEventListener( 'click', function () {
        console.log( 'clicked once, removing...' );
        self.removeEventListener( 'click' );
    } );

    this.addEventListener( 'click',          () => {
        console.log( 'clicked once, removing...' );
        this.removeEventListener( 'click' );
    } );

This pattern is such a huge source of headaches for new JS users that
I'm infinitely glad that there is finally a better way.

### Promises

Promises are another way to make callbacks easier to deal with,
especially for async requests that have a single response:

    function get( url, cb ) {
        var request = new XMLHttpRequest();

        request.addEventListener( 'load', function () {
            cb.call        ( this.responseText );
        } );
        request.open( "GET", url );
        request.send();

    }

    // Fetch three things in order
    get( 'http://example.com', function ( response ) {
        get( 'http://example.net', function ( response ) {
            get( 'http://example.org', function ( response ) {
                console.log( 'Done!' );
            } );
        } );
    } );

    function get( url     ) {
        var request = new XMLHttpRequest();
        var promise = new Promise();
        request.addEventListener( 'load', function () {
            promise.resolve( this.responseText );
        } );
        request.open( "GET", url );
        request.send();
        return promise;
    }

    // Fetch three things in order
    get( 'http://example.com' )
        .then( () => get( 'http://example.net' ) )
        .then( () => get( 'http://example.org' ) );

Promises encapsulate a request and hold on to the response (the
resolution). Order doesn't matter, which fixes another common misstep by
novice async programmers:

    var request = new XMLHttpRequest();

    request.open( "GET", url );
    request.send();

    request.addEventListener( 'load', function () { /* ... */ } );

Here, it is possible that the `send()` finishes immediately, and the
`load` event gets fired _before we added our listener!_ With Promise,
calling `then()` after the promise has been resolved results in the same
behavior, just a little faster:

    var promise = new Promise();
    promise.then( ( response ) => console.log( response ) );
    promise.resolve( "response" );

    var promise = new Promise();
    promise.resolve( "response" );
    promise.then( ( response ) => console.log( response ) );

## Objects

JavaScript objects are unique. But now we've got some much-needed
features that make them more functional and easier to use for those
schooled in `class`ic OO programming.

### Classes

The original JavaScript way to create classes was to create
a constructor `function`. The constructor could define some properties,
and then add object methods to the constructor function's `prototype`
property:

    function Person( name ) {

            // Define the "name" property of the object
            this.name = name;
    }
    // Add a method to say the person's name
    Person.prototype.sayName = function () {
            console.log( this.name );
    };

Now we can use our "class" to create an object with the `new` keyword,
and then use the `sayName()` method to say the person's name:

    var doug = new Person( "Doug" );
    doug.sayName();
    // "Doug"

Class (or `static`) methods go on the constructor function itself:

    Person.birth = function ( parents ) {
        var baby = new Person();
        baby.parents = parents;
        return baby;
    };

To extend a class, we create a new constructor function and assign our
"superclass" to the new constructor function's `prototype`:

    function Employee( name, salary ) {
        Person.call( this, name ); // Superclass constructor
        this.salary = salary;
    }
    // Inherit from the Person class
    Employee.prototype = new Person(); // No name!?

This approach has some problems: First, our constructor is a function
like any other, which means you can accidentally call it as a function.
It won't do what you expect, and it may even break things.

    var lana = Person( "Lana" );
    lana.sayName();
    // Cannot read property "sayName" of undefined

So we didn't get an object, but what did happen?

    console.log( window.name );
    // Lana

The `name` property exists on the `window` object?! When a method is
called without an object, or when a constructor is called without `new`,
the `this` keyword refers to the "default object" (in browsers, the
`window` object, in NodeJS, the `global` object). So, when we called our
constructor as a function, we set the `name` property of the `window`
object.

ES5 added a way to fix this problem, and a few others, by doing `'use
strict'` at the top of your file or function:

    function Person( name ) {

            'use strict';
            // Define the "name" property of the object
            this.name = name;
    }

Functions that run under `'use strict'` do not have a default `this` set
to `window`, so trying to run this constructor as a function again
results in an error:

    var pete = Person( "Pete" );
    // TypeError: Cannot set property 'name' of undefined

Early JavaScript had another problem: Our `Employee` class's `prototype` is an
instance of the `Person` class, but the `Person` constructor requires an
argument that we can't give it! To fix this problem, ES5 added
`Object.create()`, which changes our inherited class to:

    function Employee( name, salary ) {
        Person.call( this, name ); // Superclass constructor
        this.salary = salary;
    }
    // Inherit from the Person class
    Employee.prototype = Object.create( Person.prototype );
    // Fix the "constructor" property
    Employee.prototype.constructor = Employee;

ES5 also introduced "property definitions", which allow us to declare
properties as unwritable, or define functions that will be called when
getting or setting a property. The syntax for it is kind of awkward,
though...

Let's make sure our Employee's salary can't be less than 0 through
functions. We can use `Object.defineProperty` to create a "property
definition". Each property definition can have `get` and `set`
properties to define the getter and setter function for the property.

    Object.defineProperty( Employee.prototype, "salary", {
        get: function () {
            return this._salary;
        },
        set: function ( newSalary ) {
            if ( newSalary < 0 ) {
                throw new Error( "Salary must be greater than 0" );
            }
            this._salary = newSalary;
        }
    } );

ES6 adds a real `class` keyword which allows us to write all this in
a more intuitive fashion. Under the hood, the `prototype` property and
all its associated magic still exist, `class` just makes this easier to
write with fewer things to remember.

Here's our `Person` class again:

    function Person( name ) {

            'use strict';
            // Define the "name" property of the object
            this.name = name;
    }
    // Add a method to say the person's name
    Person.prototype.sayName = function () {
            console.log( this.name );
    };
    // Add a static method to birth new people
    Person.birth = function ( parents ) {
            var baby = new Person();
            baby.parents = parents;
            return baby;
    };


And now here's our `Person` class written using ES6 classes:

    class    Person         {
        constructor( name ) {

            // Define the "name" property of the object
            this.name = name;
        }
        // Add a method to say the person's name
        sayName() {
            console.log( this.name );
        }
        // Add a static method to birth new people
        static birth( parents ) {
            var baby = new Person();
            baby.parents = parents;
            return baby;
        }
    }

Notice first that we have a lot less typing: All references to
`prototype` are gone, and no need to introduce functions with the
`function` keyword. We don't need to `'use strict'`, because all classes
built with the `class` keyword are assumed to be strict.

Our constructor is called simply `constructor`, and can only be used as
a constructor:

    var doug = new Person( "Doug" );
    // OK

    var doug = Person( "Doug" );
    // TypeError: Class constructor Foo cannot be invoked without 'new'

Class methods are now explicitly declared on the class using the
`static` keyword, making them stand out.

ES6 classes also make it easier to inherit from classes. Here's our
`Employee` class again:

    function Employee( name, salary ) {
            Person.call( this, name ); // Superclass constructor
            this.salary = salary;
    }
    // Inherit from the Person class
    Employee.prototype = Object.create( Person.prototype );
    // Fix the "constructor" property
    Employee.prototype.constructor = Employee;

And here's how it looks now with ES6 and the `extends` keyword:

    class Employee extends Person {
        constructor( name, salary ) {
            super( name );             // Superclass constructor
            this.salary = salary;
        }
    }

No need for fiddling around with the Person prototype to ensure Employee
gets linked correctly. No need to "fix" the constructor. We don't even
need to explicitly refer to our Person class in our Employee
constructor, because of a new `super` keyword.

### `super`

The `super` keyword has even more use. In addition to being able to call
our superclass's constructor, we can also call any of our superclass's
methods as well. This is very similar to Java's `super` keyword.


    class Employee extends Person {
        constructor( name, salary ) {
            super( name );             // Superclass constructor
            this.salary = salary;
        }
        // Override the method to also say the person's salary
        sayName() {
            super.sayName();
            console.log( `Salary: ${this.salary}` );
        }
    }

### Object shorthand

We can use the same syntax we use to build classes in every other object
we build as well.

#### Named functions

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

#### Member variables

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

## ES6 Modules

One of the biggest limitations JavaScript has had is a lack of proper
modules and namespaces. Sure, you can split your code into multiple
files, and as long as you get the order right in the markup, everything
will probably work out fine.

ES6 modules allow you to be more explicit and declarative about what
your library exposes to the public. It allows your consumers to pick
only what they want from your module, and it has built-in support for
asynchronous loading (much like JavaScript dependency injection systems
do).

Now we can be explicit about the relationship between our code files and
the symbols within them, and we can load them in the right order
automatically.

    // person.js
    export class Person {
        // ...
    }

    // employee.js
    import Person from person;
    export class Employee extends Person {
        // ...
    }

    // main.js
    import * from employee; // Get everything exported by employee.js
    let me = new Employee( "Doug" );

In our main application, we only need to load our `main.js` file, and
the other files will be loaded first, and our `Employee` object created.

We can export `function`, `class`, `var`, `const`, and `let` from our
modules.

    export const PI = 3.14159;
    export const E = 2.718;

We can also build our export list using the object construction syntax:

           const PI = 3.14159;
           const E = 2.718;
    export { "PI": PI, "E": E };

Which, as mentioned previously, can be done shorter in ES6 as:

           const PI = 3.14159;
           const E = 2.718;
    export {       PI,      E };

## Not covered

* typed arrays (ArrayBuffer)
* proxy objects
* Subclass built-ins like Array, Date, and Element

### New core objects

* Map/WeakMap
* Set/WeakSet

### Extended core objects

* new Array methods
* new Math methods
* new String methods

### DOM

querySelector / querySelectorAll

$( '.foo' );
document.querySelectorAll( '.foo' );

$( '.foo' ).first();
document.querySelector( '.foo' );

var elem = document.getElementById( 'bar' );
elem.querySelector( '.foo' );

### Destructuring

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

## How to use ES6 now

The JavaScript ecosystem has some tools that will let us use ES6
features in web browsers that do not yet support ES6 natively!

For this, we need to install `nodejs` and `npm`. I'll leave that up to
you, Your Milage May Vary based on your OS. For me, it was pretty easy
as I use Homebrew for OSX and just did `brew install nodejs npm`

Then I needed to configure `npm` to install `babel` and `browserify`.

Babel is the ES6 to ES5 compiler system. It's much, much more than that,
too much more than that, but that is what we're using it for. This gets
us our ES6 syntaxes.

Browserify is how we get our ES5 from Babel to work in a browser. This
includes combining all of our individual modules into one file, and sets
up the right functions for loading them so that it all comes together
and works as we expect.

So, first we need to define our project in a file called `package.json`:

    {
      "name": "my-project",
      "version": "1.0.0"
    }

Then we can install babel and our dependencies and tell npm to update
our package.json for us:

    $ npm install --save-dev babel-cli babelify browserify

Once these are installed, npm will add them to our `package.json`:

    {
      "name": "my-project",
      "version": "1.0.0",
      "devDependencies": {
        "babel-cli": "^6.0.0",
        "babelify": "^7.3.0",
        "browserify": "^13.1.1"
      }
    }

By default, babel does nothing. To make it do something, we need to
configure it, and install more plugins. The easiest thing to do is to
install the `latest` bundle. And we'll need the `polyfill` to ensure
proper support of Promises and other expected functions.

    $ npm install --save-dev babel-polyfill babel-preset-latest

Finally, we need a way to make ES6 modules work. Since there are lots of
ways, we'll install the one that babeljs recommends as a default,
CommonJS:

    $ npm install --save-dev babel-plugin-transform-es2015-modules-commonjs

Now with all our stuff installed, we can configure babel using a file
called `.babelrc`:

    {
        "presets": ["latest"],
        "plugins": ["transform-es2015-modules-commonjs"]
    }

We'll put all our source code in a "src/" directory, and then
compile our software using browserify. Since we installed the browserify
command locally in our "node_modules" directory, the easiest way to
invoke it will be to use an "npm script".

npm scripts set up the environment correctly so you can run the
locally-installed programs[1]. These scripts go in our `package.json`
file under a new key called, `"scripts"`. We'll name our script
`"build"` and run the `browserify` command with the appropriate
arguments to read from `src/` and write to `dist/app.js` using
`babelify` to transform our ES6 code to browser-ready code.

    "scripts": {
        "build": "browserify src/* -o dist/app.js -t [ babelify ]"
    },

Finally, we need to make sure our directory structure exists:

    $ mkdir src dist

Now we can add some ES6 code to our "src/" directory and run `npm run
build` to build our software.

    $ npm run build

This creates a file called "app.js" in our "dist/" directory that we can
load in our web browser!

1: These programs are technically installed into `node_modules/.bin`,
but just putting this directory into your `PATH` will only work as long
as you `chdir` into the same directory as your `node_modules` directory.
