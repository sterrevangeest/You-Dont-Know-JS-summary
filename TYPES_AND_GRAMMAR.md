# You Don't Know JS: Types & Grammar


## Chapter 1: Types

### Built-in Types 

Meaning of **types** in JS: a type is an intrinsic, built-in set of characteristics that uniquely identifies the behavior of a particular value and distinguishes it from other values, both to the engine and to the developer.

JS built-in types: 
`Undefined`, `Null`, `Boolean`, `String`, `Number`, `Object` and `Symbol` (added in ES6)
All of these types are called "primitves", except `object`. 

`function` is actually a subtype of object.

### Values as Types 

In JavaScript, variables don't have types -- values have types. Variables can hold any value, at any time.

**`undefinend` vs "undeclared" 

`undefined` value: variables that have no value currently. 

An "undefined" variable is one that has been declared in the accessible scope, but at the moment has no other value in it. By contrast, an "undeclared" variable is one that has not been formally declared in the accessible scope.

JavaScript unfortunately kind of conflates these two terms, not only in its error messages ("ReferenceError: a is not defined") but also in the return values of `typeof`, which is `"undefined"` for both cases.

However, the safety guard (preventing an error) on typeof when used against an undeclared variable can be helpful in certain cases. 

## Chapter 2: Values

Let's look at several of the built-in value types in JS, and explore how we can more fully understand and correctly leverage their behaviors.

### Arrays 

JS `array`s are just containers for any type of value. 

Generally, it's not a great idea to add `string` keys/properties to `array`s. Use `object`s for holding values in keys/properties, and save `arrays` for strictly numerically indexed values.

**Array-Likes**
There will be occasions where you need to convert an `array`-like value (a numerically indexed collection of values) into a true `array`, usually so you can call array utilities (like `indexOf(..)`, `concat(..)`, `forEach(..)`, etc.) against the collection of values.

### Strings 

It's important to realize that JavaScript `string` are really not the same as `array`s of characters.
JavaScript `string`s are immutable, while `array`s are quite mutable.

The other way to look at this is: if you are more commonly doing tasks on your "strings" that treat them as basically arrays of characters, perhaps it's better to just actually store them as arrays rather than as strings. You'll probably save yourself a lot of hassle of converting from string to array each time. You can always call join("") on the array of characters whenever you actually need the string representation.


### Numbers 

JavaScript has just one numeric type: `number`.

- Very large or very small numbers will be default outputted in exponent form, the same as the output of the `toExponential();` method. 
- `toFixed();` method allows you to specify many fractional decimal places you'd like the value to be represented with.

  ```
  var a = 42.59;

  a.toFixed( 0 ); // "43"
  a.toFixed( 1 ); // "42.6"
  ```
  Notice that the output is actually a string representation of the number, and that the value is 0-padded on the right-hand   side if you ask for more decimals than the value holds.
  
- `toPrecision();` specifies how many significant digits should be used to represent the value:

  ```
  var a = 42.59;

  a.toPrecision( 1 ); // "4e+1"
  a.toPrecision( 2 ); // "43"
  a.toPrecision( 3 ); // "42.6"
  ```

- With `Number.EPSILON` you can compare two `number`s for "equality" within the rounding error tolerance. 

- Test if a value is an integer (you have to polyfill for pre-ES6) : `Number.isInteger(..)`

- Test if a value is a safe integer (you have to polyfill for pre-ES6): 
  Max: 9007199254740991 (`Number.MAX_VALUE`) / ES6 (`Number.MAX_SAFE_INTEGER`)
  Min: -9007199254740991 (`Number.MIN_VALUE`) / ES6 (`Number.MIN_SAFE_INTEGER`)
  
- The expression `void ___` "voids" out any value, so that the result of the expression is always the `undefined` value. It   doesn't modify the existing value; it just ensures that no value comes back from the operator expression.

## Chapter 3: Natives
Several times in Chapters 1 and 2, we alluded to various built-ins, usually called "natives".
Here's a list of the most commonly used natives:

* `String()`
* `Number()`
* `Boolean()`
* `Array()`
* `Object()`
* `Function()`
* `RegExp()`
* `Date()`
* `Error()`
* `Symbol()` -- added in ES6!

As you can see, these natives are actually built-in functions.

### Internal `[[Class]]`

Values that are `typeof` `"object"` (such as an array) are additionally tagged with an internal `[[Class]]` property (think of this more as an internal classification rather than related to classes from traditional class-oriented coding). This property cannot be accessed directly, but can generally be revealed indirectly by borrowing the default `Object.prototype.toString(..)` method called against the value.

