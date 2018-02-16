# You-Dont-Know-JS-summary
## Up & Going
## _Chapter 1: Into Programming + Chapter 2: Into Javascript_

## Statements
###  Statement

is a group of words, numbers, and operators that performs a specific task. 

`a = b * 2` 

### Variables
Variables hold values 

`a` and `b`

### Operators
Perform actions with the values and variables (such as assignment and mathematic multiplication).

`=` and `*`

## Expressions 
Statements are made up of one or more expressions. An **expression** is any reference to a variable or value, or a set of variable(s) and value(s) combined with operators.

For example:

`a = b * 2;`
This statement has four expressions in it:

- `2` is a literal value expression
- `b` is a variable expression, which means to retrieve its current value
- `b * 2` is an arithmetic expression, which means to do the multiplication
- `a = b * 2` is an assignment expression, which means to assign the result of the b * 2 expression to the variable a (more on assignments later)

### Call expression

`alert( a );` 

### Const

In the newest version of JavaScript, a new way to declare constants, by using `const` instead of `var`.

`const TAX_RATE = 0.08;`

Constants are useful just like variables with unchanged values, except that constants also prevent accidentally changing value somewhere else after the initial setting.

## Conditionals

`if..else` statement.

_if_ this condition is (not) true, do the following...
_else_ do this.

```
if (a == 2) {
	// do something
}
else if (a == 10) {
	// do another thing
}
else if (a == 42) {
	// do yet another thing
}
else {
	// fallback to here
}
```
### Conditional operator

A more concise form of a single `if..else` statement

```
var a = 42;
var b = (a > 41) ? "hello" : "world";
```

If the test expression (`a > 41` here) evaluates as `true`, the first clause `("hello")` results, otherwise the second clause `("world")` results, and whatever the result is then gets assigned to `b`.

## Loops
### `while` loop

```
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
}
```

### `do..while` loop**

```
do {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```
### `for` loop** 
The `for` loop has three clauses: 
- the initialization clause (`var i=0`)
- the conditional test clause (`i <= 9`)
- the update clause (`i = i + 1`)
`for` loop is more compact and often easier form to understand and write. 

```
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
```

## Functions

A function is generally a named section of code that can be "called" by name, and the code inside it will be run each time

Functions can:
- optionally take arguments (aka parameters) -- values you pass in. 
- And they can also optionally return a value back.

Are often used for:
- code that you plan to call multiple times
- organising related bits of code into named collections

```
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}

var amount = 99.99;

amount = calculateFinalPurchaseAmount( amount );

console.log( amount.toFixed( 2 ) );		// "107.99"
```

### Scope 

In JS every function gets it's own scope. Scope is basically al collection of variables as well as the rules for how those variables are accessed by name. Only code inside that function can access that function's _scoped_ values. 

Also, a scope can be nested inside another scope. The code inside the innermost scope can access varaibles from either scope. 

```
function outer() {
	var a = 1;

	function inner() {
		var b = 2;

		// we can access both `a` and `b` here
		console.log( a + b );	// 3
	}

	inner();

	// we can only access `a` here
	console.log( a );			// 1
}

outer();
```

So, code inside the `inner()` function has access to both variables `a` and `b`, but code in `outer()` has access only to `a` -- it cannot access `b` because that variable is only inside `inner()`.

## Functions As Values

Though it may not seem obvious from that syntax, `foo` is basically just a variable in the outer enclosing scope that's given a reference to the function being declared. That is, the `function` itself is a value, just like `42` or `[1,2,3]` would be.

```
function foo() {
	// ..
}
```

### Immediately Invoked Function Expressions (IIFEs)

There's another way to execute a function expression, which is typically referred to as an _immediately invoked function expression_ (IIFE):

```
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

Because an IIFE is just a function, and functions create variable scope, using an IIFE in this fashion is often used to declare variables that won't affect the surrounding code outside the IIFE.

### Closure 
You can think of closure as a way to "remember" and continue to access a function's scope (its variables) even once the function has finished running.

- **Modules**
The most common usage of **closure** in JavaScript is the module pattern. 
Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as a public API that is accessible from the outside.

More detailed explanation about Closure and Modules [here](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch2.md#closure).

## This Identifier

If a function has a `this` reference inside it, that `this` reference usually points to an `object`. But which `object` it points to depends on how the function was called.

Important: realize that `this` does not refer to the function itself. 

```
function foo() {
	console.log( this.bar );
}

