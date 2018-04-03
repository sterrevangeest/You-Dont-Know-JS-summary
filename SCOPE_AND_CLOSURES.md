# You Don't Know JS: Scope & Closures
## Chapter 1: What is Scope? 

The inclusion of variables into our program begets the most interesting questions we will now address: where do those variables live? In other words, where are they stored? And, most importantly, how does our program find them when it needs them?

## Compiler Theory
Despite the fact that JavaScript falls under the general category of "dynamic" or "interpreted" languages, it is in fact a _compiled_ language. It is not compiled well in advance, as are many traditionally-compiled languages, nor are the results of compilation portable among various distributed systems.

In a traditional compiled-language process, a chunk of source code, your program, will undergo typically three steps _before_ it is executed, roughly called "compilation":

1. **Tokenizing/Lexing**: breaking up a string of characters into meaningful (to the language) chunks, called tokens. 
2. **Parsing**: taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an "AST" (Abstract Syntax Tree
3. **Code-Generation**: he process of taking an AST and turning it into executable code. This part varies greatly depending on the language, the platform it's targeting, etc.

The JavaScript engine is vastly more complex than just those three steps, as are most other language compilers. For instance, in the process of parsing and code-generation, there are certainly steps to optimize the performance of the execution, including collapsing redundant elements, etc.

Let's just say, for simplicity's sake, that any snippet of JavaScript has to be compiled before (usually right before!) it's executed. So, the JS compiler will take the program `var a = 2;` and compile it first, and then be ready to execute it, usually right away

## Understanding Scope
Think of terms of conversations:

The "characters" that interact to process the program `var a = 2`.

1. **The engine**: responsible for start-to-finish compilation execution of our JavaScript program
2. **Compiler**: one of _Engine's_ friends: handles all the dirty work of parsing and code-generation. 
3. **Scope**: another friend of _Engine_; collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

**Nested Scope**

Just as a block or function is nested inside another block or function, scopes are nested inside other scopes.
So, if a variable cannot be found in the immediate scope, _Engine_ consults the next outer containing scope, continuing until found or until the outermost (aka, global) scope has been reached.

The simple rules for traversing nested Scope: Engine starts at the currently executing Scope, looks for the variable there, then if not found, keeps going up one level, and so on. If the outermost global scope is reached, the search stops, whether it finds the variable or not.

### Back & Forth

Statement:
`var a = 2;`

But the Engine sees two distinct statements: 
- one which _compiler_ will handle during compilation
- one which _engine_ will handle during execution 

How _Engine and friends_ will approach the program `var a = 2`; 

1. The _compiler_ : perform lexing to break it down into tokens --> parse into a tree. 
2. The _compiler_ : code-generation. _Compiler_ will proceed as: 
  * Encountering `var a`, _Compiler_ asks _Scope_ to see if a variable `a` already exists for that particular scope collection. 
    - If so _Compiler_ ingnores this declaration and moves on. 
    - Otherwise, _Compiler_ asks _Scope_ to declare a new variable called `a` for that scope collection. 
  * _Compiler_ then produces code for _Engine_ to later execute, to handle the `a = 2` assignments. The code _Engine_ runs will first ask _Scope_ if there is a variable called `a` accessible in the current scope collection. 
    - If so _Engine_ uses that variable
    - If not, _Engine_ looks elsewhere. 
    
If _Engine_ eventually finds a variable, it assigns the value `2` to it. If not, _Engine_ will raise its hand and yell out an error!
To summarize: two distinct actions are taken for a variable assignment: 
First, Compiler declares a variable (if not previously declared in the current scope), and second, when executing, Engine looks up the variable in Scope and assigns to it, if found.

**Compiler Speak**

When _Engine_ executes the code that _Compiler_ produced step (2), it has to look-up the variable `a` to see if it has been declared, and this look-up is consulting _Scope_. 

In this case 

`var a = 2` 

_Engine_ would be performing an Left-hand Side (LHS) look-up for the variable `a`. The other type look-up is called Right-hand Side (RHS).(Side of an assignment operation. )

LHS = when a variable appears on the left-hand side of an assignement operation
RHS = when a variable appears on the right-hand of an assignent operation 

Ask yourself: _"who's the target of the assignment (LHS)"_ and _"who's the source of the assignment (RHS)"._

Why does it matter whether we call it LHS or RHS? Because these two types of look-ups behave differently in the circumstance where the variable has not yet been declared (is not found in any consulted Scope).

- If an RHS look-up fails to ever find a variable, anywhere in the nested Scopes, this results in a ReferenceError being thrown by the Engine. It's important to note that the error is of the type ReferenceError.

- If the Engine is performing an LHS look-up and arrives at the top floor (global Scope) without finding it, and if the program is not running in "Strict Mode" [^note-strictmode], then the global Scope will create a new variable of that name in the global scope, and hand it back to Engine. "Strict Mode" [^note-strictmode], which was added in ES5, has a number of different behaviors from normal/relaxed/lazy mode. One such behavior is that it disallows the automatic/implicit global variable creation. In that case, there would be no global Scope'd variable to hand back from an LHS look-up, and Engine would throw a ReferenceError similarly to the RHS case.

## Chapter 2: Lexical Scope

There are two predominant models for how scope works:

1. Lexical Scope, by far the most common, used by vast majority of programming languages. 
2. Dynamic Scope

### Lex-time

The first traditional phase of a standard language compiler is called **lexing**.  

Lexical scope is scope that is defined at lexing time. In other words, lexical scope is based on where variables and blocks of scope are authored, by you, at write time, and thus is (mostly) set in stone by the time the lexer processes your code.

### Look-ups

**Scope look-up stops once it finds the first match.** The same identifier name can be specified at multiple layers of nested scope, which is called "shadowing" (the inner identifier "shadows" the outer identifier). Regardless of shadowing, scope look-up always starts at the innermost scope being executed at the time, and works its way outward/upward until the first match, and stops.

`window.a`  --> Global variables are also automatically properties of the global object (`window` in browsers, etc.), so it is possible to reference a global variable not directly by its lexical name, but instead indirectly as a property reference of the global object. This technique gives access to a global variable which would otherwise be inaccessible due to it being shadowed. However, non-global shadowed variables cannot be accessed.

### Cheating Lexical

If lexical scope is defined only by where a function is declared, which is entirely an author-time decision, how could there possibly be a way to "modify" (aka, cheat) lexical scope at run-time?

JavaScript has two such mechanisms. Both of them are equally frowned-upon in the wider community as bad practices to use in your code. But the typical arguments against them are often missing the most important point: **cheating lexical scope leads to poorer performance**.

You can cheat lexical-scope with: 
- `eval`: takes a string as an argument, and treats the contents of the string as if it had actually been authored code at that point in the pogram. (you can programmatically generate code inside of your authored code, and run the generated code as if it had been there at author time)
- `with`: short-hand for making multiple property references against an object without repeating the object reference itself each time. 

While the `eval(..)` function can modify existing lexical scope if it takes a string of code with one or more declarations in it, the `with` statement actually creates a whole new lexical scope out of thin air, from the object you pass to it.

If Engine finds an `eval` or `with` in the code, it has to assume that all its awareness of identifier location may be invalid, bc it cannot know at lexing time exactly what code you may pass to `eval` or `with`. Most of the optimizations it would make are pointless if `eval` or `with` are present, so it simply doesn't perform the optimizations at all. --> Your code will run slower. 

SO DON'T USE `EVAL` AND `WITH`. 


## Chapter 3: Function vs. Block Scope

_(Recap) Scope consists of a series of "bubbles" that each act as a container or bucket, in which identifiers (variables, functions) are declared. These bubbles nest neatly inside each other, and this nesting is defined at author-time._

Can other structures in JavaScript create bubbles of scope?

### Scope From Functions 
Each function you declare creates a bubble for itself, but no other structures create their own scope bubbles. (Niet helemaal waar). 

Function scope encourages the idea that all variables belong to the function, and can be used and reused throughout the entirety of the function (and indeed, accessible even to nested scopes).

### Hiding In Plain Scope 

The traditional way of thinking about functions is that you declare a function, and then add code inside it. But the inverse thinking is equally powerful and useful: take any arbitrary section of code you've written, and wrap a function declaration around it, which in effect "hides" the code.

Why would "hiding" variables and functions be a useful technique? (Variety of reasons, arise from the software design principle "Principle of Least Privilege")

You should expose only what is minimally necessary, and "hide" everything else. 
- It keeps private details private.
- It avoids unintended collision between two different identifiers with the same name but different intended usages. 
   * Global "Namespaces":  
   * Module Management: using any of various dependency managers. No libraries ever add any identifiers tot the global    scope, but are instead required to have identifier(s) explicitly imported into another specific scope through usage of the dependency manager's various mechanisms.(????????) They simply use the rules of scoping as explained here to enforce that no identifiers are injected into any shared scope, and are instead kept in private, non-collision-susceptible scopes, which prevents any accidental scope collisions. 

### Functions As Scopes 

Problems with wrapping a function around a snippet of code. 
- You have to declare a named function, which means that the identifier name itself pollutes the enclosing scope. 
- You have to explicitly call the function by name so that the wrapped code actually executes. 

--> More ideal if the function didn't need a name, or the name didn't pollute the enclosing scope. 

```
var a = 2;

(function foo(){ // <-- insert this

	var a = 3;
	console.log( a ); // 3

})(); // <-- and this

console.log( a ); // 2
```

This may seem like a minor detail, it's actually a major change. Instead of treating the function as a standard declaration, the function is treated as a **function-expression** (not a function declaration).

`(function foo(){ .. })` as an expression means the identifier `foo` is found only in the scope where the `..` indicates, not in the outer scope. Hiding the name foo inside itself means it does not pollute the enclosing scope unnecessarily.


#### Differences between Declarations and Expressions




### Invoking Function Expressions Immediately (IIFE) 

An IIFE is a function that runs as soon as it is defined. It contains two major parts: 

1. The anonymous function with lexical scope enclosed within the grouping operator `()`. This prevents accessing variables within the IFFE idom as well as polluting the global scope. 

2. The second part is creating the immediately executing function expression (), through which the JavaScript engine will directly interpret the function.

**You don't need to name a function expression/IIFE, but:**

1. Anonymous functions have no useful name to display in stack traces, which can make debugging more difficult.

2. Without a name, if the function needs to refer to itself, for recursion, etc., the deprecated `arguments.callee` reference is unfortunately required. Another example of needing to self-reference is when an event handler function wants to unbind itself after it fires.

3. Anonymous functions omit a name that is often helpful in providing more readable/understandable code. A descriptive name helps self-document the code in question.

### Blocks As Scopes

Block-scoping is all about declaring variables as close as possible, as local as possible, to where they will be used. 

Like: 
```
for (var i=0; i<10; i++) {
	console.log( i );
}
```
Why pollute the entire scope of a function with the i variable that is only going to be (or only should be, at least) used for the for-loop?

Examle/form of block scope: 

- `with` 
- `try/catch` 
- `let` another way to declare variables. Creates a block-scoped variable.
The `let` keyword attaches the variable declaration to the scope of whaterver block it's contained in. Declarations made with let will not hoist to the entire scope of the block they appear in. Such declarations will not observably "exist" in the block until the declaration statement. 
- `const` also creates a block-scoped variable, but whose value is fixed/constant. Changing the value will give errors.


## Chapter 4: Hoisting

There's a subtle detail of how scope attachment works with declarations that appear in various locations within a scope. 

Recall that the Engine actually will compile your JavaScript code before it interprets it. Part of the compilation phase was to find and associate all declarations with their appropriate scopes.

So, the best way to think about things is that **all declarations**, both variables and functions, are processed first, before any part of your code is executed.

- JS sees `var a = 2;` as two assignments: `var a` and `a = 2`. 
	- the **declaration** `var a` is processed during the **compilation** phase. 
		```
		var a; 
		```
		```
		a =2;
		console.log(a);
		```
	- the **assignment** `a = 2` is left in place for the **execution** phase. 

So, one way of thinking, sort of metaphorically, about this process, is that variable and function declarations are "moved" from where they appear in the flow of the code to the top of the code. This gives rise to the name "Hoisting".

Important to note: 
- hoisting is per-scope. A variable will host to the top of a function. 
- function declarations are hoisted, but function expressions are not. 
- function declarations are hoisted first, then variable declarations/normal variables. 


## Chapter 5: Scope Closure

Closure is all around you in JavaScript, you just have to recognize and embrace it. Closures happen as a result of writing code that relies on lexical scope. They just happen. 

Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.












   
   
   
















  
