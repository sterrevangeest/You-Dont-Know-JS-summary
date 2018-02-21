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