var bar = "global";

var obj1 = {
	bar: "obj1",
	foo: foo
};

var obj2 = {
	bar: "obj2"
};

// --------

foo();				// "global"
obj1.foo();			// "obj1"
foo.call( obj2 );		// "obj2"
new foo();			// undefined
```

There are four rules for how this gets set, and they're shown in those last four lines of that snippet.

1. `foo()` ends up setting `this` to the global object in non-strict mode -- in strict mode, `this` would be `undefined` and you'd get an error in accessing the `bar` property -- so `"global"` is the value found for `this.bar`.
2. `obj1.foo()` sets this to the `obj1` object.
3. `foo.call(obj2)` sets this to the `obj2` object.
4. `new foo()` sets `this` to a brand new empty object.


## Objects

The `object` type refers to a compound value where you can set properties (named locations) that each hold their own values of any type.

```
var obj = {
	a: "hello world",
	b: 42,
	c: true
};

obj.a;		// "hello world"
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world"
obj["b"];	// 42
obj["c"];	// true

```

### Arrays

An array is an `object` that holds values of any type not particularly in named properties/keys, but rather in numerically indexed positions. 

```
var arr = [
	"hello world",
	42,
	true
];

arr[0];			// "hello world"
arr[1];			// 42
arr[2];			// true
arr.length;		// 3

typeof arr;		// "object"
```
## Coercion

Coercion comes in two forms in JavaScript: _explicit_ and _implicit_.

- Explicit coercion: you can see obviously from the code that a conversion from one type to another will occur
```
var a = "42";

var b = Number( a );

```

- Implicit coercion: when the type conversion can happen as more of a non-obvious side effect of some other operation
```
var a = "42";

var b = a * 1;	// "42" implicitly coerced to 42 here

```

## Equality 

### Equality

- `=` equals 
- `==` checks for value equality with coercion allowed
- `===` checks for value equality without allowing coercion
- `!==` not equal

### Inequality

The `<`, `>`, `<=`, and `>=` operators are used for inequality.

### Strict Mode

ES5 added a "strict mode" to the language, which tightens the rules for certain behaviors. Generally, these restrictions are seen as keeping the code to a safer and more appropriate set of guidelines. You can opt in to strict mode for an individual function, or an entire file, depending on where you put the strict mode pragma.

Strict mode makes several changes to normal JavaScript semantics :

1. Eliminates some JavaScript silent errors by changing them to throw errors.
2. Fixes mistakes that make it difficult for JavaScript engines to perform optimizations: strict mode code can sometimes be made to run faster than identical code that's not strict mode.
3. Prohibits some syntax likely to be defined in future versions of ECMAScript.
[Bron: Strict mode - Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)


## Prototype 

When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.

```
var foo = {
	a: 42
};

// create `bar` and link it to `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;		// "hello world"
bar.a;		// 42 <-- delegated to `foo`
```

Visualize `foo` and `bar` objects and their relationship: 
![alt text](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/fig6.png)

## Polyfilling and Transpiling 

Some of the JS features we've already covered, and certainly many of the features covered in the rest of this series, are newer additions and will not necessarily be available in older browsers.

### Polyfilling

The word "polyfill" is an invented term (by Remy Sharp) (https://remysharp.com/2010/10/08/what-is-a-polyfill) used to refer to taking the definition of a newer feature and producing a piece of code that's equivalent to the behavior, but is able to run in older JS environments.

### Transpiling

The better option is to use a tool that converts your newer code into older code equivalents. This process is commonly called "transpiling," a term for transforming + compiling.

There are several important reasons you should care about transpiling:

- The new syntax added to the language is designed to make your code more readable and maintainable. The older equivalents are often much more convoluted. You should prefer writing newer and cleaner syntax, not only for yourself but for all other members of the development team.
- If you transpile only for older browsers, but serve the new syntax to the newest browsers, you get to take advantage of browser performance optimizations with the new syntax. This also lets browser makers have more real-world code to test their implementations and optimizations on.
- Using the new syntax earlier allows it to be tested more robustly in the real world, which provides earlier feedback to the JavaScript committee (TC39). If issues are found early enough, they can be changed/fixed before those language design mistakes become permanent.