```
Object.prototype.toString.call( [1,2,3] );			// "[object Array]"

Object.prototype.toString.call( /regex-literal/i );	// "[object RegExp]"
```

### Boxing Wrappers 

These object wrappers serve a very important purpose. Primitive values don't have properties or methods, so to access `.length` or `.toString()` you need an object wrapper around the value. Thankfully, JS will automatically box (aka wrap) the primitive value to fulfill such accesses.

It might seem to make sense to just have the object form of the value from the start, so the JS engine doesn't need to implicitly create it for you. But it turns out that's a bad idea. Browsers long ago performance-optimized the common cases like `.length`, which means your program will actually go slower if you try to "preoptimize" by directly using the object form (which isn't on the optimized path).

**Unboxing**

If you have an object wrapper and you want to get the underlying primitive value out, you can use the `valueOf()` method:

```
var a = new String( "abc" );
var b = new Number( 42 );
var c = new Boolean( true );

a.valueOf(); // "abc"
b.valueOf(); // 42
c.valueOf(); // true
```


### Natives as Constructors 

**Array**

Notice:

```
var a = new Array( 4 );                         
var b = [ undefined, undefined, undefined ];
var c = [];
c.length = 5;

a;    //length 4
b;    //length 3
c;    //length 5
```
So, if you wanted to actually create an array of actual undefined values (not just "empty slots"), how could you do it (besides manually)?:
```
var a = Array.apply( null, { length: 3 } );
a; // [ undefined, undefined, undefined ]
```
(Detailed expl. in book)

**Regular expressions**
`RegExp(..)` has some reasonable utility: to dynamically define the pattern for a regular expression. 

```
var name = "Kyle";
var namePattern = new RegExp( "\\b(?:" + name + ")+\\b", "ig" );

var matches = someText.match( namePattern );
```
This kind of scenario legitimately occurs in JS programs from time to time, so you'd need to use the new RegExp("pattern","flags") form.

### Date(..) and Error(..)
The `Date(..)` and `Error(..)` native constructors are much more useful than the other natives, because there is no literal form for either.

- To create a date object value, you must use `new Date()`. The `Date(..)` constructor accepts optional arguments to specify the date/time to use, but if omitted, the current date/time is assumed. By far the most common reason you construct a date object is to get the current timestamp value (a signed integer number of milliseconds since Jan 1, 1970). You can do this by calling `getTime()` (or `Date.now()`)on a date object instance.

```
if (!Date.now) {
	Date.now = function(){
		return (new Date()).getTime();
	};
}
```

- The `Error(..)` constructor behaves the same with the `new` keyword present or omitted. The main reason you'd want to create an error object is that it captures the current execution stack context into the object (in most JS engines, revealed as a read-only `.stack` property once constructed). This stack context includes the function call-stack and the line-number where the error object was created, which makes debugging that error much easier. You would typically use such an error object with the `throw` operator:
 
 ```
function foo(x) {
	if (!x) {
		throw new Error( "x wasn't provided" );
	}
	// ..
}
```
 ### Symbols 
 
New in ES6. 
Symbols are special "unique" (not strictly guaranteed!) values that can be used as properties on objects with little fear of any collision.

### Natice prototypes 

Each of the built-in native constructors has its own `.prototype` object -- `Array.prototype`, `String.prototype`, etc. These objects contain behavior unique to their particular object subtype.

Note: By documentation convention, `String.prototype.XYZ` is shortened to `String#XYZ`, and likewise for all the other `.prototype`s. 

- String#indexOf(..): find the position in the string of another substring
- String#charAt(..): access the character at a position in the string
- String#substr(..), String#substring(..), and String#slice(..): extract a portion of the string as a new string
- String#toUpperCase() and String#toLowerCase(): create a new string that's converted to either uppercase or lowercase
- String#trim(): create a new string that's stripped of any trailing or leading whitespace

None of the methods modify the string in place. Modifications (like case conversion or trimming) create a new value from the existing value.

## Chapter 4: Coercion

Some say: - coercion is magical, evil, confusing, and just downright a bad idea.

Converting a value from one type to another is often called "type casting," when done explicitly, and "coercion" when done implicitly (forced by the rules of how a value is used).

**"implicit coercion" vs. "explicit coercion."**

**"explicit coercion"** is when it is **obvious** from looking at the code that a type conversion is intentionally occurring, whereas **"implicit coercion"** is when the type conversion will occur as a **less obvious side effect** of some other intentional operation. 














