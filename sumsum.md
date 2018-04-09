
###  Statement

is a group of words, numbers, and operators that performs a specific task. 

`a = b * 2`

### Expressions 
Statements are made up of one or more expressions. An **expression** is any reference to a variable or value, or a set of variable(s) and value(s) combined with operators.

### Const

In the newest version of JavaScript, a new way to declare constants, by using `const` instead of `var`.

`const TAX_RATE = 0.08;`

Constants are useful just like variables with unchanged values, except that constants also prevent accidentally changing value somewhere else after the initial setting.


### Let

#### Let vs. Var 

`var` uses function scope
`let` uses block scope 
The hoisting process which happens behind the scenes and lets variables be available to a broader scope then where they are actually declared and used makes big code prone to error. The scope can get really confused when you have more than one variable with the same name. 


### Functions As Scopes 
Problems with wrapping a function around a snippet of code.

You have to declare a named function, which means that the identifier name itself pollutes the enclosing scope.
You have to explicitly call the function by name so that the wrapped code actually executes.
--> More ideal if the function didn't need a name, or the name didn't pollute the enclosing scope.


### Immediately Invoked Function Expressions (IIFEs)

An IIFE is a function that runs as soon as it is defined. It contains two major parts:

1. The anonymous function with lexical scope enclosed within the grouping operator (). This prevents accessing variables within the IFFE idom as well as polluting the global scope.

2. The second part is creating the immediately executing function expression (), through which the JavaScript engine will directly interpret the function.


#### You don't need to name a function expression/IIFE, but:

1. Anonymous functions have no useful name to display in stack traces, which can make debugging more difficult.

2. Without a name, if the function needs to refer to itself, for recursion, etc., the deprecated arguments.callee reference is unfortunately required. Another example of needing to self-reference is when an event handler function wants to unbind itself after it fires.

3. Anonymous functions omit a name that is often helpful in providing more readable/understandable code. A descriptive name helps self-document the code in question.
 
### Blockscoping 

Block-scoping is all about declaring variables as close as possible, as local as possible, to where they will be used.

### Hoisting 

A Function Declaration defines a named function variable without requiring variable assignment. 
A Function Expression defines a function as a part of a larger expression syntax (typically a variable assignment).



