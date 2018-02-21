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










