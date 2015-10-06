
# JavaScript Style Guide

* Influenced by
[https://github.com/dockyard/styleguides/blob/master/javascript.md](https://github.com/dockyard/styleguides/blob/master/javascript.md)

* The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)

## Table Of Contents

### Conventions

* [ES2015 and ES2016](#es2015-and-es2016)
* [Line Endings](#line-endings)
* [Type Coercion](#type-coercion)

### Grammar

* [Block Statements](#block-statements)
* [Conditional Statements](#conditional-statements)
* [No-op conditionals](#no-op-conditionals)
* [Commas](#commas)
* [Semicolons](#semicolons)
* [Comments](#comments)
* [Variables](#variables)
* [Whitespace](#whitespace)
* [Newlines](#newlines)
* [Naming conventions](#naming-conventions)
* [Line Lengths](#line-lengths)

### Constructors

* [Constructors](#constructors-1)

### Objects

* [Objects](#objects-1)
* [Properties](#properties)

### Strings

* [Strings](#strings-1)

### Arrays

* [Arrays](#arrays-1)

### Functions

* [Functions](#functions-1)
* [Function Arguments](#function-arguments)

### Enumerables

* [Enumerables](#enumerables-1)



## ES2015 and ES2016

* ES2015 and ES2016 syntax **MUST** be employed whenever possible


## Line Endings

* Use *nix line endings


## Type Coercion

* **MUST** be explicit
* **MUST** use coercion methods available on types over global methods

```javascript
// good
( 123 ).toString();

Date().toString();

let value = Number( '3.14' );

return Boolean( result );


// bad
String( 123 );

String( Date() );

let value = + "3.14";

return !!result;
```


## Block Statements

* **MUST** use spaces before leading brace

```javascript
// good
function doStuff( foo ) {
    return foo;
};

// bad
function doStuff( foo ){
    return foo;
};
```

* **SHOULD** use spaces after leading parenthesis
* **SHOULD** use spaces before closing parenthesis when part of block statment
on a single line
* **SHOULD NOT** use spaces before closing parenthesis when part of a series of
closing parenthesis or brackets on their own line

```javascript
// should use leading space
// should use closing space
return fooBar( foo );

// should use leading space
// should not use closing space
return fooBar( foo, ( bars ) => {
    return bars.map( ( bar ) => {
        return bar * 2;
    });
});
```

* Opening curly brace (`{`) **MUST** be on the same line as the beginning of a
statement or declaration

```javascript
// good
return fooBar( foo, ( bars ) => {
    return bars.map( ( bar ) => {
        return bar * 2;
    });
});

//bad
return fooBar( foo, (bars) =>
{
    return bars.map((bar) =>
    {
        return bar * 2;
    });
});
```

* **MUST** keep `else` and its accompanying braces on the same line

```javascript
let bar;

// good
if ( 1 === foo ) {
    bar = 2;
} else {
    bar = '2';
}

// bad
if ( 1 === foo ) {
    bar = 2;
}
else if ( 2 === foo ) {
    bar = 1;
}
else {
    bar = 3;
}
```

* **MUST** use multiline format

```javascript
// good
while ( foo > 3 ) {
    ...
}

// bad
while ( foo > 3 ) { ... }
```



## Conditional Statements

* **MUST NOT** contain value assignments

```javascript
// bad
if ( someVariable = 'someString' ) {
    ...
}
```

* **MUST** use strict equality (`===` and `!==`)

* When using strict equality you **MUST** place the value on the left of the
operation and the item you are checking on the right

```javascript
// good
if ( false === foo ) {
    ...
}

// bad
if ( foo === false ) {
    ...
}
```

* Non-strict equality comparisons **SHOULD** be constructed in a
"grammar-friendly" order


```javascript
// recommended
if ( itemCount > 0 ) {
    ...
}

// allowed
if ( 0 < itemCount ) {
    ...
}
```

* **MUST** use curly braces for all conditional blocks

```javascript
// good
if ( notFound ) {
    doBarrelRoll();
} else {
    jumpOnCouch();
}

// bad
if ( notFound )
    doBarrelRoll();
else
    jumpOnCouch();
```

* **MUST** use explicit conditions when checking for non `null`, `undefined`,
`true`, `false` values

```javascript
const foo = 'foo';
const arr = [ 1, 2, 3 ];

// good
if ( arr.length > 0 ) {
    return;
}

if ( '' !== foo ) {
    ...
}

// bad
if ( !arr.length ) {
    return;
}

if ( !foo ) {
    return;
}
```

* **MUST** use multiline format

```javascript
// good
if ( 'bar' === foo ) {
    return;
}

// bad
if ( 'bar' === foo ) { return; }
```


## No-op conditionals

* **MUST** be placed as close to the beginning of the logic block as possible,
including before `let`s or `const`s

```javascript
// good
sortColumn( column ) {
    if ( this.loading ) {
        return;
    }

    let columnTitle = column.title;

    if ( !columnTitle ) {
        return;
    }

    let firstName = 'Joe';
    let lastName = 'Smith';
}

// bad
sortColumn( column ) {
    let columnTitle = column.title;
    let firstName = 'Joe';
    let lastName = 'Smith';

    if ( this.loading ) {
        return;
    }

    if ( !columnTitle ) {
        return;
    }
}
```


## Commas

* **MUST NOT** use trailing commas

```javascript
// good
const foo = {
    bar: [ 1, 2, 3 ],
    baz: {
        a: 'a'
    }
}

// bad
const foo = {
    bar: [1, 2, 3],
    baz: {
        a: 'a',
    },
}
```

* **MUST NOT** use leading commas

```javascript
// good
const potato = [
    'potatoes',
    'are',
    'delicious'
];

// bad
const potato = [
    'potatoes'
    , 'are'
    , 'delicious'
];
```


## Semicolons

* Use semicolons`;`

## Comments

* [JSDoc conventions](http://www.usejsdoc.org/) **MUST** be used at all times

* There are two types of descriptions:
    * short, usually consisting of a single line. **SHOULD** almost always
    be populated and **MUST** placed on the first line of a DocBlock when
    it is
    * long, consisting of multiple lines of text. Is **OPTIONAL** and if
    populated it **MUST** be placed after the short description

* There **MUST** always be a single, empty line after each of the description
types

* There **MUST** always be a single, empty line after the end of each
`@example` block

* You **MUST NOT** use synonyms (e.g. use `@returns` instead of `@return`)

* `@type` and `@param` types **MUST** be capitalized (e.g. Number, Boolean,
Object, Function, ...)
    * The exception to this is `undefined` and `null`, both which **MUST**
    remain lowercased

* When a function does not return a value it **MUST** be documented as
returning `undefined`

```
@returns {undefined}
```

* Characters **SHOULD NOT** be spaced out for evenness

```javascript
/**
 * Bad spacing example
 *
 * @param   {Number}  age        Age of the registrant
 * @param   {String}  fullName   Fullname of the registrant
 * @param   {Boolean} registered Whether registrant is registered
 * @returns {undefined}
 */
function( age, fullName, registered ) { ... }

/**
 * Good spacing example
 *
 * @param {Number} age Age of the registrant
 * @param {String} fullName Fullname of the registrant
 * @param {Boolean} registered Whether registrant is registered
 * @returns {undefined}
 */
function( age, fullName, registered ) { ... }
```

* When specifying a value with the `@augments` tag...
    * ...classes referenced from external libraries **MUST** be referenced in
    the format of `<library>/<pathing>/<class>`
    * ...classes referenced from within the same project **MUST** be referenced
    beginning with `module:` and then the path to the class

* Optional parameters **MUST** be denoted via the use of brackets ( `[ ]` )
around the parameter name and not an equal sign (=) after the type value

* `@typedef` blocks **SHOULD** be positioned before the DocBlocks that use them

* **MUST** use `//` for non-documenting comments (both single and multiline)

```javascript
// good
function foo() {
  const bar = 5;

  // multiplies `bar` by 2.
  const newBar = fooBar( bar );

  console.log( newBar );
}

// bad
function foo() {
  const bar = 5;

  /* multiplies `bar` by 2. */
  const newBar = fooBar( bar );

  console.log( newBar );
}
```


## Variables

* **MUST NOT** ever use `var`.
* **MUST** use `const` to declare variables with a constant reference (not
value)
* **MUST** use `let` to declare variables with a variable reference
* **MUST** use `const` over `let` whenever possible


```javascript
// good
const a = [ 1, 2, 3 ];
let b = [ 4, 5, 6 ];

function doStuff() {
    b = [ 1, 2, 3 ];

    return b;
}

// bad
var a = [1, 2, 3];
let b = [1, 2, 3];
```

* Note that `const` refers to a **constant reference**, not a constant value

```javascript
const coolKids = ['Estelle', 'Lauren', 'Romina'];
coolKids.push('Marin');
console.log(coolKids); // ['Estelle', 'Lauren', 'Romina', 'Marin']

coolKids = ['Doug', 'Lin', 'Dan']; // SyntaxError: "coolKids" is read-only
```

* Note that both `let` and `const` are block scoped

```javascript
{
    let a = 1;
    const b = 2;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

* `const`s **SHOULD** be defined inline where they are needed and don't need to
be placed at the beginning of the scoped block

```javascript
// good
const isTrue = true;

if ( foo ) {
    const bar = 123;
}

const baz = 456;


// bad
const bar = 123;
const isTrue = true;
if ( foo ) { ... }
```

* **MUST** use a single `const` declaration for each assignment

```javascript
// good
const a = 1;
const b = 2;

// bad
const a = 1, b = 2;
```

* **MUST** use a single `let` declaration for each assignment

```javascript
// good
let a = 1;
let b = 2;

// bad
let a = 1, b = 2;
```


* `let`s **SHOULD** be defined inline where they are needed and don't need to
be placed at the beginning of the scoped block

```javascript
sortColumn( column ) {
    let columnTitle = Ember.get( column, 'title' );

    columnTitle = columnTitle + 'test';

    let firstName = Ember.get( column, 'firstName' );
    let lastName = Ember.get( column, 'lastName' );
}
```


## Whitespace

* **MUST** use soft tabs set to 4 spaces

```javascript
// good
function() {
∙∙∙∙const name;
}

// bad
function() {
⇥const name;
}
```

* **MUST** place 1 space before a leading brace (`{`)

```javascript
// good
obj.set( 'foo', {
    foo: 'bar'
});

test( 'foo-bar', function() {
    ...
});

// bad
obj.set( 'foo',{
    foo: 'bar'
});

test( 'foo-bar', ()=>{
    ...
});
```

* **MUST NOT** use spaces before semicolons

```javascript
// good
const foo = {};

// bad
const foo = {} ;
```

* **MUST** keep parentheses adjacent to the function name when declared or called

```javascript
// good
function foo( bar ) {
    ...
}

foo( 1 );

// bad
function foo (bar) {
    ...
}

foo ( 1 );
```

* Operators by default **MUST** have spaces around them *...EXCEPT...*
    * Non-word unary operators **MUST NOT**

```javascript
// good
if ( !available ) {
    ...
}

if ( !( 'foo' in obj ) ) {
    ...
}

i++;

let j = -x;

delete myVariable;

let a = b;


// bad
if ( ! available ) {
    ...
}

if ( ! ( 'foo' in obj ) ) {
    ...
}

i ++;

let j = - x;

let a=b;
```


* **MUST NOT** aligning on colons and equal signs

```javascript
// good
let animal = 'cat';
let car = 'fast';

callThisFunction({
    display: true,
    collapsible: false,
    order: 'ASC'
});

// bad
let animal = 'cat';
let car    = 'fast';

callThisFunction({
    display     : true,
    collapsible : false,
    order       : 'ASC'
});
```

* Lines **MUST NOT** contain any trailing whitespaces

* Blank lines **MUST NOT** contain any whitespace (not the same thing as a
newline)

* It is **RECOMMENDED** the following deviations be observed:

```javascript
// Function accepting an array, no space
foo([ 'alpha', 'beta' ]);

// Function accepting an object, no space
foo({
    a: 'alpha',
    b: 'beta'
});
```

## Newlines

* `Array`s and `Object`s can be defined with all of their elements or
properties on a single line, but **MUST** be kept to 80 characters or less in
length

* Once a newline is introduced into the definition of the `Array` or `Object`
then ALL elements or properties **MUST** be defined on a newline


```javascript
// good

let test = [ 'okay', 'good' ];

let test = [
    'okay',
    'good'
];

let test = { okay: true };

let test = {
    okay: true
};

let test = { okay: true, wrong: false };

let test = {
    okay: true,
    wrong: false
};

let test = [
    'okay',
    { okay: true }
];

let test = {
    bindings: [
        'okay'
    ]
};


// bad

let test = [ 'okay', 'nope'
];

let test = [
    'okay', { okay: false }
];

let test = { okay: false
};

let test = {
    bindings: [ 'okay' ]
};
```


## Naming Conventions

* **MUST** be descriptive with naming

```javascript
// good
function checkValidKey( key, value = 0 ) {
    ...
}

// bad
function check( k, v = 0 ) {
    ...
}
```

* **SHOULD** name collection objects in either plural form or with an appended plural term (i.e., "items", "collection")

```javascript
let item = {};
let items = [ item ];

let hardware = new Hardware();
let hardwareCollection = [ hardware, new Hardware() ];
```

* Acronyms **MUST** be treated as words when in names

```javascript
// Good
let createHtmlSnippet;

// Bad
let createHTMLSnippet;
```

* Only industry-standard, unambiguous abbreviations **MUST** be used
    * When used, abbreviations **MUST** be used universally in the entire
    codebase, or not at all
    * When in doubt about clarity, you **MUST NOT** abbreviate
    * If you would expand the abbreviation when saying it out loud, you
    **MUST NOT** abbreviate

```javascript
// Good
var http, tcp, ip;

// Bad
var hw, sw, vg; // hardware, software, virtual guest
```

* Names **MUST** be as explicit as necessary to avoid any ambiguity in context.
When there are two similar names, at least one of them **MUST** call out
exactly what differentiates it.

```javascript
// Contrived example copies items from `virtualGuests` to another array
let virtualGuests;
let filteredVirtualGuests;
```

* camelCase **MUST** be used for functions, objects, instances, let, const, etc

```javascript
// good
const presenceValidator = true;

let fooBar = function fooBar() {
    ...
}
```

* PascalCase **MUST** only be used for constructors, classes or enums

```javascript
// good
class Validator {
    constructor( options ) {
        this.rules = options.rules;
    }
}

const presenceValidator = new Validator({
    rules: {}
});

/* @enum {String} */
const ColumnSize = Object.freeze({
    LARGE: 'large',
    MEDIUM: 'medium'
});

// bad
function Validate (options ) {
    return options === true;
}

const validatedItem = Validate( item );

/* @enum {String} */
const columnSize = Object.freeze({
    LARGE: 'large',
    MEDIUM: 'medium'
});
```

* Imports that are either module (namespaces) or classes **MUST** be
capitalized.  All others **MUST** be lowercased.

```javascript
// good
import Ember from 'ember';
import Route from 'app/base/route';
import ModalManager from 'sl-ember-components/mixins/sl-modal-manager';
import PreferenceAdapter from 'app/adapters/preference';
import config from 'app/config/environment';

var translations = require( 'translations/en' );
```


## Line Lengths

* The number of characters per line **MUST NOT** exceed 120 and 80 **SHOULD**
be considered the preferred limit.


## Constructors

* **MUST** use `class` instead of manipulating `prototype`

```javascript
// good
class Car {
    constructor( make = 'Tesla' ) {
        this.make = make;
        this.position = { x: 0, y: 0 };
    }

    move( x = 0, y = 0 ) {
        this.position = { x, y };

        return this.position;
    }
}

// bad
function Car( make = 'Tesla' ) {
    this.make = make;
    this.position = { x: 0, y: 0 };
}

Car.prototype.move = function( x = 0, y = 0 ) {
    this.position = { x, y };

    return this.position;
}
```

* **MUST** use `extends` for inheritance

```javascript
class HondaCivic extends Car {
    constructor() {
        super( 'Honda' );
    }
}
```

## Objects

* **MUST** use literal form for object creation

```javascript
// good
let doug = {};

// bad
let doug = new Object();
```

* **MUST** pad single-line objects with white-space

```javascript
// good
let rob = { likes: 'ember' };

// bad
let rob = {hates: 'spiders'};
```

## Strings

* **SHOULD** use single quotes, and use double quotes to avoid escaping

```javascript
// good:
const foo = 'bar';
const baz = "What's this?";

// bad
const foo = "bar";
```

* **SHOULD** use templates strings when constructing strings with dynamic values

```javascript
const prefix = 'Hello';
const suffix = 'and have a good day.';

// good
return `${prefix} world, ${suffix}`;

// bad
return prefix + ' world, ' + suffix;
```

## Arrays

* **MUST** use literal form for array creation (unless you know the exact length)

```javascript
// good
const foo = [ 1, 2, 3 ];
const bar = new Array(3);
let list = [];

// bad
const foo = new Array();
const bar = new Array( 1, 2, 3 );
let list = new Array();
```

* **MUST** use `new Array` if you know the exact length of the array and know
that its length will not change

```javascript
const foo = new Array(16);
```

* **MUST** use `push` to add an item to an array

```javascript
const foo = [];
const { length } = foo;

// good
foo.push( 'bar' );

// bad
foo[length] = 'bar';
```

* **MUST** use spread

```javascript
// join 2 arrays
const foo = [ 0, 1, 2 ];
const bar = [ 3, 4, 5 ];

foo.push( ...bar );

// avoid using `Function.prototype.apply`
const values = [ 25, 50, 75, 100 ];

// good
const max = Math.max.apply( Math, values );

// better
const max = Math.max( ...values );
```

* **MUST** join single line array items with a space

```javascript
// good
const foo = [ 'a', 'b', 'c' ];

// bad
const foo = ['a','b','c'];
```

* **MUST** use array destructuring

```javascript
const arr = [ 1, 2, 3, 4 ];

// good
const [ head, ...tail ] = arr;

// bad
const head = arr.shift();
const tail = arr;
```

## Properties

* **MUST** use property value shorthand

```javascript
const name = 'Derek Zoolander';
const age = 25;

// good
const foo = {
    name,
    age
};

// bad
const foo = {
    name: name,
    age: age
};
```

* **MUST** use dot-notation when accessing properties

```javascript
const foo = {
    bar: 'bar'
};

// good
foo.bar;

// bad
foo[ 'bar' ];
```

* **MUST** use `[]` when accessing properties via a variable

```javascript
const propertyName = 'bar';
const foo = {
    bar: 'bar'
};

// good
foo[propertyName];

// do you even javascript
foo.propertyName;
```

* **MUST** use object destructuring when accessing multiple properties on an object

```javascript
// good
function foo( person ) {
    const { name, age, height } = person;

    return `${name} is ${age} years old and ${height} tall.`
}

// bad
function foo( person ) {
    const name = person.name;
    const age = person.age;
    const height = person.height;

    return `${name} is ${age} years old and ${height} tall.`
}
```

## Functions

* **MUST** use object method shorthand

```javascript
// good
const foo = {
    value: 0,

    bar( value ) {
        return foo.value + value;
    }
};

// bad
const foo = {
    value: 0,

    bar: function bar( value ) {
        return foo.value + value;
    }
};
```

* **MUST** use scope to lookup functions (not variables)

```javascript
// good
function foo() {
    function bar() {
        ...
    }

    bar();
}

// bad
function foo() {
    const bar = function bar() {
        ...
    }

    bar();
}
```

* **MUST** use fat arrows to preserve `this` when using function expressions or
anonymous functions

```javascript
const foo = {
    base: 0,

    // good
    bar( items ) {
        return items.map( ( item ) => {
            return this.base * item.value;
        });
    },

    // good
    bar( items ) {
        return items.map( ( item ) => this.base + item.value );
    },

    // bad
    bar( items ) {
        const _this = this;

        return items.map( function( item ) {
            return _this.base + item.value;
        });
    }
};
```

* **MUST** always use parentheses around arguments

```javascript
// good
[ 1, 2, 3 ].map( ( x ) => x * x );

// bad
[ 1, 2, 3 ].map( x => x * x );
```

* If the function body fits on one line, feel free to omit the braces and use
implicit return. Otherwise, add the braces and use a return statement.

```javascript
// good
[ 1, 2, 3 ].map( ( x ) => x * x );

// good
[ 1, 2, 3 ].map( ( x ) => {
    return { number: x };
});
```

* It is **RECOMMENDED** to use fat arrows over the more verbose anonymous
function syntax


## Function Arguments

* **MUST** never use `arguments` – use rest instead

```javascript
// good
function foo( ...args ) {
    return args.join( '' );
}

// bad
function foo() {
    const args = Array.prototype.slice.call( arguments );

    return args.join( '' );
}
```

* `arguments` object must not be passed or leaked anywhere. See the
[reference](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers#3-managing-arguments).

* **MUST NOT** re-assign the arguments

```javascript
// good
// Use a new variable if you need to assign the value of an argument
function fooBar( opt ) {
    let _opt = opt;

    _opt = 3;
}

// bad
function fooBar() {
    arguments = 3;
}

// bad
function fooBar( opt ) {
    opt = 3;
}
```

* **MUST** use default params instead of mutating them

```javascript
// good
function fooBar( obj = {}, key = 'id', value = 0 ) {
    if ( key ) {
        return obj[key];
    } else {
        obj[key] = value;
        return obj[key];
    }
}

// bad
function fooBar( obj, key, value ) {
    obj = obj || {};
    key = key || 'id';
    ...
}
```


### Enumerables

Although enumerables don't exist natively in JavaScript yet, the functionality to create an object structure analogous to an enum exists using a combination of ES2015's `const` with `Object.freeze()`.

```javascript
const EnumType = Object.freeze({
    FIRST: 'first',
    SECOND: 'second',
    THIRD: 'third'
});
```

When defining an enum type in an ES2015 module intended to be used by other consuming code, you should then export the enum in a block with the enum.

```javascript
export { EnumType };
```
