
# ES6 for Occasional JavaScript Devs

<http://preaction.github.io/ES6-For-JS-Irregulars/>

<div style="width: 40%; float: left">

by [Doug Bell](http://preaction.me)  
<small>(he, him, his)</small>  
[<i class="fa fa-twitter"></i> @preaction](http://twitter.com/preaction)  
[<i class="fa fa-github"></i> preaction](http://github.com/preaction)  
[Chicago.PM](http://chicago.pm.org)  

</div>
<div style="width: 20%; float: left; text-align: center">
<img src="http://preaction.me/images/avatar-small.jpg" style="display: inline-block; max-width: 100%"/>
</div>
<div style="width: 40%; float: left">

[<i class="fa fa-file-text-o"></i> Notes](https://github.com/preaction/ES6-For-JS-Irregulars/blob/master/NOTES.md)  
<small> </small>  
[Source on <i class="fa fa-github"></i>](https://github.com/preaction/ES6-For-JS-Irregulars/)  

[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode)  

<small>
For speaker view, press `S`<br/>
For full-screen, press `F`
</small>
</div>

------

# JavaScript

---

# Love it

---

# Loathe it

---

# Need it

------

# Evolving

---

# ECMA

---

# ECMAScript 3

---

# 1999

---

# `try/catch`

---

# Regular expressions

---

# ECMAScript 5

There is no 4

---

# 2009

---

# 'use strict'

---

# JSON

---

# ECMAScript 6

---

# 2015

------

# Variables and Data Structures

------

# `let`

---

# Block scoping

---

```
var ary = [ 0, 1, 2, 3 ];
for ( var i = 0; i < ary.length; i++ ) {

        setTimeout(
            function () {
                console.log( i );
            },
            1000 * i
        );

}
```

---

```
4
4
4
4
```

---

```
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
```

---

```
0
1
2
3
```
---

```
var ary = [ 0, 1, 2, 3 ];
for ( let i = 0; i < ary.length; i++ ) {

        setTimeout(
            function () {
                console.log( i );
            },
            1000 * i
        );

}
```

------

# `const`

---

# Block-scoped Constants

---

```
const PI = 3.1415925;
const E = 2.71828;
```

------

# Template Literals

---

# String Interpolation

---

```
var name = "Doug";
console.log( "Hello, " + name );
```

---

```
var name = "Doug";
var hello = `Hello, ${name}`;
```

---

# Expressions

---

```
console.log( `2π = ${ Math.PI * 2 }` );
```

Also, Unicode!

---

# Multiline strings

---

```
console.log( `This is a string
    on multiple lines` );
```

------

# Object Shorthand

---

# Typing in objects

---

# `{ ... }`

---

# Named Functions

---

```
var calc = {
    add: function add( x, y ) { return x+y }
};
```

---

```
var calc = {
                  add( x, y ) { return x+y }
};
```

---

# Member variables

---

```
var   PI = 3.1415925;
var   E = 2.71623;
var numbers = {
    PI: PI,
    E:  E
};
```

---

```
const PI = 3.1415925;
const E = 2.71623;
let numbers = {
        PI,
        E
};
```

---

# Together

---

```
var Math = {
    add: function add( x, y ) { return x + y },
    PI: PI,
    E:  E
};
```

---

```
let Math = {
                  add( x, y ) { return x+y }
        PI,
        E
};
```

------

# Destructuring

---

# Fill many values from a function

---

# Arrays

---

```
let ary = [ 0, 1, 2, 3, 4 ];
let [ first, second ] = ary;

console.log( first );
console.log( second );
```

---

# Objects

---

```
const FOO = Math.PI, BAR = Math.E;
const { "PI": FOO, "E": BAR } = Math;
```

------

# Iterables

---

# New interface

---

# More predictable array loops

---

# Old: `for...in`

---

```
let ary = [ 1, 2, 3, 4 ];
ary.foo = "bar"; // Arrays are objects and can have properties
for ( let i in ary ) {
    console.log( i );
}
```

---

```
1
2
3
4
foo // ???
```

---

# New: `for...of`

---

```
let ary = [ 1, 2, 3, 4 ];
ary.foo = "bar"; // Arrays are objects and can have properties
for ( let i of ary ) {
    console.log( i );
}
```

---

```
1
2
3
4
```

------

# Functions and Arguments

------

# Default Param Values

---

```
function pow( x, y=1 ) {
    return Math.pow( x, y );
}
```

------

# Spread Operator

---

# Use an array of arguments

---

```
function add( x, y ) {
    return x + y;
}
let nums = [ 1, 2 ];

console.log( add.apply(    nums ) );
```

---

```
function add( x, y ) {
    return x + y;
}
let nums = [ 1, 2 ];

console.log( add      ( ...nums ) );
```

------

# Rest Operator

---

```
function sum( ) {
    var sum = 0;
    for ( var i = 0; i < arguments.length; i++ ) {
        sum += arguments[i];
    }
    return sum;
}
```

---

```
function sum( ...numbers ) {
    var sum = 0;
    for ( var i = 0; i < numbers  .length; i++ ) {
        sum += numbers  [i];
    }
    return sum;
}
```

------

# Questions?

---

# Thank you!

